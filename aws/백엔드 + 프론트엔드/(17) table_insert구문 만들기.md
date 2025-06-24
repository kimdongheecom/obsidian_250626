
#### 프롬프팅(엑셀 파일 업로드하고나서 아래 문장 프롬프팅 하면 됨)

categories 테이블에 GRI STANDARD 컬럼에서 GRI 2는 id 칼럼에 insert하고, :는 삭제하고, General Disclosures는 title colum에 insert하고, 2021은 year 칼럼에 insert하는 sql 구문을 작성해줘.


#### 답변

네, 요청하신 내용을 바탕으로 categories 테이블에 데이터를 삽입하는 SQL 구문을 작성해 드리겠습니다.

### 1. 요청하신 "GRI 2"에 대한 SQL 구문

가장 먼저 요청하신 "GRI 2: General Disclosures 2021" 데이터 하나만을 categories 테이블에 삽입하는 SQL 구문입니다.

```
INSERT INTO categories (id, title, year)
VALUES ('GRI 2', 'General Disclosures', '2021');
```


**설명:**

- GRI STANDARD 컬럼의 "GRI 2: General Disclosures 2021" 문자열을 파싱합니다.
    
- id 컬럼에 'GRI 2'를 삽입합니다. (CHAR(6) 크기에 맞습니다)
    
- title 컬럼에 'General Disclosures'를 삽입합니다.
    
- year 컬럼에 '2021'을 삽입합니다.
    

---

### 2. 제공된 파일의 다른 GRI Standard를 포함하는 종합 SQL 구문

제공해주신 엑셀 파일에 있는 다른 GRI Standard들도 함께 categories 테이블에 추가할 수 있도록 여러 데이터를 한 번에 삽입하는 예시 구문입니다.

**참고:** id 컬럼이 CHAR(6)으로 정의되어 있어 'GRI 201'과 같이 6자리를 초과하는 값은 오류가 발생할 수 있습니다. 이를 해결하기 위해 id 컬럼의 데이터 타입을 VARCHAR(10) 등으로 변경하는 것을 권장합니다. 아래 구문은 id 컬럼이 충분한 길이를 가진다고 가정하고 작성되었습니다.

**테이블 구조 변경 예시 (선택 사항)**

```
-- 만약 id 컬럼 길이를 변경해야 한다면 아래와 같이 실행할 수 있습니다.
ALTER TABLE categories MODIFY id VARCHAR(10);
```

**종합 INSERT SQL 구문**

```
INSERT INTO categories (id, title, year) VALUES
('GRI 1', 'Foundation', '2021'),
('GRI 2', 'General Disclosures', '2021'),
('GRI 3', 'Material Topics', '2021'),
('GRI 201', 'Economic Performance', '2016'),
('GRI 202', 'Market Presence', '2016'),
('GRI 203', 'Indirect Economic Impacts', '2016'),
('GRI 204', 'Procurement Practices', '2016'),
('GRI 205', 'Anti-corruption', '2016'),
('GRI 206', 'Anti-competitive Behavior', '2016'),
('GRI 207', 'Tax', '2019'),
('GRI 301', 'Materials', '2016'),
('GRI 302', 'Energy', '2016'),
('GRI 303', 'Water and Effluents', '2018'),
('GRI 304', 'Biodiversity', '2016'),
('GRI 305', 'Emissions', '2016'),
('GRI 306', 'Waste', '2020'),
('GRI 308', 'Supplier Environmental Assessment', '2016'),
('GRI 401', 'Employment', '2016'),
('GRI 402', 'Labor/Management Relations', '2016'),
('GRI 403', 'Occupational Health and Safety', '2018'),
('GRI 404', 'Training and Education', '2016'),
('GRI 405', 'Diversity and Equal Opportunity', '2016'),
('GRI 406', 'Non-discrimination', '2016'),
('GRI 407', 'Freedom of Association and Collective Bargaining', '2016'),
('GRI 408', 'Child Labor', '2016'),
('GRI 409', 'Forced or Compulsory Labor', '2016'),
('GRI 410', 'Security Practices', '2016'),
('GRI 411', 'Rights of Indigenous Peoples', '2016'),
('GRI 413', 'Local Communities', '2016'),
('GRI 414', 'Supplier Social Assessment', '2016'),
('GRI 415', 'Public Policy', '2016'),
('GRI 416', 'Customer Health and Safety', '2016'),
('GRI 417', 'Marketing and Labeling', '2016'),
('GRI 418', 'Customer Privacy', '2016');
```


#### 프롬프팅
-- =================================================================  
-- Table: 공시 (Disclosure) - categories의 자식  
-- =================================================================  
CREATE TABLE disclosures (  
id CHAR(6) PRIMARY KEY,  
title VARCHAR(70),  
category_id CHAR(6),  
CONSTRAINT fk_disclosures_to_categories  
FOREIGN KEY (category_id)  
REFERENCES categories (id)  
ON DELETE RESTRICT ON UPDATE CASCADE  
);

COMMENT ON TABLE disclosures IS '공시 정보 테이블';  
COMMENT ON COLUMN disclosures.id IS '공시 PK (고정 6자리 텍스트)';  
COMMENT ON COLUMN disclosures.title IS '공시 제목 (70자 이내)';  
COMMENT ON COLUMN disclosures.category_id IS '부모 카테고리 FK';

위의 것은 disclosures 테이블인데, 이 테이블에 공시사항 컬럼에서 2-1은 id 칼럼에 insert하고, 조직 세부 정보는 title 컬럼에 insert하고, GRI STANDARD에 있는 GRI 2는 category_id에 insert하는 sql 구문을 작성해줘.


#### 답변

네, 알겠습니다. 요청하신 disclosures 테이블에 데이터를 삽입하는 SQL 구문을 작성해 드리겠습니다.

### 1. 요청하신 "2-1" 공시사항 하나에 대한 SQL 구문

가장 먼저 요청하신 "2-1 조직 세부 정보" 데이터 하나만을 disclosures 테이블에 삽입하는 SQL 구문입니다.

```
INSERT INTO disclosures (id, title, category_id)
VALUES ('2-1', '조직 세부 정보', 'GRI 2');
```

**설명:**

- id 컬럼에 '2-1'을 삽입합니다.
    
- title 컬럼에 '조직 세부 정보'를 삽입합니다.
    
- category_id 컬럼에 'GRI 2'를 삽입합니다. 이 값은 categories 테이블에 먼저 존재해야 합니다 (Foreign Key).
    

---

### 2. 제공된 파일의 모든 공시사항을 포함하는 종합 SQL 구문

제공해주신 엑셀 파일에 있는 모든 공시사항을 disclosures 테이블에 삽입할 수 있도록 전체 데이터를 바탕으로 SQL 구문을 작성했습니다.

**※ 사전 조건:** 이 구문을 실행하기 전에, 먼저 categories 테이블에 'GRI 2', 'GRI 3', 'GRI 201' 등의 데이터가 모두 입력되어 있어야 합니다.

Generated sql

```
-- GRI 2 Disclosures
INSERT INTO disclosures (id, title, category_id) VALUES
('2-1', '조직 세부 정보', 'GRI 2'),
('2-2', '조직의 지속 가능성 보고에 포함된 엔티티', 'GRI 2'),
('2-3', '보고서 보고기간, 보고주기 및 문의처', 'GRI 2'),
('2-4', '정보 재진술', 'GRI 2'),
('2-5', '외부 검증', 'GRI 2'),
('2-6', '활동, 가치 사슬 및 기타 비즈니스 관계', 'GRI 2'),
('2-7', '근로자', 'GRI 2'),
('2-8', '기타 근로자', 'GRI 2'),
('2-9', '지배구조', 'GRI 2'),
('2-10', '최고 거버넌스 기구의 지명 및 선정', 'GRI 2'),
('2-11', '최고의사결정기구 의장', 'GRI 2'),
('2-12', '영향 관리를 감독하는 최고의사결정기구의 역할', 'GRI 2'),
('2-13', '영향 관리에 대한 책임 위임', 'GRI 2'),
('2-14', '지속가능성 보고에 대한 최고의사결정기구 역할', 'GRI 2'),
('2-15', '이해관계 상충', 'GRI 2'),
('2-16', '중요 문제에 대한 커뮤니케이션', 'GRI 2'),
('2-17', '최고의사결정기구의 집단지식', 'GRI 2'),
('2-18', '최고의사결정기구의 성과 평가', 'GRI 2'),
('2-19', '보수정책', 'GRI 2'),
('2-20', '보수 결정 절차', 'GRI 2'),
('2-21', '연간 총 보상 비율', 'GRI 2'),
('2-22', '지속가능한 개발전략에 관한 성명서', 'GRI 2'),
('2-23', '정책 선언문', 'GRI 2'),
('2-24', '정책 공약 책임', 'GRI 2'),
('2-25', '부정적인 영향을 해결하기 위한 프로세스', 'GRI 2'),
('2-26', '제기된 우려사항 및 조언에 대한 메커니즘', 'GRI 2'),
('2-27', '법규제에 대한 컴플라이언스', 'GRI 2'),
('2-28', '멤버십 조직', 'GRI 2'),
('2-29', '이해관계자 참여 방식', 'GRI 2'),
('2-30', '단체협약', 'GRI 2');

-- GRI 3 Disclosures
INSERT INTO disclosures (id, title, category_id) VALUES
('3-1', '중요 주제 결정 과정', 'GRI 3'),
('3-2', '중요 주제 목록', 'GRI 3'),
('3-3', '중요 주제 관리', 'GRI 3');

-- GRI 200 Series Disclosures (Economic)
INSERT INTO disclosures (id, title, category_id) VALUES
('201-1', '직접적인 경제적 가치의 창출과 분배', 'GRI 201'),
('201-2', '기후변화에 따른 재무적 영향 및 기타 위험과 기회', 'GRI 201'),
('201-3', '확정급여형 연금 채무 및 기타 퇴직연금안', 'GRI 201'),
('201-4', '정부 재정지원', 'GRI 201'),
('202-1', '사업장 소재 지역의 최저 임금 대비 초임 임금의 비율', 'GRI 202'),
('202-2', '사업장이 소재한 지역사회에서 고용된 고위 임원의 비율', 'GRI 202'),
('203-1', '사회기반시설 투자 및 서비스 지원', 'GRI 203'),
('203-2', '중요한 간접 경제 영향', 'GRI 203'),
('204-1', '지역 공급업체에 지출하는 비용의 비중', 'GRI 204'),
('205-1', '사업장 부패 위험 평가', 'GRI 205'),
('205-2', '반부패 정책 및 절차에 관한 공지와 교육', 'GRI 205'),
('205-3', '확인된 부패사례와 이에 대한 조치', 'GRI 205'),
('206-1', '경쟁 저해 행위, 독과점 등 불공정 거래 행위에 대한 법적 조치', 'GRI 206'),
('207-1', '조세 접근법', 'GRI 207'),
('207-2', '조세 전략을 책임지는 지배기구, 통제 및 리스크 관리', 'GRI 207'),
('207-3', '조세 관련 이해관계자의 참여 및 관리', 'GRI 207'),
('207-4', '국가별 리포팅', 'GRI 207');

-- GRI 300 Series Disclosures (Environmental)
INSERT INTO disclosures (id, title, category_id) VALUES
('301-1', '원재료의 중량이나 부피', 'GRI 301'),
('301-2', '재생투입 원재료 사용 비율', 'GRI 301'),
('301-3', '재생된 제품 및 포장재', 'GRI 301'),
('302-1', '조직 내부 에너지 소비', 'GRI 302'),
('302-2', '조직 외부 에너지 소비', 'GRI 302'),
('302-3', '에너지 집약도', 'GRI 302'),
('302-4', '에너지 소비 감축', 'GRI 302'),
('302-5', '제품 및 서비스의 에너지 요구량 감축', 'GRI 302'),
('303-1', '용수 공유 자원 활용 및 교류', 'GRI 303'),
('303-2', '방수 관련 영향 관리', 'GRI 303'),
('303-3', '용수 취수량', 'GRI 303'),
('303-4', '용수 방류량', 'GRI 303'),
('303-5', '용수 소비량', 'GRI 303'),
('304-1', '보호지역 및 생물다양성 가치가 높은 지역 내 또는 그 인근에서 소유/임대/운영되는 사업장', 'GRI 304'),
('304-2', '조직의 사업활동, 제품, 서비스가 생물다양성에 미치는 영향', 'GRI 304'),
('304-3', '서식지 보호 또는 복구', 'GRI 304'),
('304-4', 'IUCN 적색목록 및 조직 사업의 영향을 받는 지역 내에 서식하는 국가보호종 목록', 'GRI 304'),
('305-1', '직접 온실가스 배출량 (Scope1)', 'GRI 305'),
('305-2', '간접 온실가스 배출량 (Scope2)', 'GRI 305'),
('305-3', '기타 간접 온실가스 배출량 (Scope3)', 'GRI 305'),
('305-4', '온실가스 배출 집약도', 'GRI 305'),
('305-5', '온실가스 배출 감축', 'GRI 305'),
('305-6', '오존층 파괴 물질 (ODS) 배출량', 'GRI 305'),
('305-7', '질소산화물(NOx), 황산화물(SOx) 및 기타 중요한 대기 배출량', 'GRI 305'),
('306-1', '폐기물 발생 및 중대 폐기물 관련 영향', 'GRI 306'),
('306-2', '폐기물 관련 중대 영향 관리', 'GRI 306'),
('306-3', '폐기물 발생', 'GRI 306'),
('306-4', '폐기되지 않은 폐기물', 'GRI 306'),
('306-5', '처분 대상 폐기물', 'GRI 306'),
('308-1', '환경 기준 심사를 거친 신규 공급업체', 'GRI 308'),
('308-2', '공급망의 부정적 환경 영향 및 이에 대한 조치', 'GRI 308');

-- GRI 400 Series Disclosures (Social)
INSERT INTO disclosures (id, title, category_id) VALUES
('401-1', '신규채용 및 이직', 'GRI 401'),
('401-2', '비정규직 근로자에게는 제공되지 않는 정규직 근로자를 위한 복리후생', 'GRI 401'),
('401-3', '육아휴직', 'GRI 401'),
('402-1', '운영상의 변화와 관련한 최소 공지기간', 'GRI 402'),
('403-1', '산업안전보건경영시스템', 'GRI 403'),
('403-2', '유해 요인 식별, 위험성 평가 및 사고 조사', 'GRI 403'),
('403-3', '산업보건 서비스', 'GRI 403'),
('403-4', '직업안전보건에 대한 근로자 참여, 협의 및 의사소통', 'GRI 403'),
('403-5', '산업안전보건 근로자 교육', 'GRI 403'),
('403-6', '근로자 건강 증진', 'GRI 403'),
('403-7', '경영 관계에 의해 직접적으로 연계된 산업안전보건 영향의 예방 및 저감', 'GRI 403'),
('403-8', '산업안전보건경영시스템의 적용을 받는 근로자', 'GRI 403'),
('403-9', '업무 관련 상해', 'GRI 403'),
('403-10', '업무 관련 질병', 'GRI 403'),
('404-1', '근로자 1인당 평균 교육 시간', 'GRI 404'),
('404-2', '직원 역량강화 및 평생교육 프로그램', 'GRI 404'),
('404-3', '정기적으로 성과 및 경력 개발 검토를 받는 직원 비율', 'GRI 404'),
('405-1', '지배구조 기구와 직원의 다양성', 'GRI 405'),
('405-2', '남성 대비 여성의 기본급 및 보수 비율', 'GRI 405'),
('406-1', '차별 사례 및 이에 대한 시정조치', 'GRI 406'),
('407-1', '집회결사 및 단체교섭권 훼손 위험이 있는 사업장 및 공급업체', 'GRI 407'),
('408-1', '아동노동 발생 위험이 높은 사업장 및 공급업체', 'GRI 408'),
('409-1', '강제 노역 발생 위험이 높은 사업장 및 공급업체', 'GRI 409'),
('410-1', '인권 정책 및 절차에 관한 교육을 받은 보안 담당자', 'GRI 410'),
('411-1', '토착민 권리 침해 사례', 'GRI 411'),
('413-1', '지역사회 참여, 영향 평가 및 개발 프로그램 운영 사업장', 'GRI 413'),
('413-2', '지역사회에 중대한 실제적/잠재적 부정적 영향을 미치는 사업장', 'GRI 413'),
('414-1', '사회적 기준에 따른 심사를 거친 신규 공급업체', 'GRI 414'),
('414-2', '공급망 내 부정적 사회적 영향 및 그에 대한 대응조치', 'GRI 414'),
('415-1', '정치 기부금', 'GRI 415'),
('416-1', '제품/서비스의 건강 및 안전 영향 평가', 'GRI 416'),
('416-2', '제품/서비스의 건강 및 안전 영향 관련 위반', 'GRI 416'),
('417-1', '제품/서비스 관련 정보 및 라벨링 요건', 'GRI 417'),
('417-2', '제품/서비스 정보 및 라벨링 관련 위반', 'GRI 417'),
('417-3', '마케팅 커뮤니케이션 관련 위반', 'GRI 417'),
('418-1', '고객 개인정보보호 위반 및 고객정보 분실 관련 입증된 민원', 'GRI 418');
```


#### 프롬프팅팅
-- =================================================================  
-- Table: 요구사항 (Requirements) - disclosures의 자식  
-- =================================================================  
CREATE TABLE requirements (  
id VARCHAR(10) PRIMARY KEY,  
question TEXT, -- 1000자 이내의 긴 질문이므로 TEXT 타입 권장  
disclosure_id CHAR(6),  
CONSTRAINT fk_requirements_to_disclosures  
FOREIGN KEY (disclosure_id)  
REFERENCES disclosures (id)  
ON DELETE RESTRICT ON UPDATE CASCADE  
);

COMMENT ON TABLE requirements IS '요구사항(질문) 목록 테이블';  
COMMENT ON COLUMN requirements.id IS '요구사항 PK (10자 이내 텍스트)';  
COMMENT ON COLUMN requirements.question IS '요구사항 질문 내용 (1000자 이내)';  
COMMENT ON COLUMN requirements.disclosure_id IS '부모 공시 FK';

위의 것은 requirements 테이블인데, 요구사항id는 id 칼럼에 insert하고, 요구사항은 question colum에 insert하고, 공시사항에 있는 2-1을 disclosure_id에 insert하는 sql 구문을 작성해줘.




#### 프롬프팅


-- =================================================================

-- Table: 사용자 (User) - 구글 OAuth 정보 기반

-- =================================================================

CREATE TABLE users (

    id VARCHAR(30) PRIMARY KEY, -- Google의 'sub' 클레임과 매핑 (고유 ID)

    name VARCHAR(30),

    picture VARCHAR(70), -- 권장: VARCHAR(512). URL은 70자보다 길어질 수 있습니다.

    email VARCHAR(70) UNIQUE,

    email_verified BOOLEAN DEFAULT FALSE,

    locale VARCHAR(10)

);

  

COMMENT ON TABLE users IS '사용자 정보 테이블 (Google OAuth 기반)';

COMMENT ON COLUMN users.id IS '사용자 PK (Google sub, 30자 미만)';

COMMENT ON COLUMN users.name IS '이름 (30자 이내)';

COMMENT ON COLUMN users.picture IS '프로필 사진 URL (70자 이내)';

COMMENT ON COLUMN users.email IS '이메일 (70자 이내, 고유값)';

COMMENT ON COLUMN users.email_verified IS '이메일 인증 여부 (Boolean)';

COMMENT ON COLUMN users.locale IS '지역/언어 설정 (10자 이내)';









#### 프롬프팅

-- =================================================================

-- Table: 샘플 데이터 (Sample)

-- =================================================================

CREATE TABLE samples (

    id SERIAL PRIMARY KEY, -- 1부터 시작하는 자동 증가 정수

    qual_data TEXT -- 3000자 이내의 긴 텍스트이므로 TEXT 타입 권장

);

  

COMMENT ON TABLE samples IS '정성적 데이터 샘플 테이블';

COMMENT ON COLUMN samples.id IS '샘플 PK (자동 증가 정수)';

COMMENT ON COLUMN samples.qual_data IS '정성 데이터 (3000자 이내)';


위의 것은 sample 테이블인데, answer id는 id 칼럼에 insert하고, answer는 quant_data colum에 insert하고, 공시사항에 있는 2-1을 disclosure_id에 insert하는 sql 구문을 작성해줘.

#### 프롬프팅

-- =================================================================

-- Table: 답변 (Answer) - 교차 엔터티

-- =================================================================

CREATE TABLE answers (

    id SERIAL PRIMARY KEY, -- 1부터 시작하는 자동 증가 정수

    quant_data TEXT, -- 3000자 이내의 긴 텍스트이므로 TEXT 타입 권장

    -- Foreign Keys

    requirements_id VARCHAR(10),

    user_id VARCHAR(30),

    sample_id INTEGER,

    -- Constraints

    CONSTRAINT fk_answers_to_requirements

        FOREIGN KEY (requirements_id)

        REFERENCES requirements (id)

        ON DELETE CASCADE ON UPDATE CASCADE, -- 질문이 삭제되면 답변도 삭제 (CASCADE)

    CONSTRAINT fk_answers_to_users

        FOREIGN KEY (user_id)

        REFERENCES users (id)

        ON DELETE RESTRICT ON UPDATE CASCADE, -- 사용자를 함부로 삭제하지 못하게 제한 (RESTRICT)

    CONSTRAINT fk_answers_to_samples

        FOREIGN KEY (sample_id)

        REFERENCES samples (id)

        ON DELETE RESTRICT ON UPDATE CASCADE -- 샘플 데이터를 함부로 삭제하지 못하게 제한 (RESTRICT)

);

  

COMMENT ON TABLE answers IS '답변 테이블 (Requirements, Users, Samples의 교차 엔터티)';

COMMENT ON COLUMN answers.id IS '답변 PK (자동 증가 정수)';

COMMENT ON COLUMN answers.quant_data IS '정량 데이터 (3000자 이내)';

COMMENT ON COLUMN answers.requirements_id IS '어떤 질문에 대한 답변인지 FK';

COMMENT ON COLUMN answers.user_id IS '누가 답변했는지 FK';

COMMENT ON COLUMN answers.sample_id IS '어떤 샘플 데이터에 대한 답변인지 FK';




