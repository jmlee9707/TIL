[다른 명령어 - DDL, DCL, TCL](https://velog.io/@jmlee9707/DB-SQL%EB%AA%85%EB%A0%B9%EC%96%B4DDL-DML-DCL-TCL-%EB%8D%B0%EC%9D%B4%ED%84%B0-%ED%83%80%EC%9E%85)

# DML(Data Manipulation Language)
* DML : 데이터 조작어 
* 데이터 검색, 삽입, 변경, 삭제를 수행하여 조작하는 언어
* 실질적으로 저장된 데이터를 처리할 때 사용

|SQL문|설명|
|-----|-----|
|insert(C)|데이터베이스 객체에 데이터를 입력|
|select(R)|데이터베이스 객체에서 데이터를 조회|
|update(U)|데이터베이스 객체에 데이터를 수정|
|delete(D)|데이터베이스 객체에 데이터를 삭제|

## INSERT
생략이 가능한 field
* NULL이 허용된 컬럼
* DEFAULT가 설정된 컬럼
* AUTO INCREMENT가 설정된 컬럼

```sql
/*컬럼을 설정하지 않았을 떄, 컬럼개수와 순서 일치해야함*/
INSERT INTO table_name
VALUES(col_val1, col_val2, col_val3...);

/*컬럼을 설정시에 알아서 해당 컬럼에 들어감*/
INSERT INTO table_name(col_val1, col_val2, col_val3...)
VALUES(col_val1, col_val2, col_val3...);

/*여러개의 값 insert*/
INSERT INTO table_name(col_val1, col_val2, col_val3...)
VALUES(col_val1, col_val2, col_val3...),
		(col_val1, col_val2, col_val3...),
		(col_val1, col_val2, col_val3...);

```

```sql
-- 회원 정보 등록
-- 'kim', '김', '1234', 'kimkim', 'skim.com, 'ssafy.com' 등록시간
insert into member(userid, username, userpwd, emailid, emaildomain, joindate)
values('kim', '김', '1234', 'kimkim', 'skim.com', now());

-- '제니', 'jenny', '1234'
insert into member(username, userid, userpwd)
values('제니', 'jenny', '1234');
```

## UPDATE
* WHERE 절의 conditions(조건)에 만족하는 레코드의 값을 변경
* 주의 : WHERE 절을 생략하면 모든 데이터가 바뀐다

```sql
/*where절을 설정하지 않았을 경우 모두 바뀜*/
UPDATE member
set userpwd = '9876', emaildomain = 'kimkimkim.co.kr';

/*where절 설정*/
UPDATE member
set userpwd = '5432', emaildomain = 'kimkimkim.co.kr'
where userid = 'kim';
```

##	DELETE
* WHERE절의 conditions(조건)에 만족하는 레코드의 값을 삭제
* 주의 : WHERE절을 생략하면 모든 데이터가 삭제된다

```sql
DELETE from table_name
WHERE conditions;
```

```sql
-- userid가 kim 회원 탈퇴
DELETE from member
where userid = 'kim';
```

## SELECT
> select, from, where,  groupby, having, orderby...

```sql
SELECT * |{[ALL|DISTINCT] column | expression [alias], ...}
FROM table_name;
```
|select cause|description|
|---|---|
|* | FROM 절에 나열된 테이블에서 모든 열을 선택
|ALL| 선택된 모든 행을 반환, ALL이 default(생략가능)
|DISTINCT| 선택된 모든 행 중에서 중복 행 제거
|column| FROM절에 나열된 테이블에서 지정된 열을 선택
|expression|효션식은 값으로 인식되는 하나 이상의 값, 연산자 및 SQL 함수의 조합을 뜻함
|alias|별칭

### 기본 SELECT

```sql
-- 모든 사원의 모든 정보 검색.
select *
from employees;

-- 사원이 근무하는 부서의 부서번호 검색.
select department_id
from employees;


-- 사원이 근무하는 부서의 부서번호 검색.(중복제거)
select distinct department_id
from employees;

-- 회사에 존재하는 모든 부서. 사원이 존재하는+존재하지 않는
select *
from departments;

-- 모든 사원의 사번, 이름, 급여 검색.
select employee_id,first_name, salary
from employees;
-- 조건이 없으니까 where절 생략
```

### alias, 사칙연산(+-*/), NULL value
* null : 값이 없다가 아니라 알수 없다
* alias : as 사용하거나 한칸 띄어쓰고 진행, 별칭에 특수문자 사용하고 싶을시 쌍따옴표 사용
* IFNULL(null이 아닐경우, null일 경우)

```sql
-- 모든 사원의 사번, 이름, 급여, 급여 * 12 (연봉) 검색
select employee_id "사 번",first_name as "이 름", salary as "급 여", salary*12 as "연 봉"
from employees;
```

```sql
-- 모든 사원의 사번, 이름, 급여, 급여 * 12 (연봉), 커미션, 커미션포함 연봉 검색.
select employee_id "사 번",first_name as "이 름", salary as "급 여", salary*12 as "연 봉", commission_pct, salary*(1+ifnull(commission_pct,0))*12 "커미션포함 연봉"
from employees;
```

### case when then
* java : if-elseif-else 문과 동일 
```sql
Case exp1 WHEN exp2 THEN exp3
	[WHEN exp4 THEN exp5 ... ELSE exp6]
    END
```
```sql
-- 모든 사원의 사번, 이름, 급여, 급여에 따른 등급표시 검색.
-- 급여에 따른 등급
--   15000 이상 “고액연봉“      
--   8000 이상 “평균연봉”      
--   8000 미만 “저액연봉＂

select employee_id, first_name, salary,
	case when salary>=15000 then "고액연봉"
		when salary>=8000 then "평균연봉"
		else "저액연봉"
        end 등급
        from employees;

```

### AND, OR, NOT
```sql
-- 부서번호가 50인 사원중 급여가 7000이상인 사원의
-- 사번, 이름, 급여, 부서번호
select employee_id, first_name, salary, department_id
from employees
where department_id = 50 and salary>=7000;
```

```sql
-- 근무 부서번호가 50, 60, 70에 근무하는 사원의 사번, 이름, 부서번호
select employee_id, first_name, salary, department_id
from employees
where department_id = 50 or department_id=60 or department_id=70;

/*in 사용*/
select employee_id, first_name, salary, department_id
from employees
where department_id in(50, 60, 70);
```
```sql
-- 근무 부서번호가 50, 60, 70이 아닌 사원의 사번, 이름, 부서번호
select employee_id, first_name, salary, department_id
from employees
where department_id != 50 and department_id != 60 and department_id != 70;

select employee_id, first_name, salary, department_id
from employees
where department_id not in(50, 60, 70);
```
데이터베이스는 같다비교에서는 null값은 포함하지 않음
```sql
-- 급여가 6000이상 10000이하인 사원의 사번, 이름, 급여
select employee_id, first_name, salary, department_id
from employees
/*where salary >=6000 and salary<=10000;*/
where salary between 6000 and 10000;
```

** null 비교는 =로 할 수 없음 -> `is null`이나 `is not null`로 비교해야함!**
```sql
-- 근무 부서가 지정되지 않은(알 수 없는) 사원의 사번, 이름, 부서번호 검색.
select employee_id, first_name, salary, department_id
from employees
where department_id is null;
```

### Like (wild card : %, _)
* % : 갯수에 상관이 없음
* _ : 언더바 하나당 문자 하나

```sql
-- 이름에 'x'가 들어간 사원의 사번, 이름
select employee_id, first_name
from employees
where first_name like '%x%';
```
```sql
-- 이름에 'x'가 들어간 사원의 사번, 이름
select employee_id, first_name
from employees
where first_name like '%x%';
```

### orderBy - 정렬
* defalut 값은 오름차순 : asc
* 내림차 순은 desc
```sql
-- 모든 사원의 사번, 이름, 급여
-- 단 급여순 정렬(내림차순)
select employee_id, first_name, salary
from employees
order by salary desc;
```

```sql
-- 50, 60, 70에 근무하는 사원의 사번, 이름, 부서번호, 급여
-- 단, 부서별 정렬(오름차순) 후 급여순(내림차순) 검색
select employee_id, first_name,department_id, salary
from employees
where department_id in(50, 60, 70)
order by department_id desc, salary desc;
```