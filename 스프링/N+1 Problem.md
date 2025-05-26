## N+1 Problem

N+1 문제란, 연관관계가 설정된 엔티티를 조회할 때, 1개의 메인 쿼리 + 연관된 엔티티 조회를 위한 N개의 쿼리가 추가로 발생하는 문제입니다.  
예를 들어, Post 20개 조회시, 각 Post의 Comment를 조회하면 1(Post) + 20(Comment) = 총 21개의 쿼리가 실행되게 됩니다. 

## 발생하는 이유
JPA는 연관 엔티티를 지연 로딩(Lazy Loading) 으로 설정하는 것이 기본이고, 연관 객체는 프록시(Proxy) 로 주입되고, 실제 접근할 때 SQL이 실행되기 때문입니다.  

```java
@OneToMany(mappedBy = "post", fetch = FetchType.LAZY)
private List<Comment> comments = new ArrayList<>();
```
위 코드처럼 반복문 등으로 comments에 접근하는 순간 N개의 쿼리가 추가로 실행됩니다.  

## 문제가되는 이유
데이터가 많을수록 쿼리 수가 기하급수적으로 증가합니다.  
DB부하 증가, 응답 시간 지연, 트래픽 병목이 발생합니다.  

## 해결 방법

### 1. FetchType.EAGER 사용 지양

Post 조회 시 Comment까지 무조건 즉시 로딩이 되어, 제어가 어려워지고, 필요 없는 데이터까지 불러오게 됩니다.  
EAGER는 단건 조회에서만 조인 최적화가 가능하며, 다건에서는 N+1이 발생할 수 있습니다.  

### 2. Fetch Join 사용
```java
@Query("SELECT p FROM Post p JOIN FETCH p.comments")
List<Post> findAllPostFetchJoinComments();
```
한 번의 쿼리로 연관 엔티티까지 조회
단, 컬렉션 2개 이상은 Fetch Join 불가(카다시안 곱 발생)  

### 3. @BatchSize 사용
```java
@OneToMany(mappedBy = "post", fetch = FetchType.LAZY)
@BatchSize(size = 100)
private List<Comment> comments = new ArrayList<>();
```
Lazy Loading 유지하면서, 내부적으로 WHERE IN 쿼리로 일괄 조회  
전역 설정 방법 : spring.jpa.properties.hibernate.default_batch_fetch_size=100  

### 4. @EntityGraph 사용
```java
@EntityGraph(attributePaths = {"comments"})
List<Post> findAll();
```
별도 쿼리 없이 Left Outer Join으로 연관 엔티티까지 한 번에 조회  
명시적으로 특정 연관 필드만 Fetch 조인하고 싶을 때 유용  

### 5. QueryDSL/MyBatis
직접 쿼리를 정의하며 필요한 데이터만 조인 또는 서브쿼리로 조회 가능  
복잡한 동적 조건이나 다대다 관계 최적화에 강력함  
