

기획 문서

 1차 팀 프로젝트 주제: 금융서비스 기업의 재무 데이터 시각화 도구 및 대시보드


문제점: 

첫번째 프롬프팅

- 재무제표와 esg 데이터를 한 눈에 볼수 있는 웹 사이트를 제작할거야. 시각화 대시보드를 보여주는 사이트인데, 탭은 3개이고, 첫번째 탭은 통합이고, 두번째 탭은 파이낸스이고, 세번째 탭은 esg야. 파이낸스 대표적 기능은 회사명으로 검색하면 기본 10개 회사는 디폴트로 보여주고, 나머지는 검색한 회사의 제무재표(주요 재무비율)를 보여줄꺼야. ESG는 온실가스 배출량, 대기오염 배출량 같은 정량 데이터를 3년 기간으로 연도별 막대 그래프를 보여줄꺼야. 차후 이부분을 데이터 분석을 통해 인공지능을 기반으로 요약해서 메일로 발송해주는 서비스야. 이 사이트는 fastAPI, nextjs, postgresql를 사용하고 있어. 이 사이트의 주요 사용자는 기업 정보에서 배제된 기업 정보에서 소외된 개미 투자자들에게 재무정보를 쉽게 제공하는 사이트야. 이제 이 프로젝트를 3개월 내에 구현하는 개발단계를 말해줘. 여기에 최종 우리 목표는 esg 지수가 해당 회사의 주가의 장기 전망(1년 정도)를 사용자에게 보고서로 제공하는 것이야. 사용자가 예상되는 화면도 보여줘.
#### 개발 방법론
- Agile Scrum
    
#### 기술 스택

- 백엔드 : FastAPI
    
- DB : PostgreSQL, Redis
    
- 프론트엔드 : React 기반의 Next.js
    
- 빌드 시스템 : Docker
    
- 배포 시스템 : Back - [![](https://railway.com/favicon-16x16.png)Railway](https://railway.com/) , Front - [![](https://assets.vercel.com/image/upload/q_auto/front/favicon/vercel/favicon.ico)Vercel: Build and deploy the best web experiences with the Frontend Cloud](https://vercel.com/)
    
- 버전 관리 : Git
    
- 협업 도구 : 슬랙, 지라, 노션
    
- **추가 스택 : 시각화 라이브러리**
    
    - **Plotly/Dash**: 인터랙티브 대시보드 개발
        
    - **Matplotlib/Seaborn**: 기초 시각화 및 그래프 생성
        
    - **D3.js**: 고급 시각화(선택적)
        

#### 아키텍쳐링(구조화 할 때 사용)

1. DDD
    
2. EDA
    


#### 문서화 : (템플릿화)

Jira : 개인 오늘 한 일 간단하게 LOG

옵시디언 : 구체적 요소들 (개인 공부, 추가적으로 생각나는 다양한 내용들 - 공부 + 활용하기 위한 내용들)


엑셀 파일 만든 것을 참고하기 --> 




