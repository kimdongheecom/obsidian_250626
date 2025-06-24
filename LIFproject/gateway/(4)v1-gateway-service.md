

#### 프로젝트 구조
```
gateway/
├── app/
│   ├── __init__.py                  # 빈 파일 (코드 작성 X)
│   ├── main.py                      # FastAPI 진입점
├── .env                             # 환경 변수 설정
├── requirements.txt                 # 의존성 목록
├── Dockerfile                       # Python 3.12.7 + reload 설정
├── docker-compose.yml              # FastAPI 컨테이너 실행 정의
└── README.md

```

- 도메인: `www.jinmini.com`
    
- API 기본 prefix: `/e`
    
- Swagger 루트("/")에서 "Welcome Message" 출력

#### 실행 명령어

```
# 최초 실행
docker-compose up --build -d

# 컨테이너 존재 시
docker-compose up -d

```


#### requirements.txt 주요 라이브러리

```
fastapi
uvicorn
asyncpg
sqlalchemy
python-dotenv
pytz
email_validator
passlib[bcrypt]==1.7.4
shortuuid==1.0.13
python-jose[cryptography]
redis==5.2.1

```

이어서 다시하기 gateway프로젝트 설정에 있음(auth와 gateway)