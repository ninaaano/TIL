4주차 과제를 구현하면서 DB를 다루게되었다.
요구사항 중 "게시글 데이터를 완전히 삭제하는것이 아니라 데이터의 상태를 삭제상태로 변경한다" 라는 구절이 있었다.
나는 SQL에서 삭제라함은 delete 말곤 떠오르지 않았는데
데이터를 update 함으로써 삭제 상태를 가지게 할 수 있다는 것을 알았다
완전 삭제가 아닌 삭제 상태를 가지는 것을 soft delete라고 한다
soft delete? 그게 뭘까?

Hard Delete(물리삭제)
```
DELETE FROM table WHERE id = ? 
```
SQL DELETE 문을 사용해서
row가 실제로 DB에서 삭제되는 방식이다

Soft Delete(논리삭제)
```
UPDATE table SET deleted = 1 WHERE id = ?
```
row가 삭제되지 않고 deleted 라는 flag를 통해 제어한다
삭제 여부를 알 수 있는 컬럼을 사용해서 표현한다
실제 데이터는 DB에 남아있지만 사용자에겐 정보가 삭제된 것 처럼 보여주는 방법이다.
컬럼의 자료형은 boolean 또는 datetime을 사용한다.
datetime은 NULL이 가능하고 INSERT시 기본값 NULL을 사용한다
(예: 삭제되지 않은 데이터는 removed 컬럼값이 NULL, 삭제된 데이터는 removed 값이 삭제된 일시
)
복구하거나 예전 기록을 확인할때 간편하지만 다른 테이블과 JOIN시에 항상 deleted flag를 확인해야해서 속도도 느리고 불편하다는 단점이 있다

나는 미션에서 boolean flag를 사용했다.
```
deleted boolean default false
```
기본값으로 false를 놓고 삭제시 true가 되도록 변경해주었다.