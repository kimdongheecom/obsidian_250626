
esg/  
├── app/  
│ ├── __init__.py  
│ ├── main.py  
├── .env  
├── requirements.txt  
├── Dockerfile  
├── docker-compose.yml  
└── README.md  
  
**이 구조로 만들어주고 docker-compose.yml을 통해 fastapi를 실행할 수 있도록 해줘. 도커file에서 파이썬버전은 3.12.7이고 reload옵션을 추가해줬으면 해. 그다음에 port는 9001으로 해줘. 도메인 주소는 www.kimdonghee.com 이야. APIRouter 생성자의 prefix는 /esg으로 하고, async 방식으로 작동되게 해줘. requirements.txt에 기본적으로 등록된 라이브러리는 다음과 같다.** fastapi uvicorn asyncpg sqlalchemy python-dotenv pytz email_validator passlib[bcrypt]==1.7.4 shortuuid==1.0.13 python-jose[cryptography] redis==5.2.1 aiohttp==3.8.5 bs4==0.0.1 beautifulsoup4==4.12.2 requests==2.31.0
psycopg2-binary==2.9.7 selenium==4.11.0 webdriver-manager==3.8.6 그리고 최종적으로 swagger에서 ("/")하면 Welcome Message가 뜰 수 있게 해줘. app.include_router(esg_router)처럼 app이 esg_router를 포함하게 해줘.**
**그리고 docker-compose.yml을 만들 때, 아래와 같은 형식을 붙이고, 내 코드에 맞게 변환해줘. (도커 컨테이너에 올릴 백엔드 컨테이너, DB 컨테이너 생성하기)**


###### 최종 프롬프팅 문장
"FastAPI 기반으로 비동기 ESG 데이터 수집 백엔드를 개발 중입니다. 사용자가 'LG에너지솔루션' 같은 기업명을 입력하면:

1. 웹사이트에서 지속가능경영보고서(PDF)를 Selenium으로 비동기 크롤링하여 다운로드하고,
2. `pdfplumber`로 온실가스 배출량 등 ESG 정량지표를 파싱한 뒤,
3. PostgreSQL에 저장하고,
4. Swagger에서 `/esg/{company}`로 이 전 과정을 실행할 수 있게 구성합니다.

전체 코드는 다음 구조를 따릅니다:

```
esg/
├── app/
│   ├── __init__.py               # Python 패키지 초기화 파일
│   └── main.py                   # FastAPI 진입점 (Swagger에서 "/" 접속 시 Welcome 메시지 출력)
├── .env                          # 환경변수 파일 (예: DB_URL, DOMAIN 등)
├── requirements.txt              # 라이브러리 목록 (FastAPI, selenium 등 포함)
├── Dockerfile                    # Python 3.12.7, uvicorn --reload 포함
├── docker-compose.yml           # FastAPI 서비스 정의 (포트: 9001, 도메인: www.kimdonghee.com)
└── README.md                     # 프로젝트 설명 파일

```


FastAPI는 `async def`로 비동기 방식으로 구성되어 있으며, 모든 API 라우터는 `APIRouter(prefix="/esg")`로 설정되어 있습니다. `pdfplumber`, `selenium` 등 동기 라이브러리는 `run_in_threadpool()`로 감싸 비동기화합니다. FastAPI는 Swagger UI에서 테스트 가능하며, 메인 엔드포인트 `/`에서는 'Welcome Message'가 출력되도록 설정되어 있습니다. 이 모든 환경은 Docker + docker-compose 기반으로 구성됩니다."


이 구조를 바탕으로 실제 파일도 생성하기

- ✅ `main.py`: Swagger에서 `/` 접속 시 환영 메시지 출력
    
- ✅ `Dockerfile`: reload 포함
    
- ✅ `docker-compose.yml`: 포트 `9001` 바인딩
    
- ✅ `.env`: 도메인 등 설정
    
- ✅ `requirements.txt`: 요청하신 모든 패키지 포함


##### api호출 및 크롤링 : 데이터를 수집하는 방법

공통점: **URL**을 가지고 있다.

###### api 호출
- **백엔드(서버)** 에서 데이터를 가져오는 것을 의미한다.
- 예를 들어, 구글 api 호출이라는 의미는 구글 서버에서 데이터를 가져오는 것을 의미한다. 이때 **키**를 발급받아야 한다. 왜냐하면 구글 서버는 보호되어있으니까.

###### 크롤링
- 프론트엔드(html)에서 데이터를 가져오는 것을 의미한다.
- 예를 들어, 우리는 html을 보여주고 데이터를 수집(크롤링)해왔다. 프론트엔드는 누구나 볼 수 있고, 가져올 수 있다. 키가 필요 없다. 

