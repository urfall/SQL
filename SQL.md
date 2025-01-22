- [X] 홍쌤의 데이터랩 SQLD_NEW 강의를 참고한 정리

## SQL 기본문법

구분|종류
:---:|:---:
**DDL** | CREATE, ALTER, DROP, TRUNCATE
**DML** | INSERT, DELETE, UPDATE, MERGE
**DCL** | GRANT, REVOKE
**TCL** | COMMIT, ROLLBACK
**DQL** | SELECT



## DML

- [x] 종류 : INSERT, UPDATE, DELETE, MERGE
- [x] 저장(COMMIT) 혹은 취소(ROLLBACK) 반드시 필요


### INSERT

- 한 번에 한 행만 입력 가능(SQL Server는 여러 행 동시 입력 가능)
- 하나의 컬럼에는 한 값만 입력 가능
- 컬럼별 데이터 타입과 사이즈에 맞게 입력
- 일부 컬럼만 입력 가능, 작성하지 않은 컬럼은 NULL이 입력(NOT NULL 컬럼의 경우 오류 발생)


```SQL
# 문법

INSERT INTO 테이블명 VALUES(값1, 값2, ...);
INSERT INTO 테이블명(컬럼1, 컬럼2) VALUES(값1, 값2);
```


### UPDATE

- 컬럼 단위, 다중 컬럼 수정 가능


```SQL
# 단일컬럼 수정

UPDATE 테이블명
SET 수정할컬럼명 = 수정값
WHERE 조건;
```

```SQL
# 다중컬럼 수정

UPDATE 테이블명
SET 수정할컬럼명1 = 수정값1, 수정할컬럼명2 = 수정값2, ...
WHERE 조건;
```
```SQL
# 다중컬럼 수정(서브쿼리 사용)

UPDATE 테이블명
SET (수정할컬럼명1, 수정할컬럼명2) = (SELECT 수정값1, 수정값2)
WHERE 조건;
```


### DELETE

```SQL
# 문법

DELETE[FROM] 테이블명
[WHERE 조건];
```


### MERGE

- 참조 테이블과 동일하게 맞추기(참조 테이블의 값으로 수정 등)
- INSERT, UPDATE, DELETE 동시 수행


```SQL
# 문법

MERGE INTO 테이블명
USING 참조테이블명
ON (연결조건) #괄호필수
WHEN MATCHED THEN
     UPDATE
     SET 수정내용
     DELETE (조건)
WHEN NOT MATCHED THEN
     INSERT VALUES(값1, 값2, ...);
```



## TCL

- [x] DML에 의해 조작된 결과를 제어하는 명령어, DML 수행 후 트랜잭션을 정상 종료하지 않는 경우 LOCK 발생
- [x] 분할 할 수 없는 최소 단위 (ALL OR NOTHING)
- [x]  **특징** <br/>
  원자성(atomicity): 트랜잭션 정의된 연산 모두 성공적으로 실행 or 전혀 실행되지 않은 상태로 있어야 함 <br/>
  일관성(consistency): 트랜잭션 실행 전 데이터베이스 내용이 잘못되어 있지 않다면, 실행 후에도 잘못이 있으면 안 됨 <br/>
  고립성(isolation): 트랜잭션 실행 중 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들어서는 안 됨 <br/>
  지속성(durability): 트랜잭션이 성공적으로 수행되면, 수정된 데이터베이스 내용 영구 저장 <br/>



### COMMIT

- 데이터 저장, COMMIT을 수행하면 COMMIT 이전 수행된 DML은 모두 저장되며 되돌릴 수 없음


### ROLLBACK

- 데이터 변경 취소, 최종 COMMIT 시점 이전까지 원복 가능
- SAVEPOINT를 설정하여 최종 COMMIT 이후 원하는 시점으로의 원복 가능
- SAVEPOINT 이전 수행한 UPDATE는 취소되지 않음

```SQL
# SAVEPOINT 설정

SAVEPOINT savepoint_name;
```



## DDL

- [x] 종류 : CREATE(객체 생성), ALTER(객체 변경), DROP(객체 삭제), TRUNCATE(데이터 전부 삭제)
- [x] AUTO COMMIT, 명령어 수행 시 즉시 저장(원복 불가)


### CREATE

- 테이블이나 인덱스와 같은 객체를 생성하는 명령어
- 테이블 생성 시 테이블명, 컬럼명, 컬럼순서, 컬럼크기, 컬럼 데이터타입 정의 필수
- 테이블 생성 시 소유자 명시 가능(생략 시 명령어 수행 계정 소유)

```SQL
# 테이블 생성 문법

CREATE TABLE [소유자.]테이블명(
컬럼1 테이터타입 [DEFAULT 기본값] [제약조건],
컬럼2 테이터타입 [DEFAULT 기본값] [제약조건]
);
```

```SQL
# 테이블 복제 문법

CREATE TABLE 테이블명
AS
SELECT * FROM 복제테이블명;
```

```SQL
# 테이블 구조만 복제, 항상 거짓인 조건을 전달하면 테이블 구조만 복제 가능

CREATE TABLE 테이블명
AS
SELECT *
FROM 복제테이블명
WHERE 1=2;
```

```SQL
# 테이블 복제 시 컬럼명 변경

CREATE TABLE 테이블명(새컬럼명1, 새컬럼명2) 
AS
SELECT 컬럼명1, 컬럼명2
FROM 복제테이블명;
```


> 데이터 타입

데이터 타입|설명
:--|:--
CHAR(n) | 사이즈 전달 필수, 고정형 문자 타입(빈자리는 공백을 채워 사이즈만큼 데이터 입력)
VARCHAR2(n) | 사이즈 전달 필수, 가변형 문자 타입(사이즈 보다 작은 문자값 그대로 입력)
NUMBER(p, s) | 사이즈 생략 가능, 숫자형으로 p는 총 자릿수/ s는 소수점 자릿수
DATE | 사이즈 전달 불가, 날짜형

- SQL Server <br/>
  VARCHAR2 > VARCHAR <br/>
  NUMBER > NUMERIC <br/>
  문자타입 사이즈 생략 가능 (생략 시 1) <br/>



### ALTER

- 테이블 구조 변경(컬럼명, 컬럼 데이터타입, 컬럼 사이즈, 컬럼 삭제, 컬럼 추가, 제약조건 등)
- 컬럼순서 변경 불가, 재생성으로 해결


1. 컬럼 추가
   - 새로 추가된 컬럼 위치는 맨 마지막(중간 위치에 추가 불가)
   - 여러 컬럼 동시 추가 가능, 반드시 괄호 사용
   - 데이터가 있는 테이블에 컬럼 추가 시 NOT NULL 속성 전달 불가, 데이터가 없는 테이블은 가능
   - DEFAULT를 선언하면 NOT NULL 속성 컬럼 추가 가능

```SQL
# 문법

ALTER TABLE 테이블명 ADD 컬럼명 데이터타입 [DEFAULT] [제약조건];
ALTER TABLE 테이블명 ADD (컬럼명1 데이터타입, 컬럼명2 데이터타입);
```

2. 컬럼 변경
   - 사이즈 변경: 증가는 항상 가능, 축소는 기존 데이터의 최대 사이즈 만큼 가능
   - 동시 변경 가능, 반드시 괄호 사용

```SQL
# 문법

ALTER TABLE 테이블명 MODIFY (컬럼명1 데이터타입(사이즈), 컬럼명2 데이터타입(사이즈));
ALTER TABLE 테이블명 MODIFY (컬럼명 DEFAULT 기본값);
```

3. 컬럼 이름 변경
   - 항상 가능
   - 동시 이름 변경 불가

```SQL
# 문법

ALTER TABLE 테이블명 RENAME COLUMN 기존컬럼명 TO 새컬럼명;
```


3. 컬럼 삭제
   - 항상 가능, 데이터 있어도 삭제 가능(복구 불가)
   - 동시 삭제 불가

```SQL
# 문법

ALTER TABLE 테이블명 DROP COLUMN 컬럼명;
```


### DROP

- 객체(테이블, 인덱스 등) 삭제
- DROP 후 조회 불가

```SQL
# 문법, PURGE로 삭제 시 RECYCLEBIN에서 조회 불가

DROP TABLE 테이블명 [PURGE];
```


### TRUNCATE

- 테이블 구조 남기고 데이터는 즉시 삭제, 즉시 반영(AUTO COMMIT)
- RECYCLEBIN에 남지 않음
- TRUNCATE 후 조회 가능, 데이터는 없음

```SQL
# 문법

TRUNCATE TABLE 테이블명;
```


- [x] **`DEFAULT`**
- 값이 생략될 때 자동으로 부여되는 값
- INSERT에서 NULL 직접 입력 시, DEFAULT값 대신 NULL 입력
- 이미 데이터가 존재하는 테이블에 DEFAULT값 선언 시, 기존 데이터는 수정 X
- DEFAULT값 해제 시, DEFAULT값을 NULL로 선언


- [x] **`제약조건`**
- 데이터 무결성을 위해 컬럼에 생성하는 데이터의 제약 장치
- 테이블 생성 시 정의 가능, 컬럼 추가 시 정의 가능, 이미 생성된 컬럼에 제약조건만 추가 가능

1. PRIMARY KEY(기본키)
   - 유일한 식별자, 테이블당 1개
   - UNIQUE + NOT NULL (중복, NULL 허용 X)
   - 여러 컬럼을 결합하여 기본키 생성 가능
   - PRIMARY KEY 생성 시 NOT NULL 속성 자동 부여 But, CTAS로 테이블 복사 시 속성 복사 X (NOT NULL 속성 직접 정의해야 복사 O)
   - PRIMARY KEY 생성 시 UNIQUE INDEX 자동 생성

```SQL
# 테이블 생성 시 제약조건 생성(이름 없이)

CREATE TABLE TEST(NO NUMBER(10) PRIMARY KEY,
                  NAME VARCHAR2(20));
```

```SQL
# 테이블 생성 시 제약조건 생성(이름 전달)

CREATE TABLE TEST(
NO1 NUMBER,
NO2 NUMBER,
NAME VARCHAR2(20),
CONSTRAINT TEST_PK PRIMARY KEY(NO1, NO2)
);
```

```SQL
# 컬럼 추가 시 제약조건 생성

CREATE TABLE TEST (SUBJECT VARCHAR2(20));
ALTER TABLE TEST ADD NO NUMBER(10) PRIMARY KEY;
```

```SQL
# 기존 컬럼에 제약조건 생성

CREATE TABLE TEST(NO NUMBER(10),
                  NAME VARCHAR2(20));
ALTER TABLE TEST ADD CONSTRAINT TEST_PK PRIMARY KEY(NO);
```


2. UNIQUE
   - 중복 허용 X But, NULL 허용
   - UNIQUE INDEX 자동 생성

```SQL
# UNIQUE KEY 생성

CREATE TABLE TEST(
NO NUMBER,
SUBJECT VARCHAR2(20) UNIQUE);
```

```SQL
# UNIQUE KEY 컬럼에 값 입력

INSERT INTO TEST VALUES(1, 'A');
INSERT INTO TEST VALUES(2, 'A'); #에러
INSERT INTO TEST VALUES(2, NULL);
INSERT INTO TEST VALUES(3, NULL);
```


3. NOT NULL
   - 다른 제약조건과 달리 CTAS로 복제 시 속성 복사 O
   - 컬럼 생성 시 NOT NULL을 선언하지 않으면 Nullable 컬럼으로 생성
   - 이미 만들어진 컬럼에 NOT NULL 선언 시 컬럼 수정(MODIFY)으로 해결

```SQL
# NOT NULL 선언

ALTER TABLE TEST ADD NOT NULL(COL12); #불가
ALTER TABLE TEST MODIFY COL12 NOT NULL;
ALTER TABLE TEST MODIFY COL12
                 CONSTRAINT TEST_COL12_NN NOT NULL;
```


4. FOREIGN KEY
   - 참조테이블의 참조컬럼에 있는 데이터를 확인하면서 본테이블의 데이터를 관리할 목적
   - 반드시 참조(부모)테이블의 참조컬럼(REFERENCE KEY)이 사전에 PK 혹은 UNIQUE KEY를 가져야 함!
   - 옵션: ON DELETE CASCADE / ON DELETE SET NULL

```SQL
# 문법

CREATE TABLE 테이블명(
컬럼1 데이터타입 [DEFAULT 기본값] REFERENCES 참조테이블(참조키),
...
);
```

```SQL
# 부모테이블(DEPT), 자식테이블(EMP)

ALTER TABLE DEPT ADD CONSTRAINT DEPT_PK PRIMARY KEY(DEPTNO);
ALTER TABLE EMP ADD CONSTRAINT EMP_FK FOREIGN KEY(DEPTNO) REFERENCES DEPT(DEPTNO);

DELETE EMP WHERE DEPTNO = 10; #자식테이블 삭제 가능
UPDATE EMP SET DEPTNO = 50 WHERE DEPTNO = 20; #에러, 부모테이블에 정의되지 않은 값으로 수정 불가
INSERT INTO EMP(EMPNO, ENAME, DEPTNO) VALUES(1111, 'A', 50); #에러, 부모테이블에 정의되지 않은 값으로 입력 불가

DELETE DEPT WHERE DEPTNO = 20; #에러, 자식테이블에 존재하는 정보 삭제 불가
UPDATE DEPT SET DEPTNO = 60 WHERE DEPTNO = 20; #에러, 자식테이블에 존재하는 정보 변경 불가
```

```SQL
# ON DELETE CASCADE

ALTER TABLE EMP DROP CONSTRAINT EMP_FK;
ALTER TABLE EMP ADD CONSTRAINT EMP_FK FOREIGN KEY(DEPTNO) REFERENCES DEPT(DEPTNO) ON DELETE CASCADE;

DELETE DEPT WHERE DEPTNO = 10; #부모데이터 삭제 시, 자식데이터 함께 삭제
```

```SQL
# ON DELETE SET NULL

ALTER TABLE EMP DROP CONSTRAINT EMP_FK;
ALTER TABLE EMP ADD CONSTRAINT EMP_FK FOREIGN KEY(DEPTNO) REFERENCES DEPT(DEPTNO) ON DELETE SET NULL;

DELETE DEPT WHERE DEPTNO = 10; #부모데이터 삭제 시, 자식데이터 NULL로 수정
```


5. CHECK
   - 직접 데이터의 값 제한

```SQL
# CHECK 제약조건 추가 (EMP의 SAL 값은 0 이상)

ALTER TABLE EMP ADD CONSTRAINT EMP_SAL_CK CHECK (SAL > 0);
```


> **VIEW**, 테이블처럼 조회 가능(물리적 저장 X)
```SQL
# VIEW 생성 및 조회

CREATE VIEW VIEW_EMP_DEPT
AS
SELECT E.EMPNO, E.ENAME, E.SAL, E.DEPTNO, D.DNAME
FROM EMP E, DEPT D
WHERE E.DEPTNO = D.DEPTNO;

SELECT *
FROM VIEW_EMP_DEPT;


# VIEW 삭제

DROP VIEW VIEW_EMP_DEPT;
```


> **SEQUENCE**, 연속적인 숫자 자동 부여 객체
```SQL
# 문법

CREATE SEQUENCE 시퀀스명
INCREMENT BY #증가값(DEFAULT 1)
START WITH #시작값(DEFAULT 1)
MAXVALUE #마지막값(증가시퀀스), 시작값(감소시퀀스)
MINVALUE #시작값(증가시퀀스), 마지막값(감소시퀀스)
CYCLE | NOCYCLE #시퀀스 번호 재사용(DEFAULT NOCYCLE)
CACHE N #캐시값(DEFAULT 20)
;
```


> **SYNONYM**, 테이블 별칭 생성
```SQL
# 문법

CREATE [OR REPLACE] [PUBLIC] SYNONYM 별칭 FOR 테이블명;
```
- OR REPLACE : 기존의 같은 이름 SYNONYM이 생성되어 있는 경우 대체
- PUBLIC : 생성한 SYNONYM 누구나 사용 가능 (반드시 PUBLIC으로 삭제)




## DCL

- [x] 객체에 대한 권한을 부여(GRANT), 회수(REVOKE)
- [x] 본인 소유가 아닌 테이블은 원칙적으로 조회 불가(권한통제)


1. 오브젝트 권한
   - 테이블에 대한 권한(SELECT, INSERT, UPDATE, DELETE, MERGE 권한)
   - 테이블 소유자는 타계정에 소유 테이블에 대한 조회 및 수정 권한 부여 및 회수 가능

2. 시스템 권한
   - 테이블 생성, 인덱스 삭제 등의 시스템 작업 제
   - 관리자 권한만 부여 및 회수 가능


### GRANT

- 동시에 여러 유저에게 권한 부여 가능
- 동시에 여러 권한 부여 가능
- 동시에 여러 객체 권한 부여 불가

```SQL
# 오브젝트 권한

GRANT 권한 ON 테이블명 TO 유저명;

GRANT SELECT ON EMP TO HR;
GRANT SELECT ON EMP TO HR, BI;
GRANT SELECT, UPDATE, INSERT ON EMP TO HR, BI;
GRANT SELECT ON EMP, DEPT TO HR; #에러
```

```SQL
# 시스템 권한

GRANT CREATE TABLE TO HR;
GRANT CREATE TABLE, DROP ANY TABLE TO HR, BI;
```


### REVOKE

- 동시에 여러 권한 회수 가능
- 동시에 여러 유저 권한 회수 가능

```SQL
# 문법

REVOKE 권한 ON 테이블명 FROM 유저명;

REVOKE SELECT ON EMP FROM HR;
REVOKE SELECT, UPDATE, INSERT ON EMP FROM HR, BI;
```


### ROLE

- 권한의 묶음, 생성 가능 객체
- 재접속해야 권한 부여
- ROLE에서 회수된 권한은 즉시 반영되므로, ROLE 재부여 필요 없음
- ROLE을 통해 부여한 권한은 직접 회수 불가, ROLE을 통한 회수만 가능

```SQL
# 문법

CREATE ROLE 롤명;
```

```SQL
# 생성 및 부여

CREATE ROLE ROLE_SET;

GRANT SELECT ON EMP TO ROLE_SET;
GRANT SELECT ON DEPT TO ROLE_SET;

GRANT ROLE_SET TO HR;
```

```SQL
# ROLE에서 권한 회수

REVOKE SELECT ON EMP FROM ROLE_SET;
```


> 권한부여 옵션 (중간관리자의 권한)
1. WITH GRANT OPTION
   - 오브젝트 권한을 다른 사용자에게 부여 가능
   - 중간관리자만 회수 가능
   - 중간관리자에게 부여된 권한 회수 시 제 3자에게 부여된 권한도 함께 회수
 
2. WITH ADMIN OPTION
   - 시스템/롤 권한을 다른 사용자에게 부여 가능
   - 직접 회수 가능
   - 중간관리자에게 부여된 권한을 회수해도 제 3자에게 부여된 권한 회수 X 










