## MySQL 접속

```sql
service mysql start
mysql -u root -p
```

## MySQL 닫기

```sql
EXIT;
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
```

## 데이터 삽입(INSERT)

```sql
INSERT INTO dbtable(num, name, age)
VALUES
(`김경은`, `20`)
(`박정근`, `21`)
(`이수연`, `22`);
```
