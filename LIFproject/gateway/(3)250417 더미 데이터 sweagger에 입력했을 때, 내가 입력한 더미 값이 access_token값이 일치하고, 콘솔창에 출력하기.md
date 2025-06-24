**커서 질문, 더미 토큰을 집어넣어서 sweagger에서 로그인 테스트 하려고, api 폴더 아래에 tokens_router.py 파일을 만들고, domain 폴더 아래에 있는 gateway에 token_controller.py, token_model.py, token_repository.py, token_schema.py, token_service.py 파일을 만들었어. 난 먼저, @token_schema.py 여기에 아래 코드를 내 코드에 맞춰서 구현해줘. 코드는 최대한 간단하게 해줘. 그리고 토큰은 jose가 관리하도록 해줘. 그리고 api밑에 있는 token_router.py가 있으니까 token_router.py 파일을 새로 생성하지 말고, @tokens_router.py 여기에 연결시켜. 

```
# app/domain/gateway/schemas/token_schema.py

from pydantic import BaseModel
from typing import Dict, Any

class TokenSchema(BaseModel):
    token: str

class TokenVerifyResponseSchema(BaseModel):
    message: str
    user: Dict[str, Any]
```


#### 흐름 순서가 아래 처럼 된다.

1. token_scema.py 코드 구현
2. token_model.py 코드 구현
3. token_repository.py 코드 구현
4. token_service.py 코드 구현
5. token_controller.py 코드 구현
6. token_router.py에 연결 시키기(연결시킬 때, token_controller.py와 token_schema.py를 임포트 해야함.)


#### 구현된 파일 요약

1. 토큰 스키마(app/domain/gateway/schemas/token_schema.py):

	- TokenSchema: 클라이언트에서 받은 토큰을 검증하는 간단한 스키마
	
	- TokenResponseSchema: 토큰 발급 응답 스키마
	
	- TokenVerifyResponseSchema: 토큰 검증 결과 스키마
	

2.  토큰 모델(app/domain/gateway/models/token_model.py):

	- 토큰 정보를 저장하는 간단한 모델


 3. 토큰 저장소(app/domain/gateway/repositories/token_repository.py):

	- 메모리 기반의 간단한 토큰 저장소


4. 토큰 서비스(app/domain/gateway/services/token_service.py):

	- Jose 라이브러리를 사용한 JWT 토큰 생성 및 검증
	
	- 더미 토큰 생성 기능 포함



5. 토큰 컨트롤러(app/domain/gateway/controllers/token_controller.py):

	- 서비스를 호출하고 예외 처리


6. 기존 토큰 라우터 확장(app/api/gateway/tokens_router.py):

	- 기존 라우터에 토큰 생성, 검증, 폐기, 더미 토큰 생성 기능 추가



이제 다음 엔드포인트를 통해 토큰 기능을 사용할 수 있습니다:

- /tokens/create - 새 토큰 생성

- /tokens/verify - 토큰 검증

- /tokens/revoke - 토큰 폐기

- /tokens/dummy - 테스트용 더미 토큰 생성
#### token_router.py 코드
```token_router.py

from fastapi import APIRouter, Depends, HTTPException, Request

from typing import Dict, Any, Optional

  

from app.domain.gateway.controllers.token_controller import TokenController

from app.domain.gateway.schemas.token_schema import TokenSchema, TokenResponseSchema, TokenVerifyResponseSchema

  

router = APIRouter(

    prefix="/tokens",

    tags=["tokens"],

    responses={404: {"description": "Not found"}},

)

  

controller = TokenController()

  

@router.post("/")

async def root() -> Dict[str, Any]:

    """

    Tokens 서비스 루트 엔드포인트

    """

    return {"message": "Tokens Service"}

  

@router.post("/create", response_model=TokenResponseSchema)

async def create_token(user_id: str) -> TokenResponseSchema:

    """

    사용자 ID로 새 토큰 생성

    Args:

        user_id: 사용자 ID

    Returns:

        생성된 토큰 정보

    """

    return await controller.create_token(user_id)

  

@router.post("/verify", response_model=TokenVerifyResponseSchema)

async def verify_token(token_data: TokenSchema) -> TokenVerifyResponseSchema:

    """

    토큰 검증

    Args:

        token_data: 검증할 토큰 정보

    Returns:

        토큰 검증 결과

    """

    return await controller.verify_token(token_data)

  

@router.post("/revoke")

async def revoke_token(token_data: TokenSchema) -> Dict[str, Any]:

    """

    토큰 폐기

    Args:

        token_data: 폐기할 토큰 정보

    Returns:

        폐기 결과 메시지

    """

    return await controller.revoke_token(token_data.token)

  

@router.post("/dummy")

async def test_dummy_token(data: str = Body(..., description="아무 값이나 입력해보세요")):

    """

    테스트용 더미 토큰 생성 (Body 파라미터)

    Swagger에서 입력한 문자열을 그대로 토큰으로 사용합니다.

    Args:

        data: 입력 값 (필수, Body 파라미터)

    Returns:

        메시지와 입력 데이터

    """

    print(f"✅ Swagger에서 받은 값:", data)

    # 사용자가 입력한 값을 토큰으로 사용

    return {

        "message": "✅ 콘솔에 출력 완료!",

        "received": data,

        "token_info": {

            "access_token": data,  # 사용자 입력 값을 토큰으로 사용

            "token_type": "bearer",

            "expires_in": 1800

        }

    }
```


#### 목표
그리고 더미 데이터를 사용자가 sweagger에 입력했을 때, 사용자가 입력한 더미 데이터 값이 access_token과 일치해야 하고, 그 값은 콘솔 창에 띄어야 한다. 스웨거는 입력 창이고, 스웨거에 입력을 했을 때, 만약 콘솔 창에 그 값이 띄어지면 도커 컨테이너에 들어온 것을 의미한다. 

###### 목표 요약

- Swagger에서 ** body parameter로 자유롭게 문자열 입력**
    
- FastAPI에서 해당 값을 **받아서 콘솔에 출력**
    
- Swagger 응답은 간단한 메시지만 리턴해도 OK

결과물,,,(아래는 dummy 데이터는 Body 파라미터를 사용하여 테스트 하는 방법.)
![[Pasted image 20250417183050.png]]

#### 메인 라우터에서 중요한 점
- 메인 라우터를 gateway_router로 하기로 하였음
- 메인 라우터에서 gateway_router를 생성하고, 등록하여야 함.
- 메인 라우터에서 서브라우터(서브라우터는 api 밑에 있는 라우터를 말함)를 등록해야 함.
- 
- 이미지를 참고하기(메인 라우터 생성 및 등록)
- ![[Pasted image 20250417184945.png]]
- ![[Pasted image 20250417185020.png]]
- 
- 이미지를 참고하기(메인 라우터에서 서브라우터 등록)
- ![[Pasted image 20250417185107.png]]


#### prefix 관련 주의 사항
-  gateway_router는 /e 로 하기로 함
-  tokens_router.py에서 @router.post("/") 이 말은 즉---> /e/tokens 여기에 와있다는 말이다.
-  login_router.py에서 @router.post("/") 이 말은 즉 ---> /e/login 여기에 와있다는 말이다.
-  finance_router.py에서 @router.post("/") 이 말은 즉 ---> /e/finance 여기에 와있다는 말이다.
-  esg_router.py에서 @router.post("/") 이 말은 즉 ---> /e/esg 여기에 와있다는 말이다.
- 
- 예시) tokens_router.py에 들어왔다는 것은 아래 사진을 참조해라
- ![[Pasted image 20250417183715.png]]



#### docker 명령어 관련

1. 코드 간단히 수정한 경우 --> docker compose up
2. 백엔드 파일을 크게 오류 수정해서 기존의 백엔드 컨테이너를 삭제하고 새로운 백엔드 컨테이너를 생성해서 도커 컨테이너에 다시 올리는 경우 --> docker compose down && docker compose up --build -d (오류를 수정해야할 경우, 다운을 하고 다시 컨테이너에 빌드해 놓는다. 빌드 한다는 것은 도커에 올린다는 뜻이다.)
3. 오류를 확인하고 싶은 경우 --> docker-compose logs --tail=1000 api
	1. 즉, api는 백엔드 컨테이너의 윗 부분을 말하는 것이다. 
	2. 아래 사진 참조해라. (api는 gateway의 백엔드 컨테이너 부분의 상단 위의 이름이다.)
	3. ![[Pasted image 20250417190712.png]]
4. DB 컨테이너 안으로 들어가고 싶을 경우 --> docker exec -it gateway-db psql -U postgres -d postgres
5. 백엔드 컨테이너 안으로 들어가고 싶은 경우 --> docker exec -it gateway-service bash
6. 결국, 도커 컨테이너에 올리는 것은 DB 컨테이너와 백엔드 컨테이너를 올리는 것이다.
7. 우리가 DB에 테이블을 생성해서 주입하고 싶은 경우
	1. docker exec -it gateway-db psql -U postgres -d postgres을 사용(즉, 이 말은 DB에 직접 주입하겠다라는 의미)
	2. \dt
	3. 테이블 생성(ddl.sql에 들어 있는 값 넣기 )
	4. 테이블 안에 users 넣기(dml.sql에 들어있는 값 넣기)
	5. 테이블 조회(dql.sql에 있는 명령어 쓰기)

8. docker compose up --build -d (도커 컨테이너에 DB 컨테이너이든, 백엔드 컨테이너이든 올린다는 의미)