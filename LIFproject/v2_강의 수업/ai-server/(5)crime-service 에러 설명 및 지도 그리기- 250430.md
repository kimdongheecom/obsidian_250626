

#### 스웨거 숫자 에러 정리(백엔드와 프론트엔드의 )

- 200  - 성공

- 400 - 프론트 엔드 에러
	
	- 예시) 404: 라우팅이 틀렸다. 엔드포인트가 틀렸다. 그 경로에 갔더니 타입이 없다.

- 500 - 백엔드 에러
	- 예시) 500 에러이면 마이크로 서비스의 컨트롤러와 서비스의 문제임을 알 수 있다.


```
(3단계) - 오류에 대한 캐쉬 아예 삭제
docker-compose down  
모든 관련 이미지를삭제:
docker rmi $(docker images 'ai-server*' -q)  
Docker 캐시 정리  
docker system prune -a 
다시 빌드 
docker-compose build --no-cache  
컴포즈업 
docker-compose up


(2단계 - 오류 수정하고나서 다시 재 빌드할 때)
# 1. 컨테이너 중지
docker-compose down

# 2. crime-service만 재빌드
docker-compose build --no-cache crime-service

# 3. 서비스 재시작
docker-compose up -d

(1단계 - 코드 잠깐의 수정)
docker compose restart
```