# RDBMS & SQL

## RDBMS란?

- 관계형 데이터베이스 시스템
- `테이블`(데이터를 담을 수 있는 공간) 기반의 DBMS
    - 데이터를 테이블 단위로 관리
        - 하나의 테이블은 여러개의 칼럼으로 구성
    - 중복 데이터를 최소화 시킴
        - 같은 데이터가 여러 컬럼 또는 테이블에 존재 했을 경우, 데이터를 수정 시 문제가 발생할 가능성이 높아짐 → `정규화`(데이터를 쪼개는 것)
    - 여러 테이블에 분산되어 있는 데이터를 검색 시 테이블 간의 관계(`join`)를 이용하여 필요한 데이터를 검색
    
## SQL(Structured Query Language)

- **database에 있는 정보를 사용할 수 있도록 지원하는 언어**, 즉 SQL은 RDBMS에 저장된 데이터와 통신하기 위해 필요한 프로그래밍 언어
- 모든 DBMS에서 사용 가능
- **대소문자를 구별하지 않음(단, 데이터의 대소문자는 구분) → MySQL이나 MariaDB는 데이터의 대소문자도 구분하지 않음** -> 데이터의 대소문자는 구분하는 것이 좋음
  

> 사용법 : `use db이름;` 입력

대소문자 구분법
```sql
SELECT *
FROM employees
where binary(first_name) = 'Steven'; /*대소문자 구분, Steven 검색 됨*/

SELECT *
FROM employees
where binary(first_name) = 'steven'; /*대소문자 구분, Steven 검색 안됨*/

```

