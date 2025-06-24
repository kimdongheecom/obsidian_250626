https://app.diagrams.net/ 에 들어가서 생성함(참고할 때, sql페이지 참고함)


1. 위의 사이트 들어가서 table 생성함
	1. 테이블은 총 7개 임.
	2. 각각의 컬럼명 추가해줘야 하고, 괄호 안에 있는 것은 글자수를 의미함(auto increment는 1부터 증가하는 것을 의미함)
	3. pk: 자식이고, fk는 부모이다. 그래서 까치발쪽은 자식이고, 까치발이 아닌쪽은 부모이다. 그리고 자식은 아래에 부모를 항상 써줘야한다.
	4. 1대1 대응관계도 있다( 가공시켜서 나온 관계 이고, 대응되니까 1대1로 표현함)
		1. disclosures와 sample 관계
		2. requirements와 answers 관계
	5. 아래 이미지를 구글 에이아이에 주고, sql insert문을 작성해달라고 해야한다. 
		1. 예) category.sql 작성해줘....(줄 때, **ERD 이미지와 엑셀 파일을 주고, ddl.sql을 주면서 요구해야한다.**)
![[Pasted image 20250620175203.png]]

아래 코드는 ddl.sql이다. 이걸 주면서 sql insert문을 주라고 해야한다. 

```
-- 데이터베이스 스키마 생성 (PostgreSQL 기준)
-- 1:1 관계 (requirements-answers, disclosures-sample) 반영

-- =================================================================
-- Table: 카테고리 (Category) - 최상위 분류
-- =================================================================
CREATE TABLE categories (
    id CHAR(6) PRIMARY KEY,
    title VARCHAR(50),
    year CHAR(4)
);

COMMENT ON TABLE categories IS '최상위 분류 카테고리';
COMMENT ON COLUMN categories.id IS '카테고리 PK (고정 6자리 텍스트)';
COMMENT ON COLUMN categories.title IS '카테고리 제목 (50자 이내)';
COMMENT ON COLUMN categories.year IS '연도 (고정 4자리 텍스트)';


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


-- =================================================================
-- Table: 요구사항 (Requirements) - disclosures의 자식
-- =================================================================
CREATE TABLE requirements (
    id VARCHAR(10) PRIMARY KEY,
    question TEXT,
    disclosure_id CHAR(6),
    CONSTRAINT fk_requirements_to_disclosures
        FOREIGN KEY (disclosure_id)
        REFERENCES disclosures (id)
        ON DELETE RESTRICT ON UPDATE CASCADE
);

COMMENT ON TABLE requirements IS '요구사항(질문) 목록 테이블';
COMMENT ON COLUMN requirements.id IS '요구사항 PK (10자 이내 텍스트)';
COMMENT ON COLUMN requirements.question IS '요구사항 질문 내용';
COMMENT ON COLUMN requirements.disclosure_id IS '부모 공시 FK';


-- =================================================================
-- Table: 회사 (Companies)
-- =================================================================
CREATE TABLE companies (
    id VARCHAR(20) PRIMARY KEY,
    company_add VARCHAR(50),
    company_num VARCHAR(20),
    company_hp VARCHAR(40)
);

COMMENT ON TABLE companies IS '회사 정보 테이블';
COMMENT ON COLUMN companies.id IS '회사 PK (20자 이내)';
COMMENT ON COLUMN companies.company_add IS '회사 주소 (50자 이내)';
COMMENT ON COLUMN companies.company_num IS '회사 사업자등록번호 (20자 이내)';
COMMENT ON COLUMN companies.company_hp IS '회사 홈페이지 URL (40자 이내)';


-- =================================================================
-- Table: 사용자 (Users) - companies의 자식
-- =================================================================
CREATE TABLE users (
    id VARCHAR(30) PRIMARY KEY,
    name VARCHAR(30),
    picture VARCHAR(512),
    email VARCHAR(70) UNIQUE,
    email_verified BOOLEAN DEFAULT FALSE,
    locale VARCHAR(10),
    department VARCHAR(20),
    role VARCHAR(20),
    company_id VARCHAR(20),
    CONSTRAINT fk_users_to_companies
        FOREIGN KEY (company_id)
        REFERENCES companies (id)
        ON DELETE RESTRICT ON UPDATE CASCADE
);

COMMENT ON TABLE users IS '사용자 정보 테이블 (Google OAuth 기반)';
COMMENT ON COLUMN users.id IS '사용자 PK (Google sub)';
COMMENT ON COLUMN users.name IS '이름';
COMMENT ON COLUMN users.picture IS '프로필 사진 URL';
COMMENT ON COLUMN users.email IS '이메일 (고유값)';
COMMENT ON COLUMN users.email_verified IS '이메일 인증 여부';
COMMENT ON COLUMN users.locale IS '지역/언어 설정';
COMMENT ON COLUMN users.department IS '부서';
COMMENT ON COLUMN users.role IS '역할/직책';
COMMENT ON COLUMN users.company_id IS '소속 회사 FK';


-- =================================================================
-- Table: 샘플 데이터 (sample) - disclosures와 1:1 관계
-- =================================================================
CREATE TABLE sample (
    id SERIAL PRIMARY KEY,
    qual_data TEXT,
    company_id VARCHAR(20),
    disclosure_id CHAR(6) NOT NULL, -- 1:1 관계를 위한 FK
    
    -- 1:1 관계를 위한 UNIQUE 제약조건 추가
    CONSTRAINT uq_sample_disclosure_id UNIQUE (disclosure_id), 
    
    CONSTRAINT fk_sample_to_companies
        FOREIGN KEY (company_id)
        REFERENCES companies (id)
        ON DELETE CASCADE ON UPDATE CASCADE,
        
    CONSTRAINT fk_sample_to_disclosures
        FOREIGN KEY (disclosure_id)
        REFERENCES disclosures(id)
        ON DELETE CASCADE ON UPDATE CASCADE
);

COMMENT ON TABLE sample IS '정성적 데이터 샘플 테이블 (Disclosures와 1:1 관계)';
COMMENT ON COLUMN sample.id IS '샘플 PK (자동 증가 정수)';
COMMENT ON COLUMN sample.qual_data IS '정성 데이터 (3000자 이내)';
COMMENT ON COLUMN sample.company_id IS '샘플 데이터를 생성한 회사 FK';
COMMENT ON COLUMN sample.disclosure_id IS '어떤 공시에 대한 샘플인지 나타내는 FK (고유값)';


-- =================================================================
-- Table: 답변 (answers) - requirements와 1:1 관계
-- =================================================================
CREATE TABLE answers (
    id SERIAL PRIMARY KEY,
    quant_data TEXT,
    requirement_id VARCHAR(10) NOT NULL, -- 1:1 관계를 위한 FK
    user_id VARCHAR(30) NOT NULL,
    
    -- 1:1 관계를 위한 UNIQUE 제약조건 추가
    CONSTRAINT uq_answers_requirement_id UNIQUE (requirement_id),
    
    CONSTRAINT fk_answers_to_requirements
        FOREIGN KEY (requirement_id)
        REFERENCES requirements (id)
        ON DELETE CASCADE ON UPDATE CASCADE,
        
    CONSTRAINT fk_answers_to_users
        FOREIGN KEY (user_id)
        REFERENCES users (id)
        ON DELETE RESTRICT ON UPDATE CASCADE
);

COMMENT ON TABLE answers IS '답변 테이블 (Requirements와 1:1 관계)';
COMMENT ON COLUMN answers.id IS '답변 PK (자동 증가 정수)';
COMMENT ON COLUMN answers.quant_data IS '정량 데이터 (3000자 이내)';
COMMENT ON COLUMN answers.requirement_id IS '어떤 질문에 대한 답변인지 나타내는 FK (고유값)';
COMMENT on COLUMN answers.user_id IS '누가 답변했는지 FK';
```



##### table이 총 6개이고, user.sql만 빼고 나머지는 다 insert sql을 만들었다.

### 왜 user.sql을 만들지 않는가?


네, 아주 좋은 질문입니다. 그리고 선생님의 말씀이 정확합니다. **user.sql 파일을 만들어서 사용자를 미리 추가하는 것은 일반적인 방식이 아니며, 그렇게 하지 않는 것이 맞습니다.**

왜 그런지, 그리고 DDL 파일은 어떻게 해야 하는지 자세히 설명해 드릴게요.


선생님께서 "DB를 사용한다"고 말씀하신 것과 "구글 인증"을 언급하신 것이 핵심입니다.

1. **사용자 데이터는 '동적(Dynamic)' 데이터입니다.**
    
    - categories.sql이나 requirements.sql에 들어가는 데이터는 애플리케이션이 시작되기 전에 미리 정의된, 거의 변하지 않는 **정적(Static) 데이터**입니다.
        
    - 하지만 users 테이블의 데이터는 다릅니다. 사용자는 애플리케이션이 운영되는 중에 **실시간으로 가입**합니다. 어떤 사용자가 언제 가입할지 미리 알 수 없습니다.
        
    - 따라서, 미리 INSERT 구문을 만들어 놓는 것이 의미가 없고, 가능하지도 않습니다.
        
2. **사용자 생성은 '인증 프로세스'의 결과물입니다.**
    
    - 새로운 사용자가 "Google로 로그인" 버튼을 누르면, 우리 애플리케이션은 Google에게 인증을 요청합니다.
        
    - 사용자가 Google에서 성공적으로 로그인하면, Google은 우리 애플리케이션에게 "이 사용자는 인증되었습니다"라는 신호와 함께 사용자의 고유 ID(sub), 이름, 이메일, 프로필 사진 URL 등을 보내줍니다.
        
    - 우리 **애플리케이션의 백엔드 코드**는 이 정보를 받아서, users 테이블에 해당 사용자가 이미 있는지 확인합니다.
        
        - **없으면 (최초 로그인):** 백엔드 코드가 **그때 INSERT INTO users ... SQL을 실행**하여 새로운 사용자를 데이터베이스에 저장합니다.
            
        - **있으면 (재로그인):** 이미 있는 사용자 정보를 바탕으로 로그인을 처리합니다.
            
    - 결론적으로 사용자 생성(INSERT)은 SQL 파일을 수동으로 실행해서 하는 것이 아니라, **애플리케이션 로직에 의해 자동으로 처리**되는 과정입니다. 이것이 선생님께서 "DB를 사용한다"고 말씀하신 부분의 정확한 의미일 것입니다.
        
3. **보안 및 데이터 무결성**
    
    - users 테이블에 있는 모든 사용자는 반드시 Google과 같은 신뢰할 수 있는 인증 제공업체를 통해 신원이 확인된 사용자여야 합니다.
        
    - 만약 user.sql로 사용자를 수동으로 추가하면, 인증 과정을 거치지 않은 '가짜' 사용자가 데이터베이스에 들어갈 수 있어 보안에 매우 취약해집니다.
        

### 그렇다면 ddl.sql은 고치지 않아도 되는가?

**아닙니다. ddl.sql 파일은 현재 상태 그대로 반드시 필요하며, 수정할 필요가 없습니다.**

여기서 중요한 개념 차이를 이해해야 합니다.

- **ddl.sql (Data Definition Language):** 이 파일은 CREATE TABLE ... 구문을 담고 있습니다. 이것은 데이터베이스에 users라는 **'테이블의 구조(청사진)'**를 만드는 역할을 합니다. 즉, "사용자 정보는 앞으로 이런 칸(컬럼)들에 맞춰서 저장될 것이다"라고 약속하고 틀을 짜는 과정입니다. 이 틀이 없으면 사용자 정보를 저장할 공간 자체가 없습니다.
    
- **user.sql (Data Manipulation Language):** 만약 이 파일을 만든다면, INSERT INTO ... 구문을 담게 됩니다. 이것은 이미 만들어진 users 테이블의 **'구조 안에 실제 데이터(내용물)'**를 채워 넣는 역할을 합니다.
    

**결론적으로, 선생님의 말씀은 다음과 같이 요약할 수 있습니다.**

1. **ddl.sql은 필수:** CREATE TABLE users ... 구문은 users 테이블이라는 '빈 서랍장'을 만드는 것이므로 반드시 필요합니다. **현재의 ddl.sql은 완벽하며 수정할 필요가 없습니다.**
    
2. **user.sql은 불필요:** 그 '빈 서랍장'에 들어갈 내용물(사용자 정보)은 사용자가 가입할 때마다 애플리케이션이 알아서 하나씩 채워 넣을 것이므로, 미리 INSERT 구문을 담은 파일을 만들 필요가 없습니다.


#### ERD와 별개로 아래에 있는 것도 만들어야 한다. 

![[Pasted image 20250620175939.png]]


c. 조직이 여러 엔터티로 구성된 경우, 정보 통합에 사용된 접근 방식을 설명해주세요.
	i. 접근법이 소수의 이익을 위한 정보 조정을 포함하는지 여부
	ii. 접근법이 법인 또는 법인의 일부에 대한 합병, 인수 및 처분을 고려하는 방법
	iii. 접근 방식이 이 표준의 공개와 중요한 주제에 따라 다른지 그 여부와 방법