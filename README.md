빅데이터분석기사를 공부하면서 공부 과정을 기록하지 못한 게 아쉬워서 SQLD를 준비하는 겸, SQL에 관한 공부 과정을 기록했습니다 👊👊 <br/><br/>


## SQLD 공부 과정
- 강의 : [홍쌤의 데이터랩](https://www.youtube.com/@hdatalab) <br/><br/>
**`2024.11.06.`** 홍쌤의 데이터랩 [SQLD 1과목 완벽 정리] 수강완. <br/>
**`2024.11.11.`** 홍쌤의 데이터랩 [SQLD 2과목 PART1. SQL 기본] 수강완. <br/>
**`2024.11.13.`** 홍쌤의 데이터랩 [SQLD 2과목 PART2. SQL 활용] 수강완. <br/>
**`2024.11.14.`** 홍쌤의 데이터랩 [SQLD 2과목 PART3. 관리 구문] 수강완. <br/>
**`2024.11.15.`** 홍쌤의 데이터랩 [SQLD 기출문제 풀이 1회차] 수강완. <br/>
**`2024.11.17.`** SQLD 시험. **합격🎉** <br/>

## 🤔
### SQL 명령어 종류
- **DDL** : CREATE, ALTER, DROP, TRUNCATE
- **DML** : INSERT, DELETE, UPDATE, MERGE
- **DCL** : GRANT, REVOKE
- **TCL** : COMMIT, ROLLBACK
- **DQL** : SELECT


### 정규화
- 제 1정규형 : **1NF** (First Normal Form) <br/>
테이블의 컬럼이 원자값(Atomic Value, 하나의 값)을 갖도록 테이블을 분해하는 것 <br/>

- 제 2정규형 : **2NF** (Second Normal Form) <br/>
제1 정규화를 진행한 테이블에 대해 완전 함수 종속을 만족하도록 테이블을 분해하는 것 <br/>
*완전 함수 종속 = 기본 키의 부분집합이 결정자가 되어서는 안 됨 <br/>

- 제 3정규형 : **3NF** (Third Normal Form) <br/>
제2 정규화를 진행한 테이블에 대해 이행적 종속을 없애도록 테이블을 분해하는 것 <br/>
*이행적 종속 = A→B, B→C가 성립할 때, A→C가 성립되는 것

