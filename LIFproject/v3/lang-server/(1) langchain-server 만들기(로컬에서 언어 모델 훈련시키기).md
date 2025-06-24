
#### LangGraph를 만들기 위해 아래와 같은 조건을 만족 시켜야 한다.
1. 챗봇 서비스 만들기
2. 에이전트 만들기
3. RAG 사용하기
![[Pasted image 20250509124120.png]]


참고 사이트: https://python.langchain.com/docs/tutorials/



1. KoELECTRA(API) -> 대화 생성 못하는 모델 
2. KoGPT2(API) -> fine-tuned 안되어 있음 
	1. 답변수준: { "response": "너 뭐하는중이니?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그게 무슨 소리야?\"\n\"그럼, 그", "status": "success" }
3. lcw99/ko-dialoGPT-korean-chit-chat(API) 성공. 그런데 대화수준이 너무 낮음

langchain은 아래부터

4. llama-2-7b-chat.Q4_K_M.gguf(파일다운로드) 답변을 영어로만 함
	1. 난 **llama-2-7b-chat.Q4_K_M.gguf** 이 모델로 선택함. 이 모델은 허깅페이스에서 모델 다운 받음
5. beomi/llama-2-ko-7b(파일다운로드) 시도중

(프롬프팅 순서 확인하기)

- fastAPI기반 langchain 프레임워크로 챗봇, RAG, 에이전트 서버를 제작하려고 해. 그런데 CPU로만 구현해야해. 모델 생성후, 간단한 개인화 챗봇 서비스를 제작하는 프로젝트 생성과정을 알려줘. 화면은 신경쓰지 않고, api로 제공되도록 해줘. LLaMA를 활용하는 프로젝트로 사용하고. 도커를 사용할꺼야.
- fastAPI + LangChin + llama.cpp기반 LLM RAG 챗봇 서버로 구성됨.
- 허깅페이스에 등록된 langchain 모델 중에서 cpu-only , 가볍고, 빠른 것 위주로  로컬에서 개발 가능한 모델로 파인 튜닝이 잘 되는 것을 추천해줘.
- **fastAPI기반에서 랭체인 프레임워크에서 tinyllam로 간단한 개인화된 챗봇 개발을 하고자 한다. 현재 fastAPI에 도커로 기본 설정만 되어있어. db는 필요없고, RAG가 가능한 프로그램을 작성하는 개발 순서를 알려줘.**
##### 추가로 변경된 부분
1. Dockerfile에서는 기존 CMD 부분을 제거했습니다.
	1. 이렇게 하면 컨테이너 실행 명령이 docker-compose.yml에서 관리되므로 더 유연하게 설정을 변경할 수 있습니다.

##### langchain-service 관련 라이브러리 설치하기
1. C++ 컴파일러를 Windows에 설치하는 절차를 안내해 드리겠습니다. **Visual Studio Build Tools**를 **설치**하면 **llama-cpp-python**을 빌드하는 데 필요한 컴파일러를 얻을 수 있습니다.
2. **langchain**도 성공적으로 설치
3. **langchain_community** 패키지를 설치



