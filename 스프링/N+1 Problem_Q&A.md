## 이소원

<details>
<summary><b>지연로딩으로 인해 발생하는 N+1 문제를 즉시로딩으로 해결하면 안되는 이유는?</b></summary>
<div>
  단순히 EAGER로 바꾸면, 쿼리 조회 시점에서 예상치 못하게 대량의 데이터를 가져오게 되어 오히려 성능 저하나 순환 참조 문제가 발생할 수 있기 때문입니다.
</div>
</details>

<details>
<summary><b>즉시로딩과 FETCH JOIN의 차이점이 뭔가요?</b></summary>
<div>
  즉시로딩은 JPA가 엔티티를 조회할 때 연관된 엔티티를 자동으로 즉시로딩하는 방식으로 쿼리 최적화가 불가능합니다.

반면, fetch join은 JPQL (또는 Query DSL 등)을 사용하여 연관된 엔티티를 명시적으로 fetch 하여 필요한 시점에 필요한 데이터만 로딩하는 방식으로 쿼리 최적화에 용이합니다.
</div>
</details>

<details>
<summary><b>fetch join과 @EntityGraph의 차이점이 뭔가요?</b></summary>
<div>
  fetch join은 JPQL 쿼리 내부에 `Join fetch` 구문을 직접 작성하여, 연관된 엔티티를 명시적으로 즉시로딩하는 방식입니다.

@EntityGraph 는 JPA 리포지토리 메서드를 선언해서 특정 쿼리에서만 연관 엔티티를 EAGER 처럼 즉시 로딩하도록 지정하는 방법입니다. 내부적으로는 `left outer join`을 사용하여 데이터를 가져오는 방식입니다.
</div>
</details>

<details>
<summary><b>N+1 문제가 발생하는 원인은 무엇인가요?</b></summary>
<div>
  N+1 문제는 JPA와 같은 ORM에서 연관된 엔티티를 지연 로딩(Lazy Loading) 할 때 발생하는 문제입니다.예를 들어, 부모 엔티티를 한 번 조회한 후, 각 부모 엔티티마다 연관된 자식 엔티티를 조회하면서 쿼리가 추가로 N번 발생하는 구조입니다. 

즉, 총 N+1개의 쿼리가 실행되어 비효율적인 DB 접근과 성능 저하가 발생하게 됩니다. 이는 ORM이 연관된 엔티티를 실제로 접근하는 시점에 별도의 쿼리를 날리기 때문에 발생하는 문제입니다.
</div>
</details>

<details>
<summary><b>Batch size를 이용한 N+1 문제 해결 방법을 설명해주세요</b></summary>
<div>
  N+1 문제는 주로 JPA의 Lazy Loading에서 발생합니다.

이를 해결하는 방법 중 하나가 Batch Size 설정입니다. Hibernate에서는 `@BatchSize` 어노테이션이나 application(yml) 설정을 통해 Lazy 로딩되는 엔티티들을 일정 크기로 묶어 한 번의 쿼리로 조회하도록 할 수 있습니다.

이러한 방식을 통해 Batch size를 이용하여 Lazy 로딩을 유지하면서 N+1 문제를 줄이거나 완화할 수 있습니다.
</div>
</details>

## 신예지

<details>
<summary><b>N+1 문제란 무엇인가요?</b></summary>
<div>
  N+1 문제는 연관관계가 설정된 엔티티를 조회할 때 연관된 엔티티를 조회하는 쿼리가 N + 1번 발생하는 문제입니다.
</div>
</details>

<details>
<summary><b>Lazy와 Eager 차이는 뭔가요?</b></summary>
<div>
  Lazy는 실제 접근할 때 쿼리를 실행하고, Eager는 엔티티를 조회할 때 연관 엔티티까지 즉시 함께 조회합니다.
</div>
</details>

<details>
<summary><b>Fetch Join은 언제 쓰면 좋나요?</b></summary>
<div>
  N+1 문제를 방지하고자 특정 시점에 연관 데이터를 함께 조회해야 할 때 사용합니다.
</div>
</details>

<details>
<summary><b>N+1 문제를 해결하는 방법에는 어떤 것들이 있나요?</b></summary>
<div>
  Fetch Join, @EntityGraph, @BatchSize, Querydsl, MyBatis 등의 방법으로 해결할 수 있습니다.
</div>
</details>

<details>
<summary><b>Join Fetch와 Join의 차이를 설명해주세요</b></summary>
<div>
  단순 JOIN은 SQL의 JOIN처럼 조건 필터링 용도고, FETCH JOIN은 연관 엔티티를 실제로 가져와서 영속성 컨텍스트에 등록하기 위한 JPA 전용 JOIN입니다.
</div>
</details>

## 박상윤
<details>
<summary><b>N+1문제란 무엇이며, 어떤 상황에서 발생하나요?</b></summary>
<div>
  N+1 문제는 JPA와 같은 ORM에서 연관 관계가 설정된 엔티티를 조회할 때, 1개의 쿼리로 N개의 엔티티를 조회한 후, 각 엔티티의 연관 엔티티를 다시 조회하기 위해 N번의 추가 쿼리가 발생하는 현상입니다.
주로 연관 엔티티가 LAZY 로딩으로 설정되어 있을 때 발생하며, 일대다 관계에서 리스트 조회 시 자주 발생합니다.
</div>
</details>

<details>
<summary><b>FetchType.EAGER을 사용하여 N+1 문제를 해결하려고 할 때 주의해야 할 점은 무엇이며, 왜 추천되지 않는가?</b></summary>
<div>
  FetchType.EAGER은 연관 엔티티를 무조건 함께 조회하므로 N+1 문제를 피할 수 있지만, 모든 상황에서 불필요한 데이터를 가져올 수 있어 성능 저하를 일으킬 수 있습니다.
또한 EAGER 설정은 JPA가 예기치 않게 조인을 수행하게 만들어, 복잡한 쿼리 구조나 순환 참조 문제를 유발할 수 있으므로 명시적인 Fetch Join이나 EntityGraph 방식이 더 권장됩니다.
</div>
</details>

<details>
<summary><b>@EntityGraph(attributePaths = [...])
 어노테이션을 사용하는 이유는 무엇이며, 실제로 어떤 방식으로 쿼리가 실행되는가?</b></summary>
<div>
 @EntityGraph는 지연 로딩으로 설정된 연관 엔티티를 특정 메서드에서 즉시 로딩(EAGER) 되도록 지정할 수 있는 방법입니다.

이 어노테이션을 사용하면, 내부적으로 LEFT OUTER JOIN FETCH 쿼리를 사용하여 연관 데이터를 한 번에 가져오기 때문에 N+1 문제를 방지할 수 있습니다.
</div>
</details>

<details>
<summary><b>Lazy 로딩 시 @BatchSize 를 사용하는 전략은 어떤 방식으로 쿼리 최적화를 이루며, 설정 시 주의할 점은 무엇인가?</b></summary>
<div>
 @BatchSize는 LAZY 로딩을 사용할 때 연관된 엔티티들을 프록시 객체로 미리 모아 한 번의 IN 쿼리로 조회하도록 하는 설정입니다.

예를 들어 BatchSize가 100이면, 100개의 연관 엔티티를 한 번에 조회할 수 있어 쿼리 수를 줄이고 성능을 향상시킵니다.

주의할 점은 DB의 IN 절 제한(보통 1000개 이하)을 고려해 적절한 크기를 설정해야 하며, 지나치게 큰 값은 오히려 쿼리 성능을 떨어뜨릴 수 있습니다.
</div>
</details>

<details>
<summary><b>Fetch Join과 EntityGraph의 공통점과 차이점은 무엇인가?</b></summary>
<div>
 공통점은 둘 다 연관 엔티티를 한 번의 쿼리로 함께 조회할 수 있도록 하여 N+1 문제를 해결한다는 점입니다.

Fetch Join은 JPQL에서 직접 사용하며, 쿼리 작성**이 필요하다. 반면, EntityGraph는 리포지토리 메서드에 어노테이션으로 적용할 수 있어 JPQL 없이도 조인을 수행할 수 있다.
</div>
</details>

## 변하영
<details>
<summary><b>N+1 문제란 무엇인가요?</b></summary>
<div>
 N+1 문제는 연관된 엔티티를 조회할 때, 먼저 1번의 쿼리로 부모 엔티티를 조회하고, 각 부모마다 자식 엔티티를 조회하는 쿼리가 N번 추가로 발생하는 문제입니다. 총 N+1번의 쿼리가 실행되며 성능 저하의 원인이 됩니다. 
</div>
</details>

<details>
<summary><b>FetchJoin이란 무엇인가요?</b></summary>
<div>
 FetchJoin은 JPQL에서 연관된 엔티티를 조인과 동시에 함께 로딩하도록 명시하는 방법입니다. LAZY 로딩 상태에서도 연관 데이터를 한 번의 쿼리로 가져올 수 있어 N+1 문제를 해결할 수 있습니다. 
</div>
</details>

<details>
<summary><b>@OneToMany 관계에서 EAGER를 사용하면 왜 문제가 될 수 있나요?</b></summary>
<div>
 EAGER은 연관된 데이털르 무조건 로딩하기 때문에 Many 쪽 엔티티가 많을 경우 조인 결과가 과도하게 커지고 성능 저하나 메모리 문제가 발생할 수 있습니다. 
</div>
</details>

<details>
<summary><b>FetchType.LAZY와 FetchType.EAGER의 차이는 무엇인가요?</b></summary>
<div>
 LAZY는 연관된 엔티티를 실제로 사용할 때 쿼리를 실행하는 방식이고, EAGER는 연관된 엔티티를 즉시 함께 로딩하는 방식입니다. LAZY는 불필요한 데이터 로딩을 막을 수 있어 더 자주 사용됩니다. 
</div>
</details>

<details>
<summary><b>BatchSize란 무엇인가요?</b></summary>
<div>
 연관된 프록시 객체를 한 번에 모아 where in 쿼리로 묶어 조회하게 해줍니다. where post_id in (…) 형태로 한 번에 가져와 쿼리 수를 줄일 수 있습니다. 전역 설정이나 개별 엔티티에 적용 가능하고 페이징과도 호환된다는 장점이 있습니다. 
</div>
</details>

<details>
<summary><b>EntityGraph란 무엇인가요?</b></summary>
<div>
 EntityGraph는 JPA에서 연관 엔티티를 **선언적으로 fetch join처럼 미리 로딩**할 수 있도록 도와주는 기능입니다. JPQL 쿼리문을 수정하지 않고도, 특정 메서드에서 어떤 연관 엔티티를 함께 로딩할지 지정할 수 있어 **코드가 간결하고 재사용이 쉽습니다.** 내부적으로는 Outer Left Join을 통해 연관 데이터를 한 번에 조회하며, 단순한 조회나 공통 API에 자주 활용됩니다.
</div>
</details>

## 김수훈
<details>
<summary><b>N+1 문제란 무엇인지 설명하고 예시를 들어주세요. </b></summary>
<div>
 N+1 문제란, 연관된 엔티티를 조회할 때 발생하는 성능 문제로, 하나의 엔티티를 조회하는 쿼리 1번과 연관된 엔티티를 N번 조회하면서 총 N+1번의 쿼리가 발생하는 현상을 말합니다.

예를 들어, 게시글 엔티티와 댓글 엔티티가 OneToMany 관계일 때, 게시글을 20개 조회하면 댓글 조회를 위한 쿼리가 20번 추가적으로 실행되어 총 21번의 쿼리가 실행됩니다. 이로 인해 DB 부하가 증가하고, 성능 저하가 발생할 수 있습니다.
</div>
</details>

<details>
<summary><b>N+1 문제를 해결하기 위해 FetchType.EAGER를 사용해서 해결하면 안되나요?</b></summary>
<div>
FetchType.EAGER는 모든 연관 데이터를 즉시 가져오므로 성능 저하나 메모리 사용 증가로 이어질 수 있기 때문에 사용하는 방식은 선호되지 않습니다.
</div>
</details>  

<details>
<summary><b>BatchSize는 무엇이고, 어떻게 작동하는지 간단한 예시와 함께 설명해주세요.</b></summary>
<div>
BatchSize는 Lazy Loading 시 연관 엔티티를 in 절로 묶어서 한 번에 가져올 수 있도록 도와주는 기능입니다.

예를 들어 10개의 Post를 조회했고 각 Post가 Comment와 연관되어 있을 경우, 기본 Lazy 전략은 Comment를 조회할 때 10번의 쿼리를 날리게 됩니다.

하지만 @BatchSize(size = 100) 등을 적용하면 이 Comment들을 하나의 in 절로 묶어서 한 번에 가져오기 때문에 쿼리 횟수를 줄일 수 있습니다.
</div>
</details>  

<details>
<summary><b>Fetch Join과 즉시로딩의 차이가 무엇인가요?</b></summary>
<div>
즉시로딩은 JPA의 엔티티 매핑 설정에서 사용하는 전략입니다.

즉, 엔티티를 조회할 때 연관된 엔티티도 자동으로 함께 조회하도록 기본 동작을 지정하는 것입니다.

반면에 Fetch Join은 JPQL에서 사용하는 전략입니다.

특정 쿼리에서만 일시적으로 연관 엔티티를 조인해서 한 번에 가져오고 싶을 때 사용합니다.

따라서 즉시로딩은 해당 엔티티를 조회할 때 항상  로딩되고, Fetch Join은 특정 쿼리에서만 명시적으로 조인합니다.</div>
</details>  

<details>
<summary><b>Entitygraph와 Fetch Join의 차이가 무엇이며 어떻게 사용하는지, 어떤 장단점이 있는지 설명해주세요</b></summary>
<div>
  EntityGraph는 Spring Data JPA에서 제공하는 기능으로, Repository 메서드에 @EntityGraph를 붙이면 연관된 엔티티를 Left Outer Join 방식으로 즉시 로딩 해줍니다.

JPQL을 작성하지 않아도 되고, 페이징이 가능하다는 장점이 있지만 쿼리 제어가 어렵다는 단점이 있습니다.

반면에 Fetch Join은 JPQL에서 직접 Join Fetch를 써서 연관된 엔티티를 한 번에 조인해서 가져오는 방식입니다.

쿼리를 직접 제어할 수 있어서 복잡한 조건이나 여러 조인이 필요한 상황에 유리하지만, 결과 중복 때문에 페이징이 불가능하다는 단점이 있습니다.
</div>
</details>  
