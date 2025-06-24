
(프롬프트)

1. 먼저 app에 있는 page.tsx부터 생성한다.(page는 )

```
"use client";

import React, { useState } from 'react';

import Sidebar from '@/components/Common/Sidebar';

  
export default function DoubleMateriality(){


  return (

    <div className="min-h-screen bg-white">

      <div className="container mx-auto px-4 py-8">

        <h1 className="text-3xl font-bold text-center mb-10 text-blue-600">Sustainability Report</h1>

  

        <div className="flex flex-col lg:flex-row gap-6">

          <Sidebar/>

          {/* 메인 컨텐츠 영역 */}

          <div className="w-full lg:w-3/4">

            <h1>이중중대성 평가</h1>

          </div>

        </div>

      </div>

      <footer className="bg-gray-800 text-white py-4 mt-8">

        <div className="container mx-auto px-4 text-center">

          <p>© 2024 ESG Report. All rights reserved.</p>

        </div>

      </footer>

    </div>

  );

};
```


2.   @Sidebar.tsx 여기에서 <li><a href="#esg-vision-strategy" className="block py-1 text-gray-600 hover:text-blue-500">ESG 비전 및 전략</a></li>이것을 클릭하면 @page.tsx 여기로 이동하도록 해줘.


PRD - 기획 보고서, 요수사항 보고서를 총칭함


(배경지식)
기획 --> 개발/디자인(UI/UX --> PD) --> 테스트 하고 운영팀한테 넘김(QA/QC) --> 배포



PRD를 task로 쪼개야 함.

PRD와 task를 같이 던져줘야 함.



MCP --> 


#### PRD를 만드는 방법

구글 제미나이 --> 모자라다가 --> 2.5  

구글 io --> 

퍼스널 랜딩페이지를 위한 세계적인 수준의 PRD를 만들려고 하는데, product owner로써 어떤게 필요해?
가장 보편적인 것으로 추천해서 입력해주고, nextjs와 supabase를 사용하도록 할게.





@prd.md 이 문서를 task master mcp의 parse-prd를 사용하고 @example_prd.txt를 참고해서 script/prd.txt로 파싱해줘.


task api key --> kim donghee-onboarding-api-key



task로 쪼개기

tast-master mcp를 dl용해서 현재 task 중에 복잡도가 높은 걸 subtask로 확장해줘.


task로 쪼개고, subtask로 쪼개고....



smithery ai

ai agent


다른 툴과 연결시킬 수 있는 표준화 된...

테스크 마스터에 있는 툴을 이용해서 그 툴을 쓰는 주체는 커서가 된다. 

커서는 ai -agent이다.

테스크 마스터는 툴이다.

클로드에 


테스크 마스터가 클러드를  태스트 기능을 만들어준다. 


#### 차이점
- llm --> 추론이 가능하지 않음(뇌), 학습이 된 대학생....

- ai agent --> 추론이 가능함, 예) 커서... 특화된 


필요한 툴은 ncp를 통해 연결할 수 있다. 

a2a 에이전트에게 너가 없으니.. 툴으

https://www.task-master.dev/

https://www.anthropic.com/api

https://console.anthropic.com/dashboard

https://n8n.io/

https://github.com/microsoft/playwright-mcp  

PRD를 만들고, 이것을 task로 쪼개고, subtask



n8n........


notebook lm 사이트에서

#### 지속가능경영보고서를 수집해와줘.
#### 이런 보고서에 맞는 더미데이터를 만들껀데, 프롬프트를 만들어줘.....




CONTEXT 7 MCP SERVER 깃허브에서 리드미. 복붙하면 끝. mcp.json/ anthropic api key: 이것만 놔두면 된다. 리드미 옵션2: 설치 -> 이게 더 편함 커서 세팅 .env에 ANTHROPIC_API_KEY 넣기 PRD.md 파일에 만든 PRD 넣기. 커서에게 넣으며, "이 문서를 task.master.mcp의 parse-prd를 사용해서 @example_prd.t txt" 참고헤서 script/prd.txt로 파싱해줘" 태스크, 서브테스크 쪼개기 테스크 우선순위 물어보기 커서에 골뱅이, 오토 빼기, 셋팅에 안물어보고 알아서 하게하는거 있음. task master list llm과 에이전트의 차이: 추론 가능 유무 llm은 학습된 기능만 한다. 코드에 특화된 에이전트(추론기능 추가): 커서 agent인 커서에게 무슨 툴을 사용해서 코드를 짤 수 있지? 선택하게 함. task master는 툴. api key로 연결해 사용 mcp는 표준화된 가교 역할 -> A2A: Playwright: 브라우저 뿅 띄우고 스크린샷 찍여서 보여줌. --- smithery.ai: mcp 모여있다. n8n.io: 노코드로 백엔드 만드는 서비스 - 유튜브 노코드캣 라이브래그? 래그? rag? 데이터 끌어오는 방법론 찾아봐라. notebookLM: discover 버튼 - ~에 대한 샘플을 수집해줘. 영어로. import. -> 챗: 이런 걸 만들건데, 이거 만들게 할수이쓴 프롬프트 만들어줘 할수잇스.ㅁ 딴대다닥. 데이터 분석 핫제이아이. 구글애널리틱스. 고객 리텐션. 가설 이런 기능때멩 올거다. 장치를 심어둔다. 버튼 눌렀는지. 읽다 팅겨나갔는지. 분석해야함. prd(프로덕트 만들기 위해 필요한 것 정리한 문서. 기획부터~. ) PD: 프로덕트 디자이너 풀스텍을 넘어 PD + 알파 가능해야 한다. 기획 -> 개발, 디자인 -> 운영(QA/QC) -> 배포 * 디자인을 요청할 때, 단순 지시뿐 아닐 목적, 기획 의도를 설명해줘야 함. 즉, context를 줘야한다. prd + task 함께 커서에게 요청 MCP: API와 같은 기능. But, API를 위한 코드 셋팅 필요 없음. - 지피티: 범용적 무난 - 클러드: 코딩, 글쓰기 잘함 - 제미나이: 2.5가 3월에 나오면서 뒤집음. google io. -> 질문: 더 발전된 기능 나오면 다양한거 써보는 거 좋다고 추천해주셨자나요. 저는 주로 지피티랑 클러드를 쓰는데, 지피티는 그동안 제가 프로젝트 기획하고 그 내용을 넣어놨으면 그걸 기억한다. 그 외에도 내가 원하는 방식 계속 기억.. ex. 퍼스널 랜딩페이지를 위한 세계적인 수준의 prd 를 만들려고해.




#### mcp key
[YOUR_ANTHROPIC_API_KEY_HERE]




