## 박상윤

<details>
<summary><b>트랜잭션 연산 작업 종류와 각 작업이 의미하는 것은?</b></summary>
<div>

커밋 - 하나의 트랜잭션이 성공적으로 끝났으며, 데이터베이스가 일관성이 있는 상태로 유지될 때 트랜잭션이 마무리되었다는 것을 트랜잭션 관리자에게 알리기 위한 연산입니다.

롤백 - 트랜잭션 처리가 비정상적으로 종료된 경우, 트랜잭션을 다시 시작하거나 트랜잭션의 부분 연산 결과를 취소 하는 연산 입니다.

</div>
</details>

<details>
<summary><b>JDBC 기반으로 트랜잭션을 수동 처리할 때 발생할 수 있는 문제를 설명하고, Spring의 트랜잭션 관리가 이를 어떻게 해결하는지 설명하시오.</b></summary>
<div>

- 수동 트랜잭션은 connection을 직접 전달해야 하고, 커밋/롤백 관리 로직이 서비스 계층에 노출됨  
- 트랜잭션 범위와 동기화 문제가 발생할 수 있음  
- Spring은 트랜잭션 동기화와 추상화를 제공하여 개발자는 @Transactional 만으로 관리 가능하게 함

</div>
</details>

<details>
<summary><b>트랜잭션을 시작하기 위한 Connection 객체를 특별한 저장소에 보관해두고 필요할 때 꺼내쓸 수 있도록 하는 기술과 사용하는 클래스는?</b></summary>
<div>

트랜잭션 동기화, TransactionSynchronizationManager

</div>
</details>

<details>
<summary><b>Spring이 제공하는 PlatformTransactionManager의 도입 목적을 설명하시오.</b></summary>
<div>

- JDBC, JPA, Hibernate 등 다양한 트랜잭션 기술의 공통 기능(시작, 커밋, 롤백)을 추상화합니다.  
- 서비스 계층이 특정 트랜잭션 기술에 의존하지 않고 일관된 방식으로 트랜잭션 처리 가능합니다.  
- @Transactional 같은 선언적 방식도 내부적으로 PlatformTransactionManager를 사용합니다.

</div>
</details>

---

## 신예지

<details>
<summary><b>순수 JDBC로 트랜잭션을 구현할 경우 어떤 문제가 있는지 한 가지만 설명해주세요</b></summary>
<div>

순수 JDBC로 트랜잭션을 구현할 경우 다음과 같은 문제가 발생할 수 있습니다.

- 트랜잭션 동기화 문제  
- JDBC 구현 기술이 서비스 계층에 누수되는 문제  
- 순수 비즈니스 로직과는 다른 관심사의 일(DB 접근 관련)을 수행하고 있음  
- 트랜잭션 적용 코드 반복 문제  

</div>
</details>

<details>
<summary><b>TransactionSynchronizationManager 는 어떤 역할을 하나요?</b></summary>
<div>

트랜잭션 동기화 저장소로, 작업 스레드마다 독립적으로 Connection 객체를 저장하고 관리합니다.

</div>
</details>

<details>
<summary><b>MySQL의 Default 격리수준은 무엇인가요?</b></summary>
<div>

Reapetable Read 입니다.

</div>
</details>

<details>
<summary><b>읽기에 @Transactional(readonly=true)를 붙이는 이유를 설명해주세요</b></summary>
<div>

JPA의 경우, 해당 옵션을 true로 설정하면 트랜잭션이 커밋되어도 영속성 컨텍스트를 플러시 하지 않습니다.  
플러시할 때 수행되는 엔티티는 변경 감지를 위한 스냅샷 비교 로직이 수행되지 않으므로 성능을 약간 향상 시킬 수 있음

</div>
</details>

<details>
<summary><b>PlatformTransactionManager는 어떻게 사용는지 설명해주세요</b></summary>
<div>

사용할 DB(JDBC)의 DataSource를 생성자에 넣어 트랜잭션 추상 오브젝트를 생성하여 사용합니다.

</div>
</details>

---

## 김수훈

<details>
<summary><b>트랜잭션이 필요한 이유가 무엇인가요?</b></summary>
<div>

트랜잭션은 데이터베이스의 데이터를 수정하는 도중에 예외가 발생할 경우, DB의 데이터들은 수정되기 전의 상태로 돌아가야 하기 때문입니다.  
이 때문에 데이터의 일관성을 유지하면서 안정적으로 데이터를 복구하기 위해서 트랜잭션이 필요합니다.

</div>
</details>

<details>
<summary><b>MySQL의 default 격리수준은 무엇인가요?</b></summary>
<div>

REPEATABLE READ입니다.

</div>
</details>

<details>
<summary><b>트랜잭션의 옵션으로 @Transactional(readonly=true)를 붙이는 이유가 무엇인가요?</b></summary>
<div>

트랜잭션에서 @Transactional(readonly=true)를 사용하는 이유는 성능 최적화 때문입니다.  
이 옵션을 설정하면 영속성 컨텍스트가 변경 감지와 플러시를 수행하지 않아서, 데이터베이스 조회 작업만 있을 때 불필요한 비용을 줄여 성능을 향상시킬 수 있습니다.  
따라서 주로 읽기 전용 트랜잭션에서 사용됩니다.

</div>
</details>

<details>
<summary><b>@Transactional 어노테이션의 동작원리를 설명해주세요</b></summary>
<div>

트랜잭션은 스프링이 지원하는 어노테이션 기반 AOP를 통해 구현되어 있습니다.  
그렇기 때문에 클래스나 메소드에 @Transactional이 선언되면 해당 클래스에 트랜잭션이 적용된 프록시 객체가 생성되며  
프록시 객체는 @Transactional이 포함된 메서드가 호출될 경우 트랜잭션을 시작하고 Commit 또는 Rollback을 수행합니다.

</div>
</details>

---

## 이소원

<details>
<summary><b>순수 JDBC를 사용하면 어떤 문제가 발생하고, 어떻게 해결할 수 있을까요?</b></summary>
<div>

첫 번째로, 순수 JDBC로 구현하게 되면 개발자가 여러 개의 작업을 하나의 커넥션으로 관리했을 때, 커넥션 객체를 공유하는 것과 같은 많은 작업들을 하게 되어 동기화 문제가 발생합니다.  
두 번째로, 트랜잭션을 적용하기 위해 서비스 계층이 JDBC 기술에 의존되어 있기 때문에 JDBC 구현 기술이 서비스 계층에 누수되는 문제가 발생할 수 있습니다.  
세 번째로, 순수 비즈니스 로직과는 다른 관심사인 예를들어 DB 접근 관련한 일을 수행하고 있는 문제가 발생할 수 있습니다.

동기화 문제를 해결하기 위해 Spring 트랜잭션 동기화를 적용할 수 있고.  
JDBC 기술이 서비스 계층에 누수되는 문제를 해결하기 위해 Spring 트랜잭션 추상화를 적용할 수 있고,  
순수 비즈니스 로직과 다른 관심사의 일을 수행하는 것을 방지하기 위해 소스코드에 트랜잭션 관련 로직을 넣지 않고, AOP를 이용하여 비즈니스 로직에서 완전히 분리할 수 있습니다.

</div>
</details>

<details>
<summary><b>MySQL의 Default 격리 수준은 뭔가요?</b></summary>
<div>

REPEATABLE READ

</div>
</details>

<details>
<summary><b>@Transactional(readOnly = true)를 사용하면 뭐가 좋은가요?</b></summary>
<div>

변경을 감지하지 않기 때문에 트랜잭션 컷밋할 때에도 플러시(변경사항 반영)을 하지 않습니다.  
따라서 메모리 사용이 줄고, 속도도 좀 더 빨라집니다.

</div>
</details>

<details>
<summary><b>스프링부트에서 트랜잭션을 관리하는 방법은 뭔가요?</b></summary>
<div>

스프링 부트에서 트랜잭션을 관리하는 방법은 AOP(관점 지향 프로그래밍) 기반으로, @Transactional 어노테이션을 사용해서 관리합니다.

</div>
</details>

<details>
<summary><b>예외가 발생했을 때 트랜잭션은 롤백되나요?</b></summary>
<div>

기본적으로 RuntimeException 또는 Error 가 발생하면 롤백을 합니다.  
반면, CheckedException 예외처리에서는 롤백을 하지 않습니다.  
이러한 특정 예외에서 rollbackFor 옵션을 설정하여 롤백을 처리할 수 있습니다.

</div>
</details>

---

## 변하영

<details>
<summary><b>@Transactional 어노테이션에 대해 설명해주세요</b></summary>
<div>

스프링에서 많이 사용되는 선언적 트랜잭션 방식으로, 적용된 범위가 트랜잭션이 되도록 보장해줍니다.  
getConnection(), setAutoCommit(false), commit(), rollback() 등의 JDBC에서 필요한 코드를 삽입해줍니다.  
보통 서비스 클래스 혹은 메서드 단위에 어노테이션을 추가하여 사용합니다.

</div>
</details>

<details>
<summary><b>서비스 계층에 @Transactional을 붙이는 이유는 무엇인가요?</b></summary>
<div>

서비스 계층은 여러 DAO를 묶어 하나의 비즈니스 로직을 구성하므로 트랜잭션 범위를 관리하기 적합합니다.

</div>
</details>

<details>
<summary><b>@Transactional(readOnly=true)를 왜 사용하나요?</b></summary>
<div>

읽기 전용으로 지정하면 JPA는 변경 여부를 체크하지 않게 해줍니다.  
그래서 변경 감지용 스냅샷을 만들지 않고, flush도 생략되므로 성능이 향상됩니다.

</div>
</details>

<details>
<summary><b>테스트 환경에서 @Transactional을 사용하면 어떤 장점이 있나요?</b></summary>
<div>

각 테스트마다 트랜잭션을 시작하고 테스트 후 자동으로 롤백되므로 DB를 초기화할 필요가 없습니다.  
따라서 반복 테스트가 용이하고, 테스트 간 데이터 영향이 없습니다.

</div>
</details>

<details>
<summary><b>TransactionSynchronizationManager가 무엇인가요?</b></summary>
<div>

트랜잭션 동기화 저장소로, 작업 스레드마다 독립적으로 Connection 객체를 저장하고 관리합니다.  
작업 스레드마다 connection 객체를 독립적으로 관리하기에 멀티스레드 환경에서도 충돌이 발생하지 않습니다.

</div>
</details>
