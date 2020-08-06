---
layout: post
title: MySQL
mathjax: true
tags:
  - MySQL
---

<br/>

### 마스터 테이블 생성
```sql
create table mysql.test_table1 (
  deptno double primary key,
  deptname varchar(20) not null
);
insert into mysql.test_table1 values (1, '총무팀');
insert into mysql.test_table1 values (2, '인사팀');
insert into mysql.test_table1 values (3, '재경팀');
insert into mysql.test_table1 values (4, '지원팀');

select * from mysql.test_table1;
```

### 서브 테이블 생성
```sql
create table mysql.test_table2 (
  empno int primary key,
  name varchar(20) not null,
  phone varchar(15) not null,
  deptno double not null,
  foreign key (deptno) references mysql.test_table1 (deptno) on delete cascade
);
insert into mysql.test_table2 values(1001, '홍길동', '010-1234-5678', 1);
insert into mysql.test_table2 values(2001, '피카츄', '010-4321-8756', 2);
insert into mysql.test_table2 values(2002, '라이츄', '010-7894-1111', 2);
insert into mysql.test_table2 values(3001, '꼬부기', '010-1111-2222', 3);

select * from mysql.test_table2;
```

### 테이블 속성 확인
```sql
desc mysql.test_table1;
desc mysql.test_table2;
```

#### 데이터 삭제 유의사항
- 서브테이블 on delete cascade 옵션으로 인해 마스터 테이블 row 삭제 시 해당 foreign key를 지닌 서브 테이블 데이터도 삭제됨
```sql
delete from mysql.test_table1 where deptno = 3;```

### 테이블명 변경
​```sql
alter table mysql.test_table1 rename to mysql.dept;
alter table mysql.test_table2 rename to mysql.emp;
select * from mysql.dept;
```

### 칼럼 추가
```
alter table mysql.emp add (address varchar(40));
select * from mysql.emp;
```

### 칼럼 변경
```sql
alter table mysql.emp modify address varchar(50);
desc mysql.emp;
```

### 칼럼 삭제
```sql
alter table mysql.emp drop column address;
desc mysql.emp;
```

### 칼럼명 변경
- Oracle SQL : ```alter table mysql.emp rename column (address) to (address2);```
- MySQL
  ```sql
  alter table mysql.emp change address address2 varchar(50);
  desc mysql.emp;
  ```

### 뷰 생성/조회/삭제
```sql
# 생성
create view mysql.view_emp as
  select * from mysql.emp;
# 조회 : select * from mysql.view_emp;
# 삭제 : drop view mysql.view_emp;
```



