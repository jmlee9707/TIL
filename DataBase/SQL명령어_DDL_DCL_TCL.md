# SQL 명령어
SQL 명령어에는 크게 DDL, DML, DCL, TCL으로 4가지 그룹으로 나뉜다.

## DDL(Data Definition Language)
* DDL : 데이터 정의어
* 데이터베이스 스키마를 처리하고 설명을 정의하는 언어
* 데이터베이스 객체(table, view, index, ...)의 구조를 정의
* 테이블 생성, 컬럼 추가, 타입 변경, 제약조건 지정, 수정 등

|SQL문|설명|
|-------|----------|
|create|데이터베이스 객체를 생성|
|drop|데이터베이스 객체를 삭제|
|alter|기존에 존재하던 데이터베이스 객체를 수정|

### 데이터 타입
[data type_click](https://www.techonthenet.com/mysql/datatypes.php)

#### 자주 사용하는 데이터 타입 정리
|문법|최대 크기|설명|+|
|---|---|---|---|
|VARCHAR(size)|255자|문자를 저장한다. 문자열의 길이는 `가변적`이다.|VARCHAR(20)인 칼럼에 10자만 저장하면 실제로도 10자만큼의 기억장소를 차지|
|CHAR(size)|255자|255자의 문자를 저장한다. 문자열의 길이는 `고정적`이다.|CHAR(20)인 칼럼에 10자만 저장하더라도 20자 만큼의 기억장소를 차지|
|TEXT(size)|65536(2^16-1)byte|
|INT(m)|표준 integer 값. -2147483648 ~ 2147483647.|m은 정수의 크기가 아닌 자릿수 개수이다.|
BIGINT(m)|큰 integer 값. -9223372036854775808 ~ 9223372036854775807|m은 정수의 크기가 아닌 자릿수 개수이다.|
FLOAT(m,d)|	단일 정밀 부동 소수점 숫자	|m은 정수 자릿수, d는 소수점 아래 자릿수 개수이다.
DATE|	‘1000-01-01’ ~ ‘9999-12-31’(3byte)	|‘YYYY-MM-DD’로 표기된다.
TIME|	‘-838:59:59’ ~ ‘838:59:59’(3byte)|‘HH:MM:SS’로 표기된다.|
DATETIME|8byte|‘YYYY-MM-DD HH:MM:SS’로 표기된다.|timezone의 영향을 받음
TIMESTAMP|4byte|‘YYYY-MM-DD HH:MM:SS’로 표기된다. index가 더 빠르게 생성된다|우리나라 시간, 다른 나라 시간을 다르게 표시, timezone의 영향을 받음

### 제약 조건

|제약조건|description|
|---|---|
|NOT NULL|null값을 저장할 수 없고 반드시 쿼리문을 이용하여 값을 지정|
|UNIQUE|컬럼에 중복된 값을 저장할 수 없음, NULL 값은 허용|
|DEFAULT value| 값이 전달되지 않을 때(NULL값이 들어올 경우) 추가되는 기본값 설정|
|PRIMARY KEY| `기본키` 라고 부르며 컬럼에 중복된 값을 저장할 수 없고 NULL 값도 허용하지 않음. 주로 ROW를 구분하기 위한 유일한 값을 지정할 때 사용. **중복되지 않는 유일한 값**|
|FOREIGN KEY| `참조키`, `외래키` 라고 부르며 특정 테이블의 PK 컴럼에 저장되어 있는 값만 저장.(다른 테이블에 있는 값을 가져옴) NULL값은 허용, references를 이용하여 어떤 컬럼에 어떤 데이터를 참조하는지 반드시 지정|
|CHECK|값의 범위나 종류를 지정, MYSQL 8.0.16부터 사용가능(이전 버전의 경우 설정은 되나 적용이 안됨)


### 데이터베이스 생성
```sql
/*데이터 베이스 생성*/
create database 데이터베이스명;

/*다국어 처리 : utf8md3*/
create database 데이터베이스명
default character set utf8mb3 collate utfmb3_general_ci;

/*이모지 문자까지 처리*/
create database 데이터베이스명
default character set utf8mb4 collate utfmb4_general_ci;
```

### 데이터베이스 변경
```sql
/*데이터 베이스 변경 -> dbtest의 charcter set,collate 변경*/
alter database dbtest
default character set utf8mb4 collate utfmb4_general_ci;
```

### 데이터베이스 삭제
```sql
/*데이터 베이스 삭제*/
drop database dbtest;
```

### 테이블 생성
``` sql
create table table_name(
	column_name1 Type[optional attributes],
    column_name2 Type[optional attributes],
    column_name3 Type[optional attributes],
    )
```

```sql
create table test(
	val varchar(10)
);

insert into test(val)
values ('a');

insert into test(val)
values ('b');

select *
from test;
```

```sql
create table member(
	idx			int			auto_increment,
	userid		varchar(16)	not null,
	username		varchar(20),
	userpwd		varchar(16),
	emailid		varchar(20),
	emaildomain	varchar(50),
	joindate		timestamp	default	current_timestamp,
    primary key (idx)
);
```


## DCL(Data Control Language)
* DCL : 데이터 제어어
* DB, Table의 접근권한이나 CRUD 권한을 정의
* 특정 사용자에게 테이블의 검색권한 부여/금지 등

|SQL문|설명|
|-----|-----|
|grant|데이터베이스 객체에 권한을 부여|
|revoke|데이터베이스 객체 권한 취소|

## TCL(Transaction Control Language)
* TCL : 트랜잭션 제어어
* transaction이란 데이터베이스의 논리적 연산 단위

|SQL문|설명|
|-----|-----|
|commit|실행한 Query를 최종적으로 적용|
|rollback|실행한 Query를 마지막 commit 전으로 취소시켜 데이터를 복구|

