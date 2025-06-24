
#### overview 
  
  아래는 이미지에 보이는 항목들을 **영문 폴더명으로 추천**한 것입니다. 가독성과 유지보수성을 고려해 **카멜 표기법(camelCase)** 또는 **케밥 표기법(kebab-case)**을 함께 제시하겠습니다:

|항목 이름 (한글)|의미|폴더명 추천 (camelCase)|폴더명 추천 (kebab-case)|
|---|---|---|---|
|회사소개|회사의 일반 개요|`companyIntro`|`company-intro`|
|주요 연혁|회사의 연도별 주요 이력|`history`|`history`|
|사업 소개|회사가 하는 주요 사업|`businessOverview`|`business-overview`|
|2023년 주요 성과|작년 주요 성과|`highlights2023`|`highlights-2023`|
|비전 및 전략|향후 목표 및 추진 전략|`visionStrategy`|`vision-strategy`|
|CEO Message|CEO의 인사말|`ceoMessage`|`ceo-message`|

---

💡 **추가 팁**:

- 전체를 감싸는 폴더를 `overview` 또는 `about`으로 설정하면 좋습니다.
    
    ```
    └── overview/
        ├── ceoMessage/
        ├── companyIntro/
        ├── history/
        ├── businessOverview/
        ├── highlights2023/
        └── visionStrategy/
    ```
    

원하는 스타일(예: camelCase vs kebab-case)이 있으면 그에 맞춰 전체 구조도 다시 정리해드릴 수 있습니다.


#### ESG storybook

좋습니다! 이번에는 **"ESG Storybook"** 하위 항목들을 프론트엔드 폴더 구조로 구성하기 위한 **영문 폴더명 추천**을 드립니다. 유지보수에 유리하도록 영어명은 의미가 명확하고 짧게 구성하며, 카멜 표기법(camelCase)과 케밥 표기법(kebab-case) 둘 다 제공합니다.

---

|항목 이름 (한글)|의미 설명|camelCase 폴더명|kebab-case 폴더명|
|---|---|---|---|
|ESG 비전 및 전략|ESG 목표 및 중장기 전략|`esgVisionStrategy`|`esg-vision-strategy`|
|ESG 거버넌스|ESG 조직 운영 구조|`esgGovernance`|`esg-governance`|
|ESG경영 내재화|ESG 가치의 내재화|`esgIntegration`|`esg-integration`|
|이해관계자 소통|외부 커뮤니케이션|`stakeholderEngagement`|`stakeholder-engagement`|
|이중중대성 평가|Double materiality 평가|`doubleMateriality`|`double-materiality`|
|Materiality #1 기후변화 대응|첫 번째 핵심 이슈: 기후변화 대응|`materialityClimate`|`materiality-climate`|
|Materiality #2 안전보건|두 번째 핵심 이슈: 안전보건|`materialitySafety`|`materiality-safety`|
|Materiality #3 윤리 및 컴플라이언스|세 번째 핵심 이슈: 윤리/준법|`materialityEthics`|`materiality-ethics`|

---

### ✅ 예시 디렉토리 구조 (`camelCase` 기준)

```
esgStorybook/
├── esgVisionStrategy/
├── esgGovernance/
├── esgIntegration/
├── stakeholderEngagement/
├── doubleMateriality/
├── materialityClimate/
├── materialitySafety/
└── materialityEthics/
```

원하는 표기 스타일이나 내부 파일 구조까지 필요하시면 추가로 도와드릴게요!


#### ESG performance 

좋습니다! 아래는 ESG 세 축인 **Environmental / Social / Governance** 영역별로 이미지에 있는 한글 항목들을 영어 폴더명으로 변환한 제안입니다. 유지보수 편의를 위해 **camelCase**와 **kebab-case** 두 방식 모두 제공합니다.

---

### ✅ Environmental

|항목명 (한글)|의미 설명|camelCase|kebab-case|
|---|---|---|---|
|환경경영|환경 관련 경영 활동|`environmentalMgmt`|`environmental-mgmt`|
|환경투자|친환경 설비/기술 투자|`environmentalInvestment`|`environmental-investment`|
|자원순환|재활용, 자원 절감 등|`resourceCirculation`|`resource-circulation`|
|생명다양성 보전|생태계/생물 다양성 보존|`biodiversityConservation`|`biodiversity-conservation`|

---

### ✅ Social

|항목명 (한글)|의미 설명|camelCase|kebab-case|
|---|---|---|---|
|인권보호|노동 인권 보장|`humanRights`|`human-rights`|
|다양성 및 포용성|DEI (Diversity, Equity, Inclusion)|`diversityInclusion`|`diversity-inclusion`|
|건전한 노사관계|협력적 노사 문화|`laborRelations`|`labor-relations`|
|지역사회 상생협력|지역사회 기여|`communityEngagement`|`community-engagement`|
|정보보호|개인정보/정보보안|`dataProtection`|`data-protection`|
|고객만족|고객서비스 및 품질|`customerSatisfaction`|`customer-satisfaction`|

---

### ✅ Governance

|항목명 (한글)|의미 설명|camelCase|kebab-case|
|---|---|---|---|
|지배구조|이사회 구조/운영|`corporateGovernance`|`corporate-governance`|
|공정거래|공정한 거래 질서|`fairTrade`|`fair-trade`|
|리스크 관리|기업 리스크 대응 체계|`riskManagement`|`risk-management`|

---

### 📁 폴더 구조 예시 (camelCase 기준)

```
esg/
├── environmental/
│   ├── environmentalMgmt/
│   ├── environmentalInvestment/
│   ├── resourceCirculation/
│   └── biodiversityConservation/
├── social/
│   ├── humanRights/
│   ├── diversityInclusion/
│   ├── laborRelations/
│   ├── communityEngagement/
│   ├── dataProtection/
│   └── customerSatisfaction/
└── governance/
    ├── corporateGovernance/
    ├── fairTrade/
    └── riskManagement/
```

표기법이나 구조화 방식이 다르게 필요하시면 언제든지 말씀해주세요!


#### Appendix

좋습니다! 아래는 이미지에 있는 한글 항목들을 프론트엔드에서 사용할 **영문 폴더명**으로 변환한 제안입니다. 일반적으로 사용되는 ESG 표기 용어와 명명 규칙을 고려했으며, **camelCase**와 **kebab-case** 버전 둘 다 제공합니다.

---

### 📄 Appendix 폴더명 추천

|항목명 (한글)|설명|camelCase 폴더명|kebab-case 폴더명|
|---|---|---|---|
|ESG Data|ESG 정량/정성 데이터|`esgData`|`esg-data`|
|GRI Standards 2021 Index|GRI 기준별 콘텐츠 인덱스|`griIndex2021`|`gri-index-2021`|
|UN Global Compact Index|UNGC 기준 인덱스|`ungcIndex`|`ungc-index`|
|TCFD Index|기후 재무정보 공개 프레임워크 인덱스|`tcfdIndex`|`tcfd-index`|
|수상실적 및 가입단체 현황|ESG 관련 수상 및 가입 기관 리스트|`awardsMemberships`|`awards-memberships`|
|제3자 검증의견서|외부 검증기관의 검토 의견서|`assuranceStatement`|`assurance-statement`|

---

### 📁 예시 디렉토리 구조 (camelCase 기준)

```
esgReport/
├── esgData/
├── griIndex2021/
├── ungcIndex/
├── tcfdIndex/
├── awardsMemberships/
└── assuranceStatement/
```

필요하다면 내부 파일 구조나 각 폴더 안에 들어갈 페이지 구성도 함께 설계해드릴 수 있습니다.





