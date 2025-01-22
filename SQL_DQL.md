- [X] 홍쌤의 데이터랩 SQLD_NEW 강의를 참고한 정리

## DQL
```SQL
SELECT [ALL/DISTINCT] 컬럼명
FROM 테이블명
WHERE 검색조건
GROUP BY 컬럼명
HAVING 검색조건
ORDER BY 컬럼명 [ASC/DESC]
```
- [x] ALL : 중복 허용 / DISTINCT : 중복 허용 X
- [x] ASC : 오름차순(기본) / DESC : 내림차순
- [x] WHERE : 그룹화 전 조건 / HAVING : 그룹화 후 조건


### SELECT/FROM

```SQL
# 여러 컬럼 조회
 
SELECT 컬럼1, 컬럼2
FROM 테이블;
```

```SQL
# 모든 컬럼 조회
 
SELECT *
FROM 테이블;
```

```SQL
# 중복 없이 컬럼 조회
 
SELECT DISTINCT 컬럼
FROM 테이블;
```

```SQL
# 프로그래머스 <잡은 물고기의 평균 길이 구하기>
 
SELECT ROUND(AVG(IFNULL(LENGTH, 10)),2) AS AVERAGE_LENGTH
FROM FISH_INFO;
```


### WHERE
> 조건식 종류

구분|종류
:---:|:---:
비교 | =, <>(!=), <, <=, >, >=
범위 | BETWEEN a AND b
집합 | IN, NOT IN
패턴 | LIKE
NULL | IS NULL, IS NOT NULL
복합 | AND, OR, NOT


- AND, OR로 여려 조건 연결 가능
- 조건 비교 대상의 데이터 타입 일치하는 것이 좋음
- 문자나 날짜 표현 시 반드시 홑따옴표 사용
- ORACLE은 문자 상수 대소문자 구분 (MSSQL은 대소문자 구분 X)


> **IN** <br/>
> 여러 상수와 일치하는 조건 전달 시 사용 <br/>
> 상수를 괄호로 묶어서 동시에 전달 <br/>

```SQL
SELECT SALE,
FROM TRANSACTION
WHERE ID = '001'
OR ID = '002';
```
```SQL
SELECT SALE,
FROM TRANSACTION
WHERE ID IN ('001', '002');
```

> **BETWEEN a AND b** <br/>
> 범위로 묶을 수 있는 문자, 숫자, 날짜 모두 가능 <br/>
> 반드시 A가 B보다 작아야 함. 반대로 작성 시 아무것도 출력 X <br/>


> **LIKE** <br/>
> % : 글자 수 제한 없는 모든 값 <br/>
> _ : _ 하나당 한 자릿수의 모든 값 <br/>


> **NOT** <br/>
> 조건 결과의 여집합 <br/>
> NOT IN, NOT BETWEEN a AND b, NOT LIKE, NOT NULL로 주로 사용 <br/>


```SQL
# 프로그래머스 <이름이 있는 동물의 아이디>
 
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NAME IS NOT NULL;
```


### ORDER BY

- ORDER BY 뒤에 명시된 순서대로 정렬
- SELECT에서 정의한 컬럼 별칭 사용 가능 (유일)
- SELECT에 선언된 순서대로 숫자 전달 가능 (컬럼명과 숫자 혼합 사용 가능)


>**NULL 정렬** <br/>
> 기본적으로 ORACLE은 NULL 마지막 배치, SQL Server는 처음 배치 <br/>
> ORACLE은 NULLS LAST, LULLS FIRST로 NULL 정렬 순서 변경 가능


```SQL
# 고객 ID를 기준으로 내림차순 검색
 
SELECT *
FROM CUSTOMER
ORDER BY ID DESC;
```


### GROUP BY/HAVING

- 그룹 연산에서 제외할 대상이 있다면 WHERE에서 미리 제외
- 그룹에 대한 조건은 WHERE에서 사용 X
- GROUP BY를 사용하면 데이터가 요약되므로, 요약 전 데이터와 함께 출력 X


```SQL
#  프로그래머스 <노선별 평균 역 사이 거리 조회하기>
 
SELECT ROUTE,  
CONCAT(ROUND(SUM(D_BETWEEN_DIST), 1),'km') AS TOTAL_DISTANCE, 
CONCAT(ROUND(AVG(D_BETWEEN_DIST), 2), 'km') AS AVERAGE_DISTANCE
FROM SUBWAY_DISTANCE
GROUP BY ROUTE
ORDER BY ROUND(SUM(D_BETWEEN_DIST), 1) DESC;
```



### 함수
> 문자형 함수

함수명|함수기능|기타
:---|:---|:---
LOWER(대상) | 문자열을 소문자로 | 
UPPER(대상) | 문자열을 대문자로 | 
SUBSTR(대상, m, n) | 문자열 m위치에서 n개의 문자열 추출 | n 생략 : 끝까지 추출<br/> -m : 뒤에서부터 m위치에서 오른쪽으로
INSTR(대상, 찾을 문자열, m, n) | 대상에서 찾을 문자열 위치 반환 <br/> m위치에서 시작, n번째 발견된 문자열 위치 | m, n 생략 : 1로 해석 <br/> -m : 뒤에서부터 m위치에서 왼쪽으로
LTRIM(대상, 삭제 문자열) | 문자열 중 특정 문자열을 왼쪽에서 삭제 | 삭제 문자열 생략 : 공백 삭제
RTRIM(대상, 삭제 문자열) | 문자열 중 특정 문자열을 오른쪽에서 삭제 | 삭제 문자열 생략 : 공백 삭제
TRIM(대상) | 문자열 중 특정 문자열을 쪽에서 삭제 | ORACLE : 공백만 삭제 가능
LPAD(대상, n, 문자열) | 대상 왼쪽에 문자열을 추가하여 총 n의 길이로 | 
RPAD(대상, n, 문자열) | 대상 오른쪽에 문자열을 추가하여 총 n의 길이로 |
CONCAT(대상1, 대상2) | 대상 문자열 결합 | 두 개의 인수만 전달 가능
LENGTH(대상) | 대상 문자열 길이 | 
REPLACE(대상, 찾을 문자열, 바꿀 문자열) | 일치하는 문자열 치환 및 삭제 | 바꿀 문자열 생략, 빈 문자열 전달 : 찾을 문자열 제거
TRANSLATE(대상, 찾을 문자열, 바꿀 문자열) | 문자열 1:1 치환 | 바꿀 문자열 생략 불가 <br/> 빈 문자열 전달 : 전체 NULL

- SQL Server <br/>
SUBSTR > SUBSTRING <br/>
LENGTH > LEN <br/>
INSTR > CHARINDEX <br/>


> 숫자형 함수

함수명|함수기능|기타
:---|:---|:---
ABS(숫자) | 절대값 | 
ROUND(숫자, 자리수) | 반올림 | 자리수로 반올림 <br/> 자리수 -2 : 정수 십의 자리에서 반올림
TRUNC(숫자, 자리수) | 버림 | 자리수로 버림
SIGN(숫자) | 양수면1, 음수면-1, 0이면0 | 
FLOOR(숫자) | 작거나 같은 최대 정수(버림) | 
CEIL(숫자) | 크거나 같은 최소 정수(올림) | 
MOD(숫자1, 숫자2) | 숫자1을 숫자2로 나눈 나머지 | 
POWER(m, n) | m의 n 거듭제곱 | 
SQRT(숫자) | 루트값 | 


> 날짜형 함수

함수명|함수기능|기타
:---|:---|:---
SYSDATE | 현재 날짜와 시간 | 
CURRENT_DATE | 현재 날짜 | 
CURRENT_TIMESTAMP | 현재 타임스탬프 | 
ADD_MONTHS(날짜, n) | 날짜에서 n개월 후 날짜 | -n : n개월 이전 날짜
MONTHS_BETWEEN(날짜1, 날짜2) | 날짜1과 날짜2의 개월 수 차이 | 날짜1<날짜2 : 음수
LAST_DAY(날짜) | 주어진 월의 마지막 날짜 | 
NEXT_DAY(날짜, n) | 주어진 날짜 이후 지정된 요일의 첫 날짜 | 1: 일요일 ~ 7: 토요일
ROUND(날짜, 자리수) | 날짜 반올림 | 자리수 'MONTH' 입력: 월 이전자리에서 반올림 (16일부터 올림) 
TRUNC(날짜, 자리수) | 날짜 버림 | 자리수 'MONTH' 입력: 월 이전자리에서 버림

- SQL Server <br/>
SYSDATE > GETDATE <br/>
ADD_MONTHS > DATEADD(모든 단위 날짜 연산 가능) <br/>
MONTHS_BETWEEN > DATEDIFF(두 날짜 사이의 년, 월, 일) <br/>


> 변환함수

함수명|함수기능|기타
:---|:---|:---
TO_NUMBER(문자) | 숫자 타입으로 변경 | 
TO_CHAR(대상, 포맷) | 대상을 포맷 형식으로 변경 | 포맷 'MM/DD-YYYY' 입력 : 날짜 형식, 문자 타입 <br/> 포맷 '9,999' 입력 : 천단위 구분기호, 문자 타입 <br/> 포맷 '09999' 입력 : 총 5자리로, 앞 자리수 0
TO_DATE(문자, 포맷) | 문자를 포맷 형식에 맞게 읽어 날짜 데이터로 변경 | 
FORMAT(날짜, 포맷) | 날짜의 포맷 형식 변경 | 포맷 'YYYY' 입력 : 문자 타입 '2024' 반환
CAST(대상 AS 데이터 타입) | 대상을 주어진 데이터 타입으로 변환 | 

- SQL Server <br/>
TO_NUMBER, TO_DATE, TO_CHAR > CONVERT(포맷 전달) <br/>
단순 변환은 주로 CAST 사용 <br/>


> 그룹함수

함수명|함수기능|기타
:---|:---|:---
COUNT(대상) | 행의 수 | NULL 무시 연산 <br/> 전체 NULL : 0 반환
SUM(대상) | 총 합 | NULL 무시 연산 <br/> 전체 NULL : 공집합 반환
AVG(대상) | 평균 | NULL 무시 연산 <br/> 전체 NULL : 공집합 반환
MIN(대상) | 최솟값 | NULL 무시 연산 <br/> 전체 NULL : 공집합 반환
MAX(대상) | 최댓값 | NULL 무시 연산 <br/> 전체 NULL : 공집합 반환
VARIANCE(대상) | 분산 | NULL 무시 연산 <br/> 전체 NULL : 공집합 반환
STDDEV(대상) | 표준편차 | NULL 무시 연산 <br/> 전체 NULL : 공집합 반환

- SQL Server <br/>
VARIANCE > VAR <br/>
STDDEV > STDEV <br/>


> 일반함수

함수명|함수기능|기타
:---|:---|:---
DECODE(대상, 값1, 리턴1, 값2, 리턴2, ~, 그외리턴) | 대상이 값1이면 리턴1, 값2이면 리턴2, ~, 그외에는 그외리턴값 | 그외리턴값 생략 : NULL 반환 <br/> 대소비교 불가
NVL(대상, 치환값) | 대상이 NULL이면 치환값 반환 | 
NVL2(대상, 치환값1, 치환값2) | 대상이 NULL이면 치환값2 반환, NULL아니면 치환값1 반환 | 
COALESCE(대상1, 대상2, ~, 그외리턴) | 대상 중 첫번째부터 NULL이 아닌값 반환, 모두가 NULL이면 그외리턴값 반환 | 그외리턴값 생략 : NULL 반환 
ISNULL(대상, 치환값) | 대상이 NULL이면 치환값 반환 | SQL SERVER 함수 
NULLIF(대상1, 대상2) | 대상이 같으면 NULL 반환, 다르면 대상1 반환 | 
CASE문 | 조건별 치환 및 연산 수행 | 


- CASE문 예제
```SQL
SELECT SALE,
       CASE WHEN SALE = 200 THEN 'A'
            WHEN SALE = 300 THEN 'B'
                            ELSE 'C'
       END AS GRADE
FROM TRANSACTION;
```

*같은 대상에 대해 일치하는 조건일 때, 아래 코드도 동일한 결과 반환
```SQL
SELECT SALE,
       CASE SALE WHEN 200 THEN 'A'
                 WHEN 300 THEN 'B'
                          ELSE 'C'
       END AS GRADE
FROM TRANSACTION;
```


### JOIN

- 여러 테이블의 데이터를 사용하여 동시 출력 및 참조할 때 사용
- FROM에 테이블명 나열 (ORACLE 표준은 테이블 나열 순서 중요 X, ANSI 표준은 OUTER JOIN시 순서 중요)
- WHERE에 JOIN 조건 작성
- 동일한 컬럼명이 여러 테이블에 존재할 경우 컬럼명 앞에 테이블 이름 명시
- N개의 테이블을 JOIN하려면 최소 N-1개 조건 필요

1. 조건 형태
   1. EQUI JOIN : JOIN 조건이 동등 조건인 경우
   2. NON EQUI JOIN : JOIN 조건이 동등 조건이 아닌 경우
 
2. JOIN 결과
   1. INNER JOIN : JOIN 조건이 성립하는 데이터만 출력
   2. OUTER JOIN : JOIN 조건이 성립하지 않는 데이터도 출력 (LEFT/RIGHT/FULL)

3. NATURAL JOIN : JOIN 조건 생략 시 두 테이블의 같은 컬럼명으로 자연 연결
4. CROSS JOIN : JOIN 조건 생략 시 두 테이블의 발생 가능한 모든 행을 출력, Cartesian product(카타시안곱)
5. SELF JOIN : 하나의 테이블을 두 번 이상 참조하여 연결


> **ANSI 표준** <br/>
> INNER JOIN의 경우 FROM에 INNER JOIN, JOIN을 명시 <br/>
> USING, ON 조건 필수 사용 <br/>

```SQL
# ON, JOIN 할 컬럼명이 달라도 사용 가능 (컬럼명이 같은 경우 테이블 출처 명확히)
 
SELECT ENP.ENAME, DEPT.DNAME
FROM EMP JOIN DEPT
ON ENP.DEPTNO = DEPT.DEPTNO;
```

```SQL
# USING, JOIN 할 컬럼명이 같을 때 사용 가능 (괄호 필수)
 
SELECT ENP.ENAME, DEPT.DNAME
FROM EMP JOIN DEPT
USING (DEPTNO);
```

```SQL
# NATURAL JOIN, 컬럼명이 같은 모든 컬럼을 JOIN에 사용하므로 주의
 
SELECT ENP.ENAME, DEPT.DNAME
FROM EMP NATURAL JOIN DEPT;
```

```SQL
# LEFT OUTER JOIN
 
SELECT S.STUDNO, S.NAME AS 학생명, S.GRADE, S.PROFNO, P.PROFNO, P.NAME AS 교수명
FROM STUDENT S LEFT OUTER JOIN PROFESSOR P
ON S.PROFNO = P.PROFNO
WHERE S.GRADE IN (1, 4);
```


> **ORACLE 표준** <br/>
> INNER JOIN이 기본 ( ,로 나열) <br/>
> FULL OUTER JOIN 없음 (LEFT/RIGHT OUTER JOIN 결과의 UNION 연산과 동일)


```SQL
# LEFT OUTER JOIN
 
SELECT S.STUDNO, S.NAME AS 학생명, S.GRADE, S.PROFNO, P.PROFNO, P.NAME AS 교수명
FROM STUDENT S, PROFESSOR P
WHERE S.PROFNO = P.PROFNO(+)
AND S.GRADE IN (1, 4);
```



### 정규표현식

> 정규표현식 종류

표현식|의미
:---|:---
\d | Digit, 0, 1, 2, ..., 9
\D | 숫자가 아닌 것
\s | 공백
\S | 공백이 아닌 것
\w | 단어
\W | 단어가 아닌 것 ( _제외 특수문자)
\t | Tab
\n | New line
^ | 시작 글자 (^a)
$ | 마지막 글자 (a$)
\ | Escape character, 뒤에 기호 의미 제거 
\| | 또는
. | 엔터를 제외한 모든 한 글자
[ab] | a 또는 b의 한 글자
[^ab] | a와 b 제외한 모든 문자
[0-9] | 숫자
[A-Z] | 영어 대문자
[a-z] | 영어 소문자
[A-z] | 모든 영문자
i+ | i가 1회 이상 반복
i* | i가 0회 이상 반복
i? | i가 0회~1회 반복
i{n} | i가 n회 반복
i{n1, n2} | i가 n1~n2회 반복
i{n, } | i가 n회 이상 반복
( ) | 그룹 지정
\n | 그룹 번호 (n은 숫자)
[:alnum:] | 문자와 숫자
[:alpha:] | 문자
[:blank:] | 공백
[:cntrl:] | 제어문자
[:digit:] | Digits
[:graph:] | Graphical characters <br/>
[:lower:] | 소문자
[:upper:] | 대문자
[:print:] | 숫자, 문자, 특수문자, 공백 모두
[:punct:] | 특수문자
[:space:] | 공백문자
[:xdigit:] | 16진수















