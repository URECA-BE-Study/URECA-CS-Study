## 박상윤 
<details>
<summary><b>DB Lock 수준 3가지를 말하시오.</b></summary>
<div markdown="1">
  <br>
  
- 행 수준 잠금 : 개별 행 단위로 잠근다.
- 테이블 수준 잠금 : 테이블과 인덱스 모두 잠근다.
- 데이터베이스 수준 잠금 : DB 복구 or 스키마 변경 시 발생
</div>
</details>

<details>
<summary><b>Global Lock을 사용하면 사용 가능한 쿼리 및 불가능한 쿼리와 왜 사용하는지 말하라</b></summary>
<div markdown="1">
  <br>

- 사용 가능한 쿼리 : SELECT
- 사용 불가능한 쿼리 : DDL, DML 등

DB의 구조적 변경, 백업 수행 또는 크리티컬한 데이터 마이그레이션 작업에 사용
</div>
</details>

<details>
<summary><b>MySQL InnoDB에서 Backup Lock이 도입된 배경은?</b></summary>
<div markdown="1">
  <br>
  원래 백업하려면 모든 데이터를 잠그는 락(글로벌 락)을 걸어야 했다.
  InnoDB는 변경 중이던 데이터도 안정적으로 백업할 수 있게 되었다. 
  그래서 굳이 전체를 멈출 필요가 없고, 좀 더 가벼운 락으로도 충분해졌기 때문이다. 
  (좀 더 가벼운 글로벌 락 = 백업 락)
  백업 락은 일반적인 테이블의 변경은 허용되고 스키마 변경 같은 DDL 명령어가 실행되면 복제가 일시 중지된다.
</div>
</details>

<details>
<summary><b>Intention Lock에서 Intention Shared Lock과 Intention Exclusive Lock 사용 시 걸리는 락의 종류는?</b></summary>
<div markdown="1">
  <br>
  
- intention Shared Lock이 실행되면, row-level에 S-Lock 이 걸림(공유 락)
- intention Exclusive Lock이 실행되면, row-level에 X-Lock이 걸림(배타 락)
</div>
</details>

<details>
<summary><b>Shared Lock과 Exclusive Lock의 특징</b></summary>
<div markdown="1">
  <br>
  
- Shared Lock -  하나의 트랜잭션이 데이터를 읽고 있는 경우, 다른 트랜잭션들도 해당 데이터를 읽을 수는 있지만, 쓰기 작업은 금지된다.
- Exclusive Lock - 특정 트랜잭션이 데이터를 수정하고 있는 경우, 해당 데이터에 대한 다른 모든 트랜잭션의 접근이 금지된다.
</div>
</details>

<details>
<summary><b>InnoDB는 row-level 락을 사용하면서도 왜 table-level의 Intention Lock(IS, IX)을 함께 사용하는가? 그 이유 설명</b></summary>
<div markdown="1">
  <br>
  
- InnoDB는 row-level 락만 사용하면 성능은 좋지만, 테이블 단위 DDL 충돌을 감지하기 어려움
- 따라서 트랜잭션이 특정 row를 잠글 때, 동시에 테이블 수준에 IS/IX 락을 걸어, 다른 트랜잭션이 LOCK TABLE, ALTER TABLE 등 시도 시 충돌을 빠르게 감지하게 함   
</div>
</details>

<details>
<summary><b>다음 트랜잭션으로 인해 어떤 상황이 발생될까요?</b></summary>
<div markdown="1">
  <br>

| **트랜잭션 A**                                         | **트랜잭션 B**                                         |
|------------------------------------------------------|------------------------------------------------------|
| UPDATE users SET ... WHERE id = 1 → X-Lock on row 1 | UPDATE users SET ... WHERE id = 2 → X-Lock on row 2 |
| UPDATE users SET ... WHERE id = 2 → **row 2 요청**   | UPDATE users SET ... WHERE id = 1 → **row 1 요청**   |

  <details>
  <summary>정답 보기</summary>
  
  Dead Lock
  
  </details>

</div>
</details>

<details>
<summary><b>DB 시스템이 말합니다. “야 row-level lock이 너무 많으니깐, 걍 테이블 전체 잠궈” 이 말이 의미하는게 뭘까요?</b></summary>
<div markdown="1">
  <br>
  Lock Escalation

  row-level 락이 과도하게 많아져서 관리 비용이 커질 경우, DBMS가 자동으로 더 상위 수준인 table-level 락으로 전환할 수 있다.
</div>
</details>

<br>

## 변하영
<details>
<summary><b>Row-Level Lock과 Table-Level Lock은 각각 어떤 상황에서 사용하는게 더 적절할까요?</b></summary>
<div markdown="1">
  <br>

   Row-Level Lock은 동시성이 중요한 서비스 환경에 적합합니다. 반면에 Table-Level Lock은 대량의 데이터를 일괄로 수정하거나 삭제하는 배치 작업처럼 일관성이 중요한 작업에 적합합니다. 
</div>
</details>

<details>
<summary><b>InnoDB는 왜 Intention Lock을 사용하나요?</b></summary>
<div markdown="1">
  <br>
  테이블에 어떤 행에 대한 락이 걸릴 예정이라는 정보를 미리 표시하여 효율적인 락 충돌 관리를 가능하게 합니다.
   
</div>
</details>

<details>
<summary><b>공유 락과 배타 락의 차이점은 무엇인가요?</b></summary>
<div markdown="1">
  <br>
공유 락은 데이터를 읽을 수는 있지만 쓰기는 제한되고, 배타 락은 읽기와 쓰기 모두 다른 트랜잭션이 접근할 수 없게 합니다. 
</div>
</details>

<details>
<summary><b>의도적으로 Row Lock과 Table Lock 을 중복해서 거는 이유는 무엇인가요?</b></summary>
<div markdown="1">
  <br>
  서로 다른 트랜잭션이 테이블 구조와 행 데이터를 동시에 변경하는 것을 방지하기 위해서입니다. 
  예를 들어, 행 데이터를 수정 중일 때 스키마 변경을 막기 위해 Intention Lock을 통해 Table-level Lock도 함께 관리합니다.
</div>
</details>

<details>
<summary><b>Dead Lock은 무엇이며, 어떻게 해결할 수 있나요?</b></summary>
<div markdown="1">
  <br>
  Dead Lock은 두 개의 트랜잭션 간에 각각의 트랜잭션이 가지고 있는 리소스의 Lock을 획득하려고 할 때 발생합니다.
  이를 예방하려면 트랜잭션의 자원 접근 규칙을 정하거나 Dead Lock이 감지되면 둘 중 하나의 트랜잭션을 강제종료 합니다.
</div>
</details>

<details>
<summary><b>낙관적 락과 비관적 락은 각각 언제 유리한가요?</b></summary>
<div markdown="1">
  <br>
  동시성이 높지만 충돌 가능성이 적은 경우 낙관적 락이 유리합니다.
  반대로 충돌 가능성이 높거나 중요한 데이터를 다룰 경우에는 데이터 무결성을 위해 비관적 락을 사용하는 것이 유리합니다.
</div>
</details>

<br>

## 신예지
<details>
<summary><b>Global Lock과 Backup Lock의 차이를 설명해주세요</b></summary>
<div markdown="1">
  <br>

  - Global Lock은 모든 테이블에 대한 읽기 전용 락, 거의 모든 DML/DDL 막힘
  - Backup Lock은 DDL만 막고 DML 허용, InnoDB 백업용
   
</div>
</details>

<details>
<summary><b>Intention Exclusive Lock과 Shared Lock을 동시에 걸 수 있나요?</b></summary>
<div markdown="1">
  <br>

  - 락을 걸 수 없음
</div>
</details>

<details>
<summary><b>Escalation에 대해 설명해주세요</b></summary>
<div markdown="1">
  <br>

  잠금 수준을 최적화하기 위해 데이터베이스 시스템이 특정 Lock Level에서 다른 Lock Level로 전환하는 과정을 의미
</div>
</details>

<details>
<summary><b>Lock level이 낮아질수록 동시성과 메모리 효율성은 어떻게 될까요?</b></summary>
<div markdown="1">
  <br>

   Lock 레벨이 낮을 수록 동시성은 좋아지지만, 관리해야할 Lock 이 많아지기 때문에 메모리 효율성은 떨어짐
</div>
</details>

<details>
<summary><b>Blocking의 해결방안을 말해주세요</b></summary>
<div markdown="1">
  <br>

   1. SQL문의 리펙토링
   2. 트랜잭션을 가능한 짧게 정의
   3. 동일한 데이터를 동시에 변경하는 작업을 하지 않도록 설계
   4. 대용량 작업이 불가피한 경우, 작업 단위를 쪼개거나 lock_timeout을 설정
</div>
</details>

<details>
<summary><b>데이터베이스 수준 잠금은 보통 언제 일어나나요?</b></summary>
<div markdown="1">
  <br>

  데이터베이스를 복구하거나 스키마를 변경할 때 발생한다.
   
</div>
</details>

<br>

## 이소원
<details>
<summary><b>(o, x) Intention lock은 table lock 이다.</b></summary>
<div markdown="1">
  <br>
   (o)
  
  Intention Lock은 table Lock 입니다.
  트랜잭션이 특정 테이블의 어떤 행에 락을 걸려고 하니 알아두라는 선언적 의미로 사용됩니다.
</div>
</details>

<details>
<summary><b>S Lock 과 X Lock의 차이를 설명해주세요.</b></summary>
<div markdown="1">
  <br>

  1. S Lock
     - Shared Lock
     - 데이터를 읽을 때 사용하는 Lock
     - 하나의 트랜잭션이 데이터를 읽고 있는 경우, 다른 트랜잭션들도 해당 데이터를 읽을 수는 있지만 쓰기 작업은 금지됩니다.
     - 데이터의 일관성을 유지할 수 있습니다.
     - 트랜잭션 격리 수준 중 Repeatable Read 단계와 연관
  2. X Lock
     - Exclusive Lock
     - 데이터를 변경하고자 할 때 사용됩니다.
     - 트랜잭션이 완료될 때까지 유지됩니다.
     - 특정 트랜재겻ㄴ이 update, delete 등을 통해 데이터를 수정하고 있는 경우, 다른 모든 트랜잭션의 접근이 금지됩니다.
     - 트랜잭션 격리 수준 중 Serializable 단계와 연관
</div>
</details>

<details>
<summary><b>row-level 및 table-level 에서 두 번 Lock 을 하는 이유는?</b></summary>
<div markdown="1">
  <br>

   A 트랜잭션에서 이미 테이블에 대해 락이 걸려있는데, B 트랜잭션에서 해당 테이블의 특정 row에 대한 lock을 거는 것을 원천적으로 방지할 수 있습니다. (반대의 경우도 마찬가지)
</div>
</details>

<details>
<summary><b>Dead Lock이 발생하면 어떻게 해결할 수 있을까요?</b></summary>
<div markdown="1">
  <br>

   1. Dead Lock이 감지되면 둘 중 하나의 트랜잭션을 강제 종료 합니다.
   2. 접근 순서 규칙을 정합니다.
</div>
</details>

<details>
<summary><b>읽기 위주, 충돌이 드문 시스템에서는 어떤 락을 사용해야 할까요?</b></summary>
<div markdown="1">
  <br>

   Pessimistic Lock
</div>
</details>
