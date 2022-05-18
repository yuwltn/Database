## MySQL 접속

```sql
service mysql start
mysql -u root -p
```

## MySQL 닫기

```sql
EXIT;
```

## DBMS의 관리 DB인 mysql 데이터베이스 연결
```sql
use mysql;
```

## 데이터베이스의 user 테이블 구성 확인
```sql
DESC user;
```

## mysql 데이터베이스의 호스트, 사용자, 프러그인, 권한정보를 확인

```sql
SELECT Host, User, plugin, authentication_string FROM user;
```
=> user가 root이고 Host가 localhost<br>
=> user가 root이고 Host가 127.0.0.1<br>

<img src="https://github.com/yuwltn/yuwltn/blob/main/photo/Selecthost.PNG" width="600" height="200" >

## root 사용자의 비밀번호 설정
(=>이전 mysql 버전에서는 update문으로 비밀번호를 설정 BUT GRANT를 이용해서 설정)
```sql
GRANT ALL PRIVILEGES ON *.* TO root@'localhost' IDENTIFIED BY 'iot1234';
GRANT ALL PRIVILEGES ON *.* TO root@'127.0.0.1' IDENTIFIED BY 'iot1234';
```
GRANT는 사용자에게 데이터베이스의 사용 권한을 적용한다.<br>
ALL PRIVIEGES는 데이터베이스에 대한 모든 권한이다.(DB 삭제도 가능)<br>

* 전체 DB에 대해 권한을 가진 사용자를 추가하려면 <br>
  : GRANT ALL PRIVIEGES ON *.* To root@localhost IDENTIFIED BY '패스워드'; <br>
* 일반 사용자 추가 <br>
  : GRANT ALL PRIVIEGES ON DB이름.* TO MySQL아이디@접속서버 IDENTIFIED BY '패스워드' with grant option;<br>
* 특정 이름의 데이터베이스에 대한 사용자 추가 <br>
  : GRANTON ALL PRIVIEGES ON 'DB이름_%'.* TO MySQL아이디@접속서버 IDENTIFIED BY '패스워드' with grant option;<br>
* 특정 사용자에게 DB 권한 주기<br>
  : GRANT ALL ON DB이름.* TO MySQL아이디@'localhost';<br> 

## 현재까지 설정된 내용을 적용
```sql
flush privileges;
```

## 데이터베이스 목록 보기

```sql
SHOW databases;
```

## DB 생성

```sql
CREATE DATABASE dbname;
```

데이터 베이스의 모든 이름(데이터 베이스, 테이블), 칼럼에는 **소문자**를 사용하는게 좋으며 공백 대신 `_` 를 사용한다.

데이터 베이스 **조작어**(CREATE, SELECT 등)는 **대문자**를 사용하는 것이 좋다.

## 사용할 DB 선택

```sql
USE dbname;
```

## 테이블 생성(CREATE)

```sql
CREATE TABLE dbtable
(
num int(11) NOT NULL AUTO_INCREMENT,
name varchar(20) DEFAULT 'X',
age int(11),
PRIMARY KEY(num)
) ENGINE = INNOB;
```

마지막에 `;`을 붙인다.

칼럼 사이 쉼표 `,` 가 있으며 끝에만 없다.

(

칼럼 이름 /  데이터 타입  /  제약 조건,

PRIMARY KEY(칼럼),

FOREIGN KEY(칼럼) REFERENCES 다른 테이블(칼럼)

) ENGINE = INNOB;

- 데이터 타입
- 제약 조건
    - NOT NULL : 항상 값을 넣어주어야 함
    - UNIQUE : 해당 필드는 서로 다른 값을 가져야 함
    - PRIMARY KEY : 해당 필드가  NOT NULL과 UNIQUE 제약 조건 특징을 모두 가지게 됨
    - FOREIGN KEY : 하나의 테이블을 다른 테이블에 의존하게 만듦
    - DEFAULT : 해당 필드의 기본값 설정, default 값을 따로 설정하지 않으면 모두 NULL
    - AUTO_INCREMENT : 해당 필드의 값을 1부터 시작하여 새로운 레코드가 추가될 때마다 값을 저장

## 생성된 테이블 목록 보기

```sql
SHOW TABLES;
```

## 테이블 구조 확인

```sql
DESCRIBE dbtable;
DESC dbtable;
```

## 데이터 삽입(INSERT)

```sql
INSERT INTO dbtable(num, name, age)
VALUES
(`김경은`, `20`)
(`박정근`, `21`)
(`이수연`, `22`);
```

## 입력한 레코드 확인
```sql
SELECT * FROM dbtable;
```

결과를 확인하면 한글이 입력된 부분이 '??' 또는 이상한 문자로 표현된다.<br>
한글이 입력될 수 있도록 데이터베이스의 관리시스템 설정을 변경한다.<br>

## 문자셋 utf8로 설정하기
### 데이터베이스 관리시스템 설정 확인
```sql
show variables;
show variables like 'c%';
```

문자셋에 대한 설정은 모두 character**** 로 시작하는데 확인해보면 character_set_database가 라틴어로 되어있다.<br>
이 문자셋에 대해 utf8로 변경해야 한다.

<img src="https://github.com/yuwltn/yuwltn/blob/main/photo/utf8.PNG" width =" 500" height="250" > 

### mysql 설정 파일에 연결
```sql
exit;

vi /etc/mysql/my.cnf
```
### my.cnf 파일 수정
1. 키보드에서 i를 입력(끼워 넣기)

2. [client] 부분밑에 추가<br>
default-character-set = utf8<br>

3. [mysqld] 부분밑에 추가<br>
init_connect= SET collation_connection= utf8_general_ci<br>
init_connect= SET NAMES utf8<br>
character-set-server = utf8<br>
collation-server = utf8_general_ci<br>

4. [mysqldump]부분밑에 추가<br>
default-character-set = utf8<br>

5. [mysql]부분밑에 추가<br>
default-character-set = utf8<br>

### vi 편집기 빠져나오기
1. 키보드의 좌측 최상단 Esc 키 누르기
2. :wq 입력    =>Write Quit
3. 키보드의 enter 키 입력

### 변경된 mymysql 설정을 적용하기 위해서 mysql DBMS를 재시작
```sql
service mysql restart
mysql -u root -p
password: iot1234
showvariablse like 'c%';
```
데이터베이스 접속해서 데이터베이스 문자열 속성이 utf8로 잘 바뀌었나 확인

<img src="https://github.com/yuwltn/yuwltn/blob/main/photo/utf8_2.PNG" width="500" height="250" >

다시 접속했는데 안바뀌면
```sql
update coffeetbl set 칼럼이름='tablename' WHERE CCODE=1;
```
