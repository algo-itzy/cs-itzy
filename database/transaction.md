# 트랜잭션(transaction)

* DB 상태변화를 위해 수행하는 작업의 단위.

  * = SQL문으로 DB를 변화 시키기 위해 기준에 따라 정하는 일련의 작업단위

  * = 여러 쿼리를 논리적으로 하나의 작업으로 묶어주는 것.



> TRANSACTION단위별로 내부의 쿼리들은 하나로 묶이기 때문에 
>
> 안에서 하나의 쿼리만 실행되고 나머지는 실행되지 않는 현상을 막고 하나의 쿼리처럼 동작하게 한다.



## ACID

* 원자성(Atomicity) : 트랜잭션은 DB에 모두 반영되거나, 전혀 반영되지 않아야 한다.(중간에 그치는 일을 없게함)

* 일관성(Consistency): 트랜잭션 작업처리결과는 항상 일관성 있어야 한다. DB는 항상 일관된 상태로 유지되어야한다.(DB조건에 위배되면 트랜잭션은 즉시 종료.)

* 독립성(Isolation): 둘 이상의 트랜잭션이 동시 실행되고 있을 때 어떤 트랜잭션도 다른 트랜잭션 연산에 끼어 들 수 없다.

* 지속성(Durability):트랜잭션이 성공했다면 결과는 영구적으로 반영되어야한다.

> 보장해야하는 성질들이나 성능의 문제가 일어날 수 있으므로 그 수준에 대한 설정이 필요함.



>  **사용자, 시스템간 실수가 있더라도 DB가 data를 안정적으로 보장할 수 있게 한다.**



## 트랜잭션 없는 입출금 매커니즘.

```mysql
UPDATE accounts
SET balance = balance - 10000
WHERE user = '구매자';

UPDATE accounts
SET balance = balance + 10000
WHERE user = '판매자';
```



### 예상되는 오류

- 구매자의 돈이 출금된 뒤 DB가 다운.

- 구매자의 돈이 출금 되지 않았는데 판매자에게 입금됨.

- 출금, 입금 모두 되지 않음.



### 해결방법

* 어중간한 상태는 안된다. 전부 없었던 일로 해주자!(default값으로 회귀)

*  = 트랜잭션의 역할.



![img](https://blog.kakaocdn.net/dn/b9Lr0T/btq9f6xRLp7/R2K5Hf8ksSKKj4twM2WmR1/img.png)





## 트랜잭션 있는 입출금 매커니즘.

```mysql
BEGIN TRAN
UPDATE accounts
SET balance = balance - 10000
WHERE user = '구매자';


UPDATE accounts
SET balance = balance + 10000
WHERE user = '판매자';
COMMIT TRAN
```



![img](https://blog.kakaocdn.net/dn/bxV1KD/btq9euUeChN/P0xcH8tTsT9qK3lbwTSvJ1/img.png)

### 순서

* 쿼리 요청
* 데이터캐시 확인(데이터가 없다면)
* 데이터 파일에서 데이터 가져와 업데이트
* 업데이트선행작업으로 로그에 기록을 함
* (ReDo 로그 : 변경 후 로그, UnDo로그: 변경 전 로그)

![img](https://blog.kakaocdn.net/dn/dMm9St/btq9fl3mLiN/LSXokN3UL9iVO1NGSDWnqK/img.png)

* 입금 쿼리 데이터 캐시 업데이트

![img](https://blog.kakaocdn.net/dn/LIOA8/btq9gXtER3c/I0Rr0p2594IjlXPqH4PEu1/img.png)



![img](https://blog.kakaocdn.net/dn/OQQtA/btq9hz69A5L/qM2wgW4c5fenklOCbOARVK/img.png)

* 입금

![img](https://blog.kakaocdn.net/dn/l171j/btq9floK3g7/5671AOOPDFZyCRiChvHvmk/img.png)

* 트랜잭션 완료 REDO로그에 COMMIT 기록.



> TRANSACTION은 COMMIT or ROLLBACK 두가지 상태로 나뉜다.
>
> (실행되거나 실행을 취소하고 DB를 트랜잭션 이전 상태로 되돌리는 것.)

------



## **ROLLBACK이슈 발생!(에러로 롤백명령을 내림)**

## ![img](https://blog.kakaocdn.net/dn/cqGCrL/btq9fXAIaoR/xmMZjRJ81jy9mAesM4Xf80/img.png)



UNDO로그로 데이터 복원(역순으로 올라가며)

왜냐하면 지금까지 데이터 캐시에서 데이터를 업데이트해서 변경된 상태로 있을 것이고,

UNDO로그로 다시 원래값으로 복원한다.

=> 데이터 캐시를 일관성있게 만들어줌.

![img](https://blog.kakaocdn.net/dn/dP7d30/btq9f5eGHYK/x3IKpWnRsaKNO05kx3quhK/img.png)

롤백이슈가 아닌 OS문제나 전원이 꺼진다던가 다른 곳에 문제가 생긴경우

ReDo로그를 이용해 데이터를 복원, 일관성있게 정렬하고 UnDo로그를 역순으로 정렬함.



------

### 만약 트랜잭션들에 동시 접근이 허용된다면?

* 구매자와 판매자 잔액간 혼동이 올 수 있음.

1. LOCK(락)을 이용. 트랜잭션이 시작되면 이용하고 있는(ROW,TABLE)은 트랜잭션이 끝날 때 까지 점유하는 것.

2. 동알하게 두 쿼리가 날아와도 순차적으로 시행함.

------

트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 성질







## 트랜잭션 관리



### **트랜잭션의 격리 수준**

동시에 DB에 접근 할 때 그 접은을 어떻게 제어할지에 대한 설정.(4가지 레벨 수준)

| READ-UNCOMMITTED | COMMIT 전의 트랜잭션의 데이터 변경을 다른 트랜잭션에서 읽을 수 있다.(조회가능) Dirty Read, Non-Repeatable Read, Phantom Read 발생가능. -> Dirty Read발생가능(Rollback하기 전에 다른 트랜잭션에서 접근해 data를 읽은 후 Rollback하면 오류가 있는 데이터에 접근한 트랜잭션 B는 Dirty Read를 하게됨. | 위쪽일수록 동시성이 올라가고 (격리수준은 낮아지나 성능은 올라감)  아래쪽일수록 데이터 정합성이 올라간다. (격리수준은 높지만 성능은떨어짐) |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| READ-COMMITTED   | COMMIT이 완료된 데이터만 다른 트랜잭션에서 조회 가능. (COMMIT전후 데이터를 타 트랜잭션에서 읽어 변경사항을 비교함)Non-Repeatable Read, Phantom Read 발생가능. -> Non-Repeatable Read : 같은 트랜잭션에서 Data를 두번 조회(COMMIT전후조회)했는데 불일치가 일어나는 오류. |                                                              |
| REPEATABLE-READ  | 트랜잭션 범위 내에서 조회한 내용이 항상 동일함을 보장. COMMIT전후 데이터를 동일하게 읽어오나 그 사이에 Data변경이나 삭제를 막아 반복 조회시 동일한 값을 보장함. Phantom Read Phantom Read-> Non-Repeatable Read의 종류중 하나. SELECT문을 쓸때 나타날 수 있는 현상. 해당 쿼리에 읽히거나 들어가는 행위, 새로 row가 생기거나 없어져있는 현상. |                                                              |
| SERIALIZABLE     | 한 트랜잭션에서 사용하는 데이터를 다른 트랜잭션에서 접근 불가.단순 SELECT만으로도 TRANSACTION에서 잠금설정. |                                                              |





### **트랜잭션 전파 타입**

트랜잭션이 시작하거나 참여하는 방법에 관한 설정. 트랜잭션 경계에서 트랜잭션이 어떻게 동작할 것인가?

```java
public class ServiceA {

	private ServiceB serviceB;
    
    ...
    
    @Transactionalz
    public void a() {
    	service.b();
    }
}

public class ServiceB {
	@Transactional
    public void b() {...}
}
```

예시코드는 트랜잭션이 처리되는 안에서 다른 트랜잭션이 처리된느 경우.

부모 트랜잭션이 있냐 없냐에 따라 타입별로 전파타입을 설정할 수 있다.

![img](https://blog.kakaocdn.net/dn/rJrCV/btq9fF1Fuj2/SCjenLBtcX5daixUOqGTW0/img.png)

스프링 트랜잭션은 각각 격리레벨과 전파타입을 제공하는데 중요한 데이터의 안전성에 관한 설정이므로 케이스에 따라 문제가 발생하지 않도록 각 속성을 잘 이해하고 설정해야 한다.



## Reference(Redo, Undo, 전파타입 등 추가공부 참고링크)

* [https://velog.io/@syleemk/%EB%A9%B4%EC%A0%91-%EB%8C%80%EB%B9%84-%EC%8A%A4%ED%94%84%EB%A7%81-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98-%EC%A0%84%ED%8C%8C﻿](https://velog.io/@syleemk/면접-대비-스프링-트랜잭션-전파)

* https://d2.naver.com/helloworld/407507