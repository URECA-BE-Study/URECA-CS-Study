## 변하영

<details>
<summary>QueryDSL과 JPQL의 차이점은 무엇이며 언제 QueryDSL을 사용하는 것이 유리한가요?</summary>
JPQL은 문자열 기반이라 동적 쿼리나 리팩토링에 불편함이 있지만, QueryDSL은 타입 안전한 코드 기반이라 컴파일 시점 오류를 방지할 수 있습니다. 특히 복잡하거나 조건이 자주 바뀌는 동적 쿼리 상황에서는 QueryDSL이 유리합니다.
</details>

<details>
<summary>JPA에서 트랜잭션 내에서 같은 엔티티를 두 번 조회하면 항상 같은 객체인가요? 이유는 무엇인가요?</summary>
JPA는 1차 캐시(영속성 컨텍스트)를 이용해 트랜잭션 내에서 같은 식별자의 엔티티를 조회할 경우 동일한 인스턴스를 반환합니다.
</details>

<details>
<summary>객체와 RDB 사이의 패러다임 불일치 문제에는 무엇이 있는지 설명하고 JPA가 이를 어떻게 해결하는 지 설명해주세요.</summary>
객체와 RDB 간 패러다임 불일치 문제는 대표적으로 상속, 연관관계 표현, 그래프 탐색, 비교의 차이가 있습니다. 객체는 상속과 참조 중심 구조지만, RDB는 테이블과 외래 키 기반이라 직접 매핑이 어렵습니다. JPA는 상속 전략, 연관관계 어노테이션, 지연 로딩, 1차 캐시 같은 기능으로 이 불일치를 해결합니다.
</details>

<details>
<summary>Spring Data JPA에서 Repository 인터페이스는 어떤 역할을 하며, 어떻게 동작하나요?</summary>
Spring Data JPA는 JPA를 더 추상화한 Repository 인터페이스를 제공합니다. 개발자가 JpaRepository를 상속하면, Spring Data JPA가 내부적으로 구현 클래스를 프록시 기술로 자동 생성하고 스프링 빈으로 등록해줍니다. 덕분에 EntityManager를 직접 다루지 않고도 데이터 접근 가능합니다. 
</details>

<details>
<summary>EntityManager와 Spring Data JPA의 Repository는 어떤 차이가 있나요?</summary>
EntityManager는 유연하지만 직접 관리해야 할 게 많고, Repository는 구현체가 자동 생성되어 간단하고 유지보수하기 좋습니다.
</details>

---

## 김수훈

<details>
<summary>ORM이 무엇인지 간단하게만 설명해주세요</summary>
ORM은 객체와 데이터베이스 테이블을 자동으로 매핑해주는 기술로, SQL을 직접 사용하지 않고 객체 중심으로 DB를 다룰 수 있게 해주는 도구입니다.
</details>

<details>
<summary>영속성 컨텍스트가 무엇인지 간단하게 설명해주세요</summary>

영속성 컨텍스트는 엔티티 객체를 저장하고 관리하는 JPA 내부의 메모리 공간입니다.

[참고]

EntityManager를 통해 데이터를 조회하거나 저장하면, 엔티티는 영속성 컨텍스트에 보관되고, 이 상태를 "영속 상태"라고 부릅니다.
동일한 트랜잭션 내에서는 같은 엔티티가 1차 캐시에 저장되어 있기 때문에, 같은 데이터를 두 번 조회해도 쿼리가 한 번만 날아갑니다.

또한, 트랜잭션이 끝날 때까지 변경된 내용은 자동으로 감지되어(Dirty Checking), 별도로 update 쿼리를 작성하지 않아도 데이터베이스에 반영됩니다.

</details>

<details>
<summary>JPA와 Hibernate의 차이가 무엇인가요?</summary>

JPA는 Java Persistence API의 약자로, 자바에서 ORM을 표준화하기 위한 명세입니다.

Hibernate는 JPA를 구현한 구현체 중 하나로, 실제로 JPA의 기능을 동작하게 만드는 라이브러리입니다.

쉽게 말하면, JPA는 규칙서이고, Hibernate는 그 규칙에 맞춰 만든 실제 프로그램입니다.

</details>

<details>
<summary>JPA를 사용하는 이유가 무엇인가요?</summary>

JPA를 사용하는 이유는 객체와 관계형 데이터베이스 간의 패러다임 불일치를 해결하고, 생산성과 유지보수성을 높이기 위해서입니다.

JPA를 사용하면 반복적인 SQL 작성 없이 객체지향적으로 CRUD 작업을 처리할 수 있고, 엔티티 필드 변경 시에도 SQL이 자동으로 반영되어 유지보수가 편리합니다.

또한, 1차 캐시와 쓰기 지연, 지연 로딩 등 다양한 성능 최적화 기능을 제공하며, 추상화된 구조 덕분에 DB 벤더 교체도 유연하게 대응할 수 있습니다.

</details>

<details>
<summary>JPA의 기능 중 지연로딩과 즉시로딩에 대해 설명하고 각 방식의 단점을 설명해주세요</summary>

JPA에서 지연 로딩(Lazy Loading)은 연관된 엔티티를 실제로 사용할 때까지 로딩을 미루는 방식이고,

즉시 로딩(Eager Loading)은 연관된 엔티티를 조회 시점에 함께 즉시 로딩하는 방식입니다.

예를 들어, Member와 Team이 연관된 관계일 때, Lazy 로딩을 사용하면 Member만 조회하고 getTeam()을 호출할 때 그제서야 Team엔티티를 추가로 가져옵니다.

반면 Eager 로딩은 Member를 조회할 때 Team까지 조인해서 함께 가져오므로 처음부터 모두 로딩됩니다.

Lazy는 초기 성능을 최적화할 수 있지만, 반복 접근 시 N+1 문제가 발생할 수 있고,

Eager는 성능이 예측 가능하지만 불필요한 데이터를 미리 로딩해 메모리를 낭비할 수 있습니다.

</details>

---

## 이소원

<details>
<summary>JPA는 왜 쓰나요?</summary>

JPA는 다양한 ORM 프레임워크에서 구현할 수 있는 공통 api를 제공하여, 관계형 DB와 객체의 패러다임 불일치를 해결합니다.

JPA를 사용하면 생산성이 향상되고, 유지보수에 용이하며 성능이 향상되고, 인터페이스로 추상화된 데이터 접근을 제공하기 때문에 DB 변경시 JPA에게 알려 종속성 해결이 가능합니다.

</details>

<details>
<summary>JPA의 상속 전략 3가지가 뭔가요?</summary>

단일 테이블 전략, 조인 전략, 테이블당 클래스 전략이 있습니다.

객체 지향은 상속이 일반적인 반면, 데이터베이스에는 상속의 개념이 없습니다.

이를 해결하기 위해 위 3가지 전략을 사용합니다.

</details>

<details>
<summary>JPQL과 SQL의 차이점은 무엇인가요?</summary>

JPQL은 Java 객체(Entity)를 대상으로 질의하는 객체지향 쿼리 언어이고, SQL은 데이터베이스 테이블을 직접 대상으로 질의하는 언어입니다.

JPQL은 `SELECT e FROM Employee e`처럼 Entity와 그 속성을 기준으로 작성해서 데이터베이스 독립적인 쿼리를 만들 수 있습니다. 반면 SQL은 `SELECT * FROM employee`처럼 테이블과 컬럼을 직접 지정합니다.

또한 JPQL은 JPA가 실행 시점에 데이터베이스에 맞는 SQL로 변환해서 보내기 때문에, 같은 코드로 여러 데이터베이스를 지원할 수 있습니다. 대신, JPQL은 데이터베이스 고유 기능(예: 특정 함수 사용)에 바로 접근하는 건 제한적입니다. 필요할 때는 네이티브 SQL을 사용합니다.

</details>

<details>
<summary>QueryDSL은 무엇이며 어떤 장점이 있나요?</summary>

QueryDSL은 자바 코드로 SQL 같은 쿼리를 타입 안전하게 작성할 수 있게 도와주는 프레임워크입니다.

일반적으로 JPQL이나 SQL은 문자열로 작성하기 때문에, 컴파일 시점에 문법 오류를 잡을 수 없고 런타임 에러가 발생할 위험이 있습니다. 반면 QueryDSL은 Java 코드로 쿼리를 작성하기 때문에 **컴파일 시점에 문법 오류를 잡을 수 있고**, IDE의 **자동완성** 기능도 사용할 수 있어서 생산성과 안정성이 높아집니다.

또, QueryDSL은 복잡한 동적 쿼리도 깔끔하게 관리할 수 있는 장점이 있어서, 유지보수나 가독성 측면에서도 유리합니다.

</details>

<details>
<summary>Hibernate Session과 JPA EntityManager의 차이점을 설명해주세요</summary>

Hibernate Session은 Hibernate에 특화된 API를 제공하며, JPA EntityManager는 JPA 규격을 따르는 표준 API입니다.

JPA를 사용하면 구현체가 바뀌더라도 같은 코드를 사용할 수 있어 플랫폼 독립성이 더 높습니다.
Hibernate Session을 사용하면 Hibernate의 고유한 기능을 더욱 직접적으로 활용할 수 있습니다.

</details>

---

## 박상윤

<details>
<summary>JPA란 무엇인가요?</summary>

Java 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스입니다.

</details>

<details>
<summary>JPA를 사용하는 이유는?</summary>

JPA는 객체지향 프로그래밍과 관계형 데이터베이스 간의 패러다임 불일치를 해결하기 위해 사용됩니다. SQL과 JDBC에 비해 코드량이 줄고, 객체 중심으로 개발할 수 있어 생산성과 유지보수성이 높아집니다.

</details>

<details>
<summary>Hibernate란 무엇인가?</summary>

Hibernate는 이 명세를 구현한 대표적인 구현체입니다.

</details>

<details>
<summary>Hibernate에서 Session은 어떤 역할을 하는 객체인가요?</summary>

데이터베이스 작업을 수행하는 객체 입니다.

엔티티 저장, 수정, 삭제, 조회 시 사용합니다.

</details>

<details>
<summary>Spring Data JPA란 무엇인가?</summary>

Spring에서 제공하는 JPA를 쓰기 편하게 만들어 놓은 모듈입니다.

</details>

---

## 신예지

<details>
<summary>JPA란 무엇인가요?</summary>

Java 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스입니다.

</details>

<details>
<summary>관계형 데이터베이스와 객체의 사이에는 패러다임 불일치 문제가 존재합니다. 이중 연관관계 문제에서 어떤 문제점이 있고, JPA는 이를 어떻게 해결하나요?</summary>

객체 지향 프로그래밍은 객체 간의 참조로 연관관계 표현하지만 관계형 데이터베이스는 외래키로 연관관계를 표현하기 때문에 패러다임 불일치 문제가 발생합니다. JPA는 애노테이션을 사용하여 객체 간의 연관관계를 데이터베이스 테이블의 외래 키와 매핑하여 해결합니다.

</details>

<details>
<summary>QueryDsl의 특징 3가지를 말해주세요</summary>

- 쿼리 작성 시 타입 안전성을 보장하여 컴파일 시점에 오류를 검출합니다.
- 메서드 체이닝을 통해 가독성이 높은 쿼리를 작성할 수 있습니다.
- IDE에서 자동 완성을 지원하여 개발 생산성 높일 수 있습니다.
- SQL, JPA, MongoDB, Elasticsearch 등 다양한 데이터 소스에 대한 쿼리를 지원합니다.
- 동적 쿼리 편하게 작성할 수 있습니다.

</details>

<details>
<summary>객체지향 프로그래밍에서 동일성 비교와 동등성 비교의 차이점을 설명해주세요</summary>

동일성 비교는 두 객체의 참조가 같은지를 비교합니다. 반면 동등성 비교는 두 객체가 내용상으로 같은지를 비교합니다.

</details>

<details>
<summary>Hibernate의 Session에 대해 설명해주세요</summary>

Session은 JPA의 EntityManager를 구현한 데이터베이스 작업을 수행하는 객체입니다.

엔티티 저장, 조회, 수정, 삭제 시 사용되고, 짧은 시간 동안 유지되며 사용 후에는 닫아야 합니다.

</details>
