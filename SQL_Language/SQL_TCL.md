💡 홍쌤의 데이터랩 SQLD_NEW 강의를 참고한 정리

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

