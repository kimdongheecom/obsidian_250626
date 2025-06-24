
#### sql 생성하는 프롬프팅(지금 한 작업은 테이블 생성하는 이미지를 생성한 것)

너는 DBA야. 테이블을 생성할껀데, 첫번째 테이블은 categories이고, 이것의 pk는 id이고, 텍스트 길이가 6글자야. 두번째 칼럼 명은 title이고, 길이는 50자 미만이야. 세번째 칼럼명은 year이고, 텍스트 길이는 4글자야. 이것의 자식은 disclosure가 되고, 일대 다의 관계야. disclosure 테이블은 pk로 칼럼명은 id이고, 텍스트 길이는 6글자야. 두번째 칼럼은 title이고, 텍스트 길이는 70자 이내야. 이것의 자식은 reqirements야. requirements table의 pk는 id이고, 텍스트 길이는 10자 미만이야. 두번째 칼럼은 question이고, 텍스트 길이는 1000자 이내야. 이것의 자식은 answer이야. 그리고 answer테이블의 pk는 id이고, 1부터 시작하는 오토 인크리먼트야. 두번째 컬럼명은 quant_data이고, 텍스트 길이는 3000자 이내야. answer은 requirements와 user와 sample의 교차 엔터티야.  user table의 pk는 id이고, 텍스트 길이는 30글자 미만이야. user table의 나머지 칼럼은 

{
 
  "name": "홍길동",
  "picture": "https://lh3.googleusercontent.com/a/...",
  "email": "example@gmail.com",
  "email_verified": true,
  "locale": "ko"
}

이고, 이 json의 키값과 동일하게 해주고, name 컬럼의 텍스트 길이가 30자 이내이고, picture 컬럼의 텍스트 길이는 70자 이내이고, email컬럼 텍스트 길이는 70자 이내이고,  email_verified 컬럼 텍스트 길이는 10자 이내이고, locale은 10자 이내야. sample table의 pk는 id이고, 1부터 시작하는 오토 인크리먼트야. 두번째 컬럼명은 qual_data이고, 텍스트 길이는 3000자 이내야. 

모든 자식 테이블은 FK로 부모 테이블에 id를 갖도록 해주는데, 이름은 부모 테이블의 이름_id로 해주고, 예를 들어, answer테이블의 FK는 user_id, sample_id, requirements_id로 하는것처럼 해줘. 



#### ERD를 생성하는 프롬프팅

너는 DBA야. 테이블을 생성할껀데, 첫번째 테이블은 categories이고, 이것의 pk는 id이고, 텍스트 길이가 6글자야. 두번째 칼럼 명은 title이고, 길이는 50자 미만이야. 세번째 칼럼명은 year이고, 텍스트 길이는 4글자야. 이것의 자식은 disclosures가 되고, 일대 다의 관계야. disclosures 테이블은 pk로 칼럼명은 id이고, 텍스트 길이는 6글자야. 두번째 칼럼은 title이고, 텍스트 길이는 70자 이내야. 이것의 자식은 reqirements야. requirements table의 pk는 id이고, 텍스트 길이는 10자 미만이야. 두번째 칼럼은 question이고, 텍스트 길이는 1000자 이내야. 이것의 자식은 answer이야. 그리고 answer테이블의 pk는 id이고, 1부터 시작하는 오토 인크리먼트야. 두번째 컬럼명은 quant_data이고, 텍스트 길이는 3000자 이내야. answer은 requirements와 user의 교차 엔터티야.  user table의 pk는 id이고, 텍스트 길이는 30글자 미만이야. user table의 나머지 칼럼은 

{
 
  "name": "홍길동",
  "picture": "https://lh3.googleusercontent.com/a/...",
  "email": "example@gmail.com",
  "email_verified": true,
  "locale": "ko"
}

이고, 이 json의 키값과 동일하게 해주고, name 컬럼의 텍스트 길이가 30자 이내이고, picture 컬럼의 텍스트 길이는 70자 이내이고, email컬럼 텍스트 길이는 70자 이내이고,  email_verified 컬럼 텍스트 길이는 10자 이내이고, locale은 10자 이내야. sample table의 pk는 id이고, 1부터 시작하는 오토 인크리먼트야. 두번째 컬럼명은 qual_data이고, 텍스트 길이는 3000자 이내야. 

모든 자식 테이블은 FK로 부모 테이블에 id를 갖도록 해주는데, 이름은 부모 테이블의 이름_id로 해주고, 예를 들어, answer테이블의 FK는 user_id, sample_id, requirements_id로 하는것처럼 해줘. 

