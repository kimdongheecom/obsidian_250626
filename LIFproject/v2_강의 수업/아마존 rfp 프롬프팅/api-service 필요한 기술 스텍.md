
1. "**FastAPI로 Gateway용 main.py를 만들어줘.**

- **/e/fin/{path}**, **/e/stock/{path}**, **/e/esg/{path}** 3개 엔드포인트를 만들고
    
- 각 엔드포인트는 **GET, POST, PUT, DELETE** 메서드를 모두 지원해줘.
    
- 요청을 프록시로 전달할 때는 공통 함수 `proxy_to_service()`를 만들어서 요청을 처리해줘.
    
- 프록시로 전달하기 전에 **headers에서 'host'와 'content-length'는 제거**하고 보내줘.
    
- 요청 실패하면 타임아웃, 연결실패, 500 에러 등을 구분해서 예외처리 해줘.
    
- Gateway 서비스가 시작될 때와 종료될 때는 로깅을 해줘.
    
- 요청 소요 시간도 로깅해줘.
    
- 최종 코드는 전체 다 보여줘. 파일 쪼개지 말고 한 파일에 다 넣어줘.**
    

**Docker로 배포할 거니까 app/main.py 기준으로 작성해줘.**