
1. 프레임워크 만들기(빈 깡통부터 만든다) -- 백엔드는 도메인 주도로 만들고, 프론트엔드는 이벤트 주도로 만든다. 도메인에서 api는 이벤트 주도로, domian은 도메인 주도로 아키텍쳐링한다는 것을 명심해야 한다. 

	1. (아래 사진 첨부하면서)내가 이 프로젝트는 msa 패턴으로 fastAPI를 사용하고 있어. 그리고 DB는 postgresql을 사용하고 있어. 그리고 도커 사용해. 테이블 명은 users를 사용해. 이 프로젝트에서 만드는 역할은 auth-service와 gateway_service를 만들꺼야. 그러면 백엔드에서 auth_service와 gateway_service 둘 중 뭐부터 만들어야해? ![[Pasted image 20250415182731.png]]
	2. (아래 사진 첨부하면서)나 여기까지 만들었고, 나 뭐해야해? --> 도커 세팅, 설정 완료 후 DB랑 연결되어있는지 확인하기. postgresql 컨테이너가 있어야 우리가 user_model을 담을 수 있다. 
		1. ![[Pasted image 20250415182809.png]]
 2.  Docker 세팅하기(코딩하기 전에 docker부터 세팅해야함.. 그리고 user 테이블 만들기..user 테이블 만드는 것은 model을 만들라는 소리임....)

	 1.  PostgreSQL 서비스 정의(DB 관리 시스템 서비스 정의..?) 즉,,,docker-compose.yml을 작성한다는 의미이다. posegresql서비스 정의는 docker-compose.yml 파일에서 postgresql을 하나의 독립된 컨테이너로 실행시키기 위한 설정을 말한다. 즉 postgresql이라는 데이터베이스 서버를 도커로 띄울 준비를 한다. 
		 1. ### 즉, "PostgreSQL 서비스 정의"란?
		 - Docker가 PostgreSQL 서버를 설치하고
		 - `myuser` / `mypass` 계정으로
		 - `mydb`라는 이름의 데이터베이스를 생성하고
		 - FastAPI가 여기에 접근할 수 있도록 포트를 열어주는 것을 의미한다.
		 
	 2. `auth` 컨테이너에서 연결되도록 `docker-compose.yml` 작성 --> 맨 위에 버전 삭제하기 
	 3. `.env`에 `DATABASE_URL` 추가(**DATABASE_URL=postgresql+asyncpg://myuser:mypass@db:5432/mydb**)
		 1.  FastAPI 앱(`auth` 컨테이너)이 `.env`의 `DATABASE_URL`을 통해 PostgreSQL에 접속할 수 있게 돼요:
		 ```.env
		 DATABASE_URL=postgresql+asyncpg://myuser:mypass@db:5432/mydb
		 ```

	 4. Dockerfile 만들기(docker에 담을 형식을 알려주는 곳...형식에 맞게 도커에 올려야 하기 때문이다.)
	 5. requirements.txt 만들기
	 6. 테이블 모델 생성하기.. [여기서 "나는 로그인 페이지를 구현을 할꺼야. 테이블 명은 users, admins, subscribers이야. 어떻게 아키텍쳐링을 구성하면 될까?"라고 쳇 지피티에 물어봐야 함. (아래의 이미지 두개를 참조해서..내 폴더 구조와 users테이블의 컬럼도 줘야함.. 컬럼은 유저스 테이블 밑에 있는 id, username 등등 포함한다.)]
	![[Pasted image 20250415195225.png]]![[Pasted image 20250415195303.png]]



참고사항
![[Pasted image 20250415195550.png]]



3. DB연결 설정 및 세션 처리 
	1. database.py 파일 추가하기
	2. PostgreSQL 연결 설정 (`.env` 활용) -- 코딩은 아래처럼


```
# database.py
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker
import os

DATABASE_URL = os.getenv("DATABASE_URL", "postgresql+asyncpg://user:pass@host/db")

engine = create_async_engine(DATABASE_URL, echo=True)
AsyncSessionLocal = sessionmaker(bind=engine, class_=AsyncSession, expire_on_commit=False)

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session
        
```

4.  DB 붙었는지 확인하기(굳이 이작업을 했었나? 잘 모르겠다.)
	1. FastAPI 서버 띄우고 `/health` 확인
	2. SQLAlchemy로 연결 테스트


여기서 database와 연결됬는지 확인하기 위해 더미데이터를 user01, user02를 직접 넣었던 것 같다. ddl.dml.dql? sql을 넣었던 것 같다. 

#### 로그인 + 회원가입 로드맵 시작

참고부분....사실은 폴더 구조 보여주고, 이 다음에 뭐해야하는지 쳇지피티한테 물어봐도 알려줄 것이다. 
![[Pasted image 20250415192520.png]]

그래서 아래와 같은 로드맵을 알려주었다. 쳇 지피티가... 이 부분 그냥 정리한 것이다. 

3.  DB 모델 정의 (users부터)
	1. domain/auth/models/user_model.py에다가 아래와 같은 코딩 구현(이 코드를 그대로 복사한 것보다 이 코드를 커서에 붙이고,  커서에 내 코드에 맞게 구현해달라고 해야함.)
```
from sqlalchemy import Column, Integer, String, TIMESTAMP, func
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    username = Column(String(100), nullable=False, unique=True)
    email = Column(String(100), nullable=False, unique=True)
    password_hash = Column(String, nullable=False)
    role = Column(String(50), default="user")
    created_at = Column(TIMESTAMP(timezone=True), server_default=func.now())

```

4. Pydantic 스키마 정의
	1. domain/auth/schemas/auth_schema.py에다가 아래와 같은 코딩 구현
```
from pydantic import BaseModel, EmailStr

class LoginRequest(BaseModel):
    email: EmailStr
    password: str

class SignupRequest(BaseModel):
    username: str
    email: EmailStr
    password: str

class AuthResponse(BaseModel):
    access_token: str
    token_type: str = "bearer"

```

5. 레포지토리 구현
	1. domain/auth/repositories/auth_repository.py에다가 아래와 같은 코딩 구현
```
from sqlalchemy.future import select
from domain.auth.models.user_model import User

class AuthRepository:
    async def get_user_by_email(self, db, email: str):
        result = await db.execute(select(User).where(User.email == email))
        return result.scalar_one_or_none()

    async def create_user(self, db, user_data: dict):
        user = User(**user_data)
        db.add(user)
        await db.commit()
        await db.refresh(user)
        return user
```


6. 서비스 구현
	1. domain/auth/services/auth_service.py에 아래와 같은 코딩 구현하기. 근데 service에서 token을 발급받는데, jose를 사용한다는 것을 말해줘야 한다. 커서에 이러한 로직을 주면서 jose가 관리할 수 있도록 코드를 작성해달라고 해야한다. 
```
from domain.auth.repositories.auth_repository import AuthRepository
from passlib.hash import bcrypt
from utils.jwt import create_access_token  # 다음 단계에서 생성 예정

class AuthService:
    async def signup(self, db, payload):
        hashed_pw = bcrypt.hash(payload.password)
        user_data = {
            "username": payload.username,
            "email": payload.email,
            "password_hash": hashed_pw,
            "role": "user"
        }
        return await AuthRepository().create_user(db, user_data)

    async def login(self, db, payload):
        user = await AuthRepository().get_user_by_email(db, payload.email)
        if not user or not bcrypt.verify(payload.password, user.password_hash):
            return None
        token = create_access_token({"sub": str(user.id), "role": user.role})
        return token


```


그리고`passlib[bcrypt]` 설치하기
```
pip install passlib[bcrypt]
```

7. 컨트롤러 구현
	1. domain/auth/controllers/auth_controller.py에 아래와 같은 코딩 구현
```
from domain.auth.services.auth_service import AuthService

class AuthController:
    async def login(self, db, payload):
        return await AuthService().login(db, payload)

    async def signup(self, db, payload):
        return await AuthService().signup(db, payload)

```

그리고 잠깐,,, 백엔드는 도메인 주도로 아키텍쳐링하고, 프론트엔드는 이벤트 헨들러로 아키텍쳐링한다. 그리고 도메인에서 api는 이벤트 주도로 하고, domain은 도메인 주도로 아키텍쳐링한다. 

8. 라우팅 설정
	1. app/api/auth/users_router.py에 아래와 같은 코딩을 구현한다. 
```
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession
from domain.auth.schemas.auth_schema import LoginRequest, SignupRequest, AuthResponse
from api.auth.auth_event import LoginEvent, SignupEvent
from infrastructure.database.db import get_db

router = APIRouter()

@router.post("/login", response_model=AuthResponse)
async def login(payload: LoginRequest, db: AsyncSession = Depends(get_db)):
    token = await LoginEvent(payload, db).dispatch()
    if not token:
        raise HTTPException(status_code=401, detail="Invalid credentials")
    return {"access_token": token}

@router.post("/signup")
async def signup(payload: SignupRequest, db: AsyncSession = Depends(get_db)):
    user = await SignupEvent(payload, db).dispatch()
    return {"message": "User created", "user_id": user.id}

```

9. JWT 유틸 생성
	1. utils/jwt.py에 아래와 같은 코드 구현
	2. 
```
from jose import jwt 
from datetime import datetime, timedelta
from typing import Dict

SECRET_KEY = "your-secret-key"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

def create_access_token(data: Dict) -> str:
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

```

그리고 우리는 python-jose 사용 (FastAPI 공식 추천)해야한다.

```
pip uninstall jwt
pip install python-jose[cryptography]
```

그리고 도커 사용중이니까 아래와 같은 명령어 해주기.
```
docker-compose up --build
```


10. 마지막, sweagger로 테스트 하기([http://localhost:8888/docs](http://localhost:8888/docs#/)) --> 회원가입 작성 페이지에서 회원가입 하고, 로그인 페이지에서 로그인 성공하면 성공이다!

- 아래는 회원가입 작성 페이지
![[Pasted image 20250415191446.png]]

- 아래는 로그인 요청 페이지
![[Pasted image 20250415191516.png]]

로그인 했을 때, 성공했다. --> 로그인 성공!!!

- 여기서 주의할 점은 내가 kimdonghee라는 이메일로 회원가입 했을 경우에 로그인 성공이 가능하다. 이전에 우리가 더미 데이터 user01, user02를 사용해서 작성을 했는데, 나중에 서비스에서 토큰을 발급 받았을 때 주입식 콘솔창에 직접 데이터를 주입하면 안된다. 그리고 우리는 hashed_password를 했기 때문에... 로그인 실패로 계속 뜰것이다. 그렇다면,,,,우리가 스웨거에서 회원가입 작성 페이지를 통해 회원가입 진행하고 로그인을 요청했을때만 **성공**한다. 

전체 구현 순서 (Step 1 ~ Step 8)

- 아래와 같이 코딩을 구현하지만, 이벤트 핸들러는 구현하지 않기. 백엔드에서는 이벤트 주도성으로 안한다고 생각하면 됨.
![[Pasted image 20250415190033.png]] 



requirements.txt,docker-compose.yml, README.md를 만들어주고, docker-compose.yml을 만들 때, 아래와 같은 형식을 붙이고, 내 코드에 맞게 변환해줘. (도커 컨테이너에 올릴 백엔드 컨테이너를 생성하고, 반면에 DB 컨테이너는 생성하지 않을꺼야.)


