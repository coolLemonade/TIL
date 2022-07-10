# MySQL 기초

## MySQL 사용법
### 0. MySQL
**SQL**(*Structured Query Language*)은 관계형 데이터 베이스를 다룰 때 사용하는 표준 언어로, 데이터를 표로 표시한다.   
**RDBMS**(*Realational DataBase Management System*)은 관계형 데이터베이스를 관리하는 시스템을 말하며, **MySQL**도 RDBMS다.   

### 1. MySQL에 접속하기
**1-1. Terminal을 통해서 접속하기**   
먼저, MySQL이 설치된 디렉토리로 찾아가야 한다.
아래와 같이 mysql 디렉토리의 bin에 있는 mysql을 실행한다.
```
/mysql -uroot -p
```
위 명령어를 입력하고 비밀번호를 입력한다.   
-uroot는 유저로 루트에 접근하겠다는 뜻이고, -p는 따로 패스워드를 입력하겠다는 뜻이다.   
(-p를 입력하지 않으면 입력하는 패스워드가 그대로 노출된다.)

### 2. Database 생성하기
**2-1. Database 생성하기**   
명령어 **Create**는 Database를 생성한다.
character set은 utf8-general-ci로 설정해 놓는 것이 좋다.
```
CREATE DATABASE <DATABASE NAME> CHARACTER SET utf8 COLLATE
utf8_general_ci;
```
**2-2. Database 보기**   
MySQL의 Database들을 모두 보여주는 명령어는 아래와 같다.
```
show database;
```
**2-3. Database 사용(선택)하기**   
명령어 **USE**로 사용할 Database를 선택한다.
```
USE <DATABASE NAME>;
```
**2-4. Database 삭제하기**   
명령어 **Drop**은 Database를 삭제한다.

### 3. Table 생성하기
**3-1. Table 생성하기**   
명령어 **CREATE**를 사용하여 테이블을 생성한다.   
테이블 생성할 때, 각 column의 data type을 결정한다.
```
CREATE TABLE <TABLE NAME> (COLUMN NAME1 data_type, COLUMN NAME2 data_type...)
```
- **data type**
  - Text
  - INT
  - VARCHAR
  - Number
  - Date   

**3-2. Table 보기**   
명령어 **SHOW**로 데이터베이스 아래에 있는 테이블을 확인할 수 있다.
```
show tables;
```
명령어 **DESC**(*describe*)로 해당 Table 구조를 확인할 수 있다.
```
desc TABLE NAME;
```

### 4. CRUD 명령어   
**Create, Read, Update, Delete**의 약자인 **CRUD는 데이터를 다루는 가장 기본적인 동작**이다.   
   
**4-1. READ할 때는 SELECT를 사용한다.**   
```
방법 1. SELECT <COLUMN NAME1, COLUMN NAME2> FROM <TABLE NAME> WHERE <조건>
방법 2. SELECT <COLUMN NAME> FROM <TABLE NAME> WHERE <조건> LIMIT <조회할 레코드 수>
방법 3. SELECT * FROM TABLE NAME;
방법 4. SELECT * FROM <TABLE NAME> GROUP BY <그룹핑할 기준 컬럼명>
방법 5. SELECT * FROM <TABLE NAME> ORDER BY <정렬의 기준이 될 컬럼명><DESC(내림차순) 또는 ASC(오름차순)>
```
**4-2. CREATE할 때는 INSERT를 사용한다.**   
```
insert into TABLE NAME(KEY1, KEY2) values(VALUE1, VALUE2)
``` 
**4-3. UPDATE할 때는 UPDATE를 사용한다.**   
set 뒤에 변경할 "컬럼명 = 값"을 넣고, where절에 변경하고자 하는 row를 찾기 위한 조건을 붙인다.   
update는 해당 하는 데이터를 SET으로 변경합니다. 예시에서 SET은 description='ORACLE is ...', title='Oracle'을 가리킨다.
```
UPDATE topic SET description='ORACLE is ...', title='Oracle' WHERE ID = 1;
```
**4-4. DELETE할 때는 DELETE를 사용한다.**   
명령어 delete와 삭제하고자 하는 row를 where절로 찾아준다.   
where절을 붙이지 않으면, 테이블의 모든 데이터가 삭제되므로 주의해야 한다.
```
DELETE FROM topic WHERE id = 1;
``` 

**출처** [MySQL의 기본 사용법](https://developer88.tistory.com/20)과 [MySQL 기초 정리](https://ahn3330.tistory.com/85)을 정리한 문서이다.
