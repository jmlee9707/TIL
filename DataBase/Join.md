# Join
## join 이란?
- 둘 이상의 데이터가 필요한 경우 테이블 조인이 필요
- 일반적으로 조인 조건을 포함하는 `where 절`을 작성해야 한다. **(n-1 개의 조건 설정 필요)**
    - ON : 조인 조건, where : 일반 조건
- 조인 조건은 일반적으로 각 테이블의 PK 및 FK로 구성
    - PK : unique(유일 값, 중복X) + not null → 내부적으로 index 자동 생성
    - FK : 참조키, 이웃키, 외래키 (다른 테이블의 값을 집어 넣을 수 있음), null 허용

## 종류
- `INNER JOIN` :
    - on 을 이용한 join 조건 지정 (join 조건 : on, 일반 조건 : where)
    - using을 이용한 join 조건 지정 : using 절에서는 table이름이나 alias를 명시하면 error
- `OUTER JOIN` :
    - LEFT OUTER JOIN
    - RIGHT OUTER JOIN
- `SELF JOIN` :
    - 같은 테이블 끼지 join
- `None-Equi Join` :
    - table의 PK, FK가 아닌 일반 column을 join 조건으로 지정
    - 모든 사원의 사번, 이름, 급여, 급여 등급 → 범위검색
    
### JOIN 조건 명시에 따른 구분
- NATURAL JOIN
- CROSS JOIN(FULL JOIN, CARTESIAN JOIN)

    
## JOIN 시에 주의할 점
- INNER JOIN : 어느 테이블을 먼저 읽어도 결과가 달라지지 않아 MySQL 옵티마이저가 **조인의 순서를 조절해서 다양한 방법으로 최적화를 수행할 수 있다**
- OUTER JOIN : 반드시 OUTER가 되는 테이블을 먼저 읽어야 하므로 **옵티마이저가 조인 순서를 선택할 수 없다**

### 사용법

** 소유권 명시 -> alias ** 
```sql
-- 사번이 100인 사원의 사번, 이름, 급여, 부서이름
select e.employee_id 사번, e.first_name 이름, e.salary 급여, d.department_name 부서이름
from employees e, departments d /*alias*/
where e.employee_id = d.department_id 
and e.employee_id= 100;

```
# JOIN 종류
## INNER JOIN
* 가장 일반적인 JOIN의 종류이며 `교집합`이다
* 동등 조인(Equi-join)이라고도 하며, N개의 테이블 조인시 `n-1개의 조인 조건`이 필요하다.
* on 을 이용한 join 조건 지정 (join 조건 : on, 일반 조건 : where)
* using을 이용한 join 조건 지정 : using 절에서는 table이름이나 alias를 명시하면 error
* inner 생략 가능, 그냥 join만 사용가능 -> innner join은 가장 일반적인 조인


### 형식
```sql
-- inner join
select column1, column2, column3
from table1 inner join table2
on table1.column = table2.column
```

코드 예시 : ON 사용!
```sql
select e.employee_id 사번, e.first_name 이름, e.salary 급여, d.department_name 부서이름
from employees e inner join departments d
on e.department_id = d.department_id /*inner 조건*/ 
where e.employee_id= 100; /*일반조건*/
```
join 조건은 n-1개!!!
```sql
-- 근무하는 도시 
select e.employee_id 사번, e.first_name 이름, e.salary 급여, d.department_name 부서이름, l.city
from employees e inner join departments d inner join locations l
on e.department_id = d.department_id /*inner 조건*/ 
and d.location_id = l.location_id /*inner 조건*/ 
where e.employee_id= 100; /*일반조건*/
```

Using 이용한 Join 조건
```sql
-- using, 공통으로 가지고 있는 컬럼일때 

select e.employee_id 사번, e.first_name 이름, e.salary 급여, department_id, d.department_name 부서이름
from employees e inner join departments d
using (department_id)
where e.employee_id= 100;
```
## NATURAL JOIN
* natural join은 알아서 같은 이름의 column을 join 조건으로 걸어줌
* 주의 : 모든 것들이 만족할때만 사용
* 동일한 컬럼이 여러개 있는 경우 원하는 결과를 얻을 수 없다


```sql
-- natural join
select e.employee_id 사번, e.first_name 이름, e.salary 급여, department_id, d.department_name 부서이름
from employees e natural join departments d -- 32, 동일한 컬럼이 여러개 있기 때문에 원하는 결과를 얻을 수 없다
where e.employee_id= 100;
```

## OUTER JOIN
* LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN(MYSQL엔 없음)
* 어느 한쪽 테이블에는 해당하는 데이터가 존재하는데 다른쪽 테이블에는 데이터가 존재하지 않을 경우 **그 데이터가 검색되지 않는 문제점을 해결하기 위해 사용**

`left join`
* 왼쪽 테이블을 기준으로 JOIN 조건에 일치하지 않은 데이터까지 출력
```sql
-- 회사에 근무하는 모든 사원의 사번, 이름, 부서이름
-- 회사에 근무하는 사원수 
-- 107명

select e.employee_id, e.first_name, ifnull(d.department_name, '승진발령')
from employees e left join departments d /*왼쪽 테이블의 모든 데이터를 뿌리고 싶을 때*/
on e.department_id = d.department_id;
```
`right join`
* 오른쪽 테이블을 기준으로 JOIN 조건에 일치하지 않는 데이터까지 출력
```sql
-- 회사에 존재하는 모든 부서의 부서이름과 부서에서 근무하는 사원의 사번, 이름
-- 회사의 부서수 >> 27
select count(distinct department_id)
from departments;

-- 사원이 근무하는 부서수 >> 11
select count(distinct department_id)
from employees;

-- 사원이 없는 부서의 정보는 출력이 안됨 -> right join 사용.
select department_id, department_name, employee_id, first_name
from employees e right outer join departments dbtest
using(department_id); -- 122 

-- 해결
select ifnull(d.department_name, '승진발령'), e.employee_id, e.first_name
from employees e left join departments d
on e.department_id = d.department_id
union -- 합집합 
select department_name, employee_id, first_name
from employees e right join departments dbtest
using(department_id); -- 122 
```

## SELF JOIN
* 같은 테이블끼리 JOIN
```sql
-- self join
-- 모든 사원의 사번, 이름, 매니저사번, 매니저이름=사원 이름
select e.employee_id, e.first_name, e.manager_id, ifnull(m.first_name, '우리대장님')
from employees e left outer join employees m
on e.manager_id = m.employee_id;
```

## Non-Equi JOIN
* table의 PK, FK가 아닌 일반 column을 join 조건으로 지정
* =로 비교하지 않는 join, 범위 검색
```sql
-- None-Equi join
-- 모든 사원의 사번, 이름, 급여, 급여등급, -- 내가 받는 급여가 어느 등급에 속하는지 
select e.employee_id, e.first_name, e.salary, s.grade
from employees e join salgrades s
on e.salary between s.losal and s.hisal;
```

---
### 실습
1.
```sql

-- 사번이 101인 사원의 근무 이력.
-- 근무 당시의 정보를 아래를 참고하여 출력.
-- 출력 컬럼명 : 사번, 이름, 부서이름, 직급이름, 시작일, 종료일
-- 날짜의 형식은 00.00.00

select e.employee_id, e.first_name, d.department_name, j.job_title,
date_format(h.start_date,'%y.%m.%e'),date_format(h.end_date,'%y.%m.%e')
from employees e join job_history h
on e.employee_id = h.employee_id
join departments d -- 과거의 부서 기준
on h.department_id = d.department_id
join jobs j
on j.job_id = h.job_id
where e.employee_id = 101;

-- select *
-- from job_history
-- where employee_id = 101;
```

2.
```sql

-- 위의 정보를 출력 하였다면 위의 정보에 현재의 정보를 출력.
-- 현재 근무이력의 시작일은 이전 근무이력의 종료일 + 1일로 설정.
-- 종료일은 null로 설정.
select e.employee_id, e.first_name, d.department_name, j.job_title,
date_format(h.start_date,'%y.%m.%e'),date_format(h.end_date,'%y.%m.%e')
from employees e join job_history h
on e.employee_id = h.employee_id
join departments d -- 과거의 부서 기준
on h.department_id = d.department_id
join jobs j
on j.job_id = h.job_id
where e.employee_id = 101;

```

3.
```sql
-- 101번 사원의 과거 이력 중 마지막 이력의 종료날짜 + 1
select e.employee_id, e.first_name, d.department_name, j.job_title,
date_format(h.start_date,'%y.%m.%e'),date_format(h.end_date,'%y.%m.%e')
from employees e join job_history h
on e.employee_id = h.employee_id
join departments d -- 과거의 부서 기준
on h.department_id = d.department_id
join jobs j
on j.job_id = h.job_id
where e.employee_id = 101
union
select e.employee_id, e.first_name, d.department_name, j.job_title, 
(select date_format(date_add(max(end_date), interval 1 day), '%y.%m.%d') from job_history where employee_id=101), 
null
from employees e 
join departments d -- 과거의 부서 기준
on e.department_id = d.department_id
join jobs j
on j.job_id = e.job_id
where e.employee_id = 101;
```

