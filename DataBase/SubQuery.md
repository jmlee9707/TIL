# 서브쿼리
> 서브쿼리란 **다른 쿼리 내부에 포함되어 있는 select 문**을 의미한다

- 서브 쿼리를 포함하고 있는 쿼리를 `외부쿼리(outer query)` 또는 메인 쿼리라고 부르며, 서브 쿼리는 `내부 쿼리(inner query)`라고도 부른다
- 서브 쿼리는 **비교 연산자의 오른쪽에 기술**해야 하고 **반드시 괄호(())로 감싸져 있어야만 한다**

## 종류
- `중첩 서브쿼리(Nested Subquery)` : **WHERE 문**에 작성하는 서브 쿼리
     - 단일행 (count 개수, sum 합계, min 최소값, max 최대값, avg 평균...)
     - 복수(다중) 행 : 여러개 나오는 것
     - 다중 컬럼 : 여러개의 컬럼을 얻어오는 것
 - `인라인 뷰(Inline View)` : FROM 문에 작성하는 서브 쿼리
   - view : 가상의 테이블
   - from 에 select하면 테이블로 나옴
 - `스칼라 서브 쿼리(Scalar Subquery)` : **SELECT 문에 작성하는 서브 쿼리**
 

### 주의사항
  - 서브 쿼리는 반드시 () 로 감싸야 한다
  - 서브 쿼리는 단일 행(>, <, =) 또는 다중행 비교 연산자(in, any all)와 함께 사용된다
    
### 서브쿼리가 사용 가능한 곳
* SELECT
* FROM
* WHERE
* HAVING
* ORDER BY
* INSERT 문의 VALUES
* UPDATE문의 SET

```sql
-- 사번이 100인 사원의 부서이름
-- join
select d.department_name
from employees e join departments d
on e.department_id = d.department_id
where e.employee_id = 100;

-- subquery

select department_name
from departments
where department_id = (select department_id
						from employees
						where employee_id = 100);
```

## NESTED SUBQUERY(중첩 서브쿼리)
> **WHERE 문**에 작성하는 서브 쿼리

### 1. 단일행
서브쿼리의 결과가 단일행을 리턴
```sql
-- 부서가 ‘seattle’(대소문자 구분X)에 있는 부서의 부서 번호, 부서 이름.
-- 단일행
select department_id, department_name
from departments
where location_id = (select location_id
					from locations
					where binary upper(city) = upper('seattle')
                    );
```
```sql
-- ‘adam’과 같은 부서에 근무하는 사원의 사번, 이름, 부서번호.
select employee_id, first_name, department_id
from employees
where department_id = (select department_id
						from employees
						where first_name = 'Adam'
                        );
```

```sql
-- 전체 사원의 평균 급여보다 많이 받는 사원의 사번, 이름, 급여.
-- 급여순 정렬
select employee_id, first_name,salary
from employees
where salary > (select avg(salary) from employees)
order by salary desc;
```

### 2. 다중행
서브쿼리의 결과가 다중행을 리턴 : IN, ANY(적어도), ALL
`in`
```sql
-- 근무 도시가 ‘seattle’(대소문자 구분X)인 사원의 사번, 이름.
-- 다중행 (in)'

select employee_id, first_name
from employees
where department_id in (
					select department_id
					from departments
					where location_id = (select location_id
										from locations
										where upper(city) = upper('seattle')
										)
							);
```
`any : 적어도 하나를 만족하면 `
```sql
-- 모든 사원 중 적어도(최소급여자보다) 30번 부서에서 근무하는 사원의 급여보다 많이 받는 사원의 사번, 이름, 급여, 부서번호
-- 다중행 (any
select employee_id, first_name, salary, department_id
from employees
where salary > any (
					select salary
					from employees
                    where department_id = 30
            );
```
`all : 모두 만족하면`
```sql
-- 30번 부서에서 근무하는 모든(최대급여자보다) 사원들보다 급여를 많이 받는 사원의 사번, 이름, 급여, 부서번호.
-- 다중행 (all)
select employee_id, first_name, salary, department_id
from employees
where salary > all (
					select salary
					from employees
                    where department_id = 30
            );

```
### 3. 다중열
서브쿼리의 결과가 다중열을 리턴
```sql
-- 다중열
-- 커미션을 받는 사원중 매니저 사번이 148인 사원의 급여와 부서번호가 일치하는 사원의 사번, 이름
select employee_id, first_name
from employees
where(salary, department_id) in  (
			select salary, department_id
			from employees
			where commission_pct is not null 
            and manager_id = 148
            );
```

## INLINE VIEW(인라인뷰)
> FROM 문에 작성하는 서브 쿼리

* 서브쿼리가 FROM절에 사용되면 뷰(View)처럼 결과가 동적으로 생성된 테이블로 사용이 가능하다
* 임시적인 뷰이기 때문에 데이터베이스에는 저장되지 않는다
* 동적으로 생성된 테이블이기 떄문에 column을 자유롭게 참조할 수 있다
* MySQL에서는 limit 활용이 가능하다

```sql
-- 인라인뷰(Inline View)
-- 모든 사원의 평균 급여보다 적게 받는 사원들과 같은 부서에서 근무하는 사원의 사번, 이름, 급여, 부서번호
select employee_id, first_name, salary, a.department_id
from employees e join(
			select department_id
			from employees
			where salary <(select avg(salary) from employees)
            ) a 
on e.department_id = a.department_id;
```

### TopN 질의
```sql
-- TopN 질의
-- 모든 사원의 사번, 이름, 급여를 출력.(단 아래의 조건 참조)
--   1. 사원 정보를 급여순으로 정렬.
--   2. 한 페이지당 5명이 출력.
--   3. 현재페이지가 3페이지라고 가정. (급여 순 11등 ~ 15등까지 출력)
set @pageno = 3;

select b.rn, b.employee_id, b.first_name, b.salary
from (
	  select @rownum := @rownum + 1 as rn, a.*
	  from (
		    select employee_id, first_name, salary
		    from employees
		    order by salary desc
		   ) a, (select @rownum := 0) tmp
	 ) b
where b.rn > (@pageno * 5 - 5) and b.rn <= (@pageno * 5);
```

## Scalar SubQuery
> SELECT 절에 있는 서브 쿼리

* 한개의 행만 반환

```sql
-- 60번 부서에 근무하는 사원의 사번, 이름, 급여, 부서번호, 60번부서의 평균급여
select e.employee_id, e.first_name, salary, department_id, 
	(select avg(salary) from employees where department_id = 60) as avg60
    from employees e 
    where department_id = 60;
```