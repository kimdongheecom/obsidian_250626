

1. (아래 사진 첨부하면서)내가 이 프로젝트는 msa 패턴으로 fastAPI를 사용하고 있어. 그리고 DB는 postgresql을 사용하고 있어. 그리고 도커 사용해. 테이블 명은 users를 사용해. 이 프로젝트에서 만드는 역할은 auth-service와 gateway_service를 만들꺼야. 그러면 백엔드에서 auth_service와 gateway_service 둘 중 뭐부터 만들어야해? 

 ![[Pasted image 20250415121457.png]]


3. (아래 사진 첨부하면서)나 여기까지 만들었고, 나 뭐해야해?

 ![[Pasted image 20250415121535.png]]



3. ![[Pasted image 20250415124906.png]]
나 도커먼저 할껀데, 데이터베이스 시스템은 postgresql이고, 테이블 명은 users야. 테이블 컬럼은 이미지를 참고해줘.



#### 백엔드
- 프레임워크 만들기


**커서 질문,**
melon/  
├── app/  
│ ├── __init__.py  
│ ├── main.py  
├── .env  
├── requirements.txt  
├── Dockerfile  
├── docker-compose.yml  
└── README.md  
  
**이 구조로 만들어주고 docker-compose.yml을 통해 fastapi를 실행할 수 있도록 해줘. 도커file에서 파이썬버전은 3.12.7이고 reload옵션을 추가해줬으면 해. 그다음에 port는 8000으로 해줘. APIRouter 생성자의 prefix는 /users로 하고, async방식으로 작동되게 해줘. requirements.txt에 기본적으로 등록된 라이브러리는 다음과 같다.** 
**fastapi uvicorn asyncpg sqlalchemy python-dotenv pytz email_validator passlib[bcrypt]==1.7.4 shortuuid==1.0.13 python-jose[cryptography] redis==5.2.1 그리고 최종적으로 swagger에서 ("/")하면 Welcome Message가 뜰 수 있게 해줘. app.include_router(api_router)처럼 app이 api_router를 포함하게 해줘.**


- 도커 컨테이너에 DB 테이블 생성하기 - 더미값 집어 넣기
- 웹 서버 만들기
- sweagger로 테스트 하기