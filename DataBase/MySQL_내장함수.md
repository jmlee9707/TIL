# MySQL 내장 함수
## 숫자 관련 함수

ABS(숫자) : 절대값
```sql
select abs(-5), abs(0), abs(+5)
from dual;
```

ceiling(숫자) or ceil(숫자) : 값보다 큰 정수 중 가장 작은 수(올림)
```sql
-- 13  13  -12  -12
select ceil(12.2), ceiling(12.2), ceil(-12.2), ceiling(-12.2)
from dual;
```

floor(숫자) : 값보다 작은 정수 중 가장 큰 수(버림)
```sql
-- 12  -13
select floor(12.6), floor(-12.2)
from dual;
```
 round(숫자, 자릿수) : 숫자를 자릿수를 기준으로 반올림. 
```sql
-- 1526  1526  1526.2  1526.16  1530  2000
select round(1526.159), round(1526.159, 0), round(1526.159, 1), 
	   round(1526.159, 2), round(1526.159, -1), round(1526.159, -3)
from dual;
```
truncate(숫자, 자릿수) : 숫자를 자릿수를 기준으로 버림
```sql
-- 1526  1526.1  1526.15  1520  1000
select truncate(1526.159, 0), truncate(1526.159, 1), 
	   truncate(1526.159, 2), truncate(1526.159, -1), truncate(1526.159, -3)
from dual;
```
pow(X, Y) or Power(X, Y) : X의 Y승
```sql
-- 8  8
select pow(2, 3), power(2, 3)
from dual;
```
MOD(분자, 분모) : 분자를 분모로 나눈 나머지
```sql
-- 2  2
select mod(8, 3), 8 % 3
from dual;
```
greatest(숫자1, 숫자2, 숫자3,...) : 주어진 수에서 가장 큰 수를 반환
least(숫자1, 숫자2, 숫자3,...) : 주어진 수에서 가장 작은 수를 반환
```sql
-- 9  3
select greatest(4, 3, 7, 5, 9), least(4, 3, 7, 5, 9)
from dual;
```
## 문자 관련 함수
ASCII(문자) : 문자의 아스키 코드 값 리턴
```sql
-- 48	65	97
select ASCII('0'), ASCII('A'), ASCII('a')
from dual;
```
concat('문자열', '문자열2', '문자열3') 문자열들을 결합
```sql
-- 100번 사원의 이름 Steven King
select concat(employee_id, '번 사원의 이름 ', first_name,' ' , last_name)
from employees
where employee_id = 100;
```
INSERT('문자열', 시작위치, 길이, '새로운 문자열') : 문자열 중 기존 문자열을 바뀔 문자열로 변경
```sql
-- hello jenny !!!
select insert('helloabc!!!', 6, 3, ' jenny ')
from dual;
```
replace('문자열','기존문자열', '바뀔문자열') : 문자열 중 기존 문자열을 바뀔 문자열로 변경
```sql
-- hello jenny !!!
select replace('helloabc!!!', 'abc', ' jenny ')
from dual;
```
instr('문자열', '찾는 문자열') : 문자열 중 찾는 문자열의 위치 값을 리턴
```sql
-- 7
select instr('hello jenny !!!', 'jenny')
from dual;
```
mid('문자열', 시작위치, 개수) : 문자열 중 시작위치부터 개수만큼 리턴
substring('문자열', 시작위치, 개수) : 문자열 중 시작위치부터 개수만큼 리턴
```sql
-- jenny
select mid('hello jenny !!!', 7, 5), substring('hello jenny !!!', 7, 5)
from dual;
```
reverse('문자열') : 문자열을 반대로 나열
```sql
hello jenny !!!
select reverse('!!! ynnej olleh')
from dual;
```
lcase('문자열') or lower('문자열') : 모든 문자를 소문자로 변경
```sql
-- hello jenny !!!  hello jenny !!!
select lower('hELlo JennY !!!'), lcase('hELlo jeNNy !!!')
from dual;
```
ucase('문자열') or upper('문자열') : 모든 문자를 대문자로 변경
```sql
-- HELLO JENNY !!!  HELLO JENNY !!!
select upper('hELlo jeNny !!!'), ucase('hELlo JenNy !!!')
from dual;
```
left('문자열', 개수) : 문자열 중 왼쪽에서 개수만큼 추출
right('문자열', 개수) : 문자열 중 오른쪽에서 개수만큼 추출
```sql
-- hello  ny !!!
select left('hello jenny !!!', 5), right('hello jenny !!!', 6)
from dual;
```

## 날짜 관련 함수
now() or sysdate() or current_timestamp() : 현재 날짜와 시간 리턴
```sql
-- 2022-03-16 01:06:43	2022-03-16 01:06:43	2022-03-16 01:06:43
select now(), sysdate(), current_timestamp()
from dual;
```
curdate() or current_date(): 현재 날짜 리턴
curtime() or current_time() : 현재 시간 리턴
```sql
-- 2022-03-16	2022-03-16	01:06:35	01:06:35
select curdate(), current_date(), curtime(), current_time()
from dual;
```
date_add(날짜, INTERVAL 기준 값) : 날짜에서 기준 값만큼 더한다.
date_sub(날짜, INTERVAL 기준 값) : 날짜에서 기준 값만큼 뺀다.
```sql
-- 2022-03-16 01:06:27	2022-03-16 01:06:32	2022-03-16 06:06:27	2022-03-21 01:06:27
select now() 현재시간, date_add(now(), interval 5 second) 5초후,
	   date_add(now(), interval 5 hour) 5시간후, date_add(now(), interval 5 day) 5일후
from dual;
```
* year(날짜) : 날짜의 연도 리턴
* month(날짜) : 날짜의 월 리턴
* monthname(날짜) : 날짜의 월을 영어로 리턴
* dayname(날짜) : 날짜의 요일을 영어로 리턴
* dayofmonth(날짜) : 날짜의 월별 일자 리턴
* dayofweek(날짜) : 날짜의 주별 일자 리턴 [일요일(1), 월요일(2), ..., 토요일(7)]
* weekday(날짜) : 날짜의 주별 일자 리턴 [월요일(0), 화요일(1),...,일요일(6)]
* dayofyear(날짜) : 일년을 기준으로 한 날짜까지의 일수(365일 중 x일)
* week(날짜) : 일년 중 몇 번째 주
```sql
-- 2022	3	March	Wednesday	16	4	2	75	11
select year(now()), month(now()), monthname(now()), 
       dayname(now()), dayofmonth(now()), dayofweek(now()), 
	   weekday(now()), dayofyear(now()), week(now())
from dual;
```
date_format(날짜, '형식') : 날짜를 형식에 맞게 리턴
* 날짜 형식
*  %Y 년, 2022
* %y 년, 22
* %b 월, Jan,..,Dec
* %M 월, January..December
* %m 월, 01,02, ..., 11, 12
* %d 일, 01,...,30,31
* %e 일, 1, 2,..., 30,31
* %a 요일, sun...sat
* %W 요일, Sunday, ... Saturday
* %w 요일, 0 : 일요일, ..., 6: 토요일
* %p AM or PM
* %H 시간, 00, 01, 02, ...22,23
* %h 시간, 01,02, .., 11, 12
* %I 시간, 01,02, .., 11, 12
* %k 시간, 0,1,2,...,22,23
* %l 시간, 1,2,...,11,12
* %i 분, 00..59
* %S 초, 00..59
* %s 초, 00..59
* %T 시간, 24-houer(hh:mm:ss)
*  %j 1년중 X일, 001,...365)
```sql
-- 2022-03-16 01:05:54	2022 March 16 AM 1 05 54	22-03-16 01:05:54	22.03.16 Wednesday	01시05분54초	01:05:54	075
select now(), date_format(now(), '%Y %M %e %p %l %i %S'), date_format(now(), '%y-%m-%d %H:%i:%s'),
	   date_format(now(), '%y.%m.%d %W'), date_format(now(), '%H시%i분%s초'), date_format(now(),'%T'),date_format(now(),'%j') 
from dual;
```
## 논리 관련 함수
if(논리식, 값1, 값2) : 논리식이 참이면 값1 리턴, 거짓이면 값2 리턴
ifnull(값1, 값2) : 값1이 null이면 값2로 대치, null이 아니면 값1 리턴
nullif(값1, 값2) : 값1=값2dl true이면 null 그렇지 않으면 값1이 리턴
```sql
-- 크다  작다  null 3  b  a
select if(3 > 2, '크다', '작다'), if(3 > 5, '크다', '작다'), 
       nullif(3, 3), nullif(3, 5), 
	   ifnull(null, 'b'), ifnull('a', 'b')
from dual;
```
## 집계 관련 함수
* count(필드명 : null 값이 아닌 레코드 수를 리턴
* sum(필드명) : 필드명에 해당하는 레코드 값의 합계를 리턴
* avg(필드명) : 각각의 그룹 안에서 필드명에 해당하는 레코드 값의 평균을 리턴
* max(필드명) : 필드명에 해당하는 레코드 값 중 최대값을 리턴
* min(필드명) : 필드명에 해당하는 레코드 값 중 최소값을 리턴
```sql
-- 사원의 총수, 급여의 합, 급여의 평균, 최고급여, 최저급여
select count(employee_id), sum(salary), avg(salary), max(salary), min(salary)
from employees;
```

```sql
-- 50번 부서에서 근무하는 사원의 사원수
select count(employee_id)
from employees
where department_id = 50;
```

```sql
-- 그게 누구?
select employee_id, max(salary)
from employees
where department_id = 50;
```

```sql
-- 진짜 받는지 확인 - 오류 발생
select *
from employees
where employee_id = 120;
```

## 함수
1. `단일행 함수` : input 레코드 하나하나마다 결과값을 반환
	- 숫자함수,문자함수,날짜함수
2. `다중행 함수(그룹 함수)` : 그룹당 하나의 결과값을 반환, 하나 이상의 행을 그룹으로 묶어 연산하여 총합, 평균 등을 하나의 결과로 반환
	- sum, avg, count, min, max


```sql
-- sum : 그룹의 누적 합계를 반환
-- 급여의 총합, 커미션의 총합
select sum(salary) 급여총합, sum(salary*commission_pct) 커미션 총합
from employees;
```

```sql
-- avg : 그룹의 평균 반환
-- 평균 급여 (소수 2자리까지 표현)
select round(avg(salary),2)
from employees;
```

```sql
-- count : 그룹의 총 개수를 반환
-- 사원수?
select count(employee_id) 사원수
from employees;
```

```sql
-- 사원이 근무하는 부서의 개수, 커미션을 받은 사원수.(null 제외하고 카운팅)
select count(department_id) 부서수, count(commission_pct) "커미션받은 사원수" 
from employees;
-- where commission_pct is not null;
select count(distinct department_id) 부서수 
from employees;
```

```sql
-- max : 그룹의 최대값을 반환
-- min : 그룹의 최소값을 반환
-- 80번 부서에서 근무하는 사원 중 최고급여, 최저급여
select max(salary) 최고급여, min(salary) 최저급여
from employees
where department_id = 80;
```

```sql
-- mySQL에 저장된 데이터에대한 유효성 검사의 범위를 설정하는 시스템 변수 
-- mysql 5.7??버전에서 생김
select @@sql_mode;
```
```sql
-- only_full_group_by : having이나 order by 목록이 
-- group by 절에서 명명되지 않았거나 기능적으로 종속되지 않은 
-- 혹은 집계되지 않은 열 참조시 쿼시 거부-->에러
set sql_mode = "STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION,ONLY_FULL_GROUP_BY";
```

```sql
-- groupby 절
-- select문에서 group by절을 사용하는 경우 DB는 쿼리 된 테이블의 행을 그룹으로 묶음 --> 조회하려는 레코드를 그룹핑 기준으로 그룹을 나누는 것
-- group by 생략 --> DB는 전체 레코드를 하나의 그룹으로 취급 : 다라서 집계함수 적용하면 하나의 결과 리턴
-- group by 그룹1, 그룹2, ... -> 그룹 개수만큼의 결과
-- select 문의 집계수를 각 그룹에 적용하고 각 그룹에 대해 단일 결과 행 반환
-- select 절의 모든 요소는 group by절의 표현식, 집계 함수를 포함하는 표현식 또는 상수만 가능 

-- 부서 번호, 부서별 급여의 총합, 평균급여
-- department_id 는 다중행 반환 / sum, avg는 단일행 반환
select department_id, sum(salary), avg(salary)
from employees;

-- 올바른 형태
select department_id, sum(salary), avg(salary)
from employees
group by department_id;
```