


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










