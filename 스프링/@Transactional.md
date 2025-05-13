# @Transactional 

## 트랜잭션(Transaction)이란?

### 📌 개념
- DBMS 또는 유사 시스템에서 데이터의 상태를 변경하는 연산들의 집합으로, 하나의 논리적인 작업 단위
- 모든 작업이 성공해야 완료되며, 하나라도 실패 시 전체 작업을 취소해야 함

### 📌 필요성
- 시스템 중간에 장애나 예외가 발생해도 데이터 일관성을 보장하기 위해 필요
- 예: 은행 송금 중 잔액 차감은 되었지만 상대방 계좌에 입금이 되지 않은 경우 등

### 📌 특징 (ACID)

| 속성 | 설명 |
|------|------|
| **Atomicity (원자성)** | 모두 성공하거나 모두 실패해야 함 |
| **Consistency (일관성)** | 트랜잭션 전후 데이터 일관성 유지 |
| **Isolation (격리성)** | 트랜잭션 간 간섭 방지 |
| **Durability (지속성)** | 성공된 트랜잭션은 영구 반영 |

### 📌 트랜잭션 연산

- **Commit**: 트랜잭션 성공 후 변경사항을 확정
- **Rollback**: 트랜잭션 중 예외 발생 시 변경사항을 원상 복구

---

## 스프링에서의 트랜잭션 처리

### 0. 순수 JDBC 방식

```java
public class MemberService {
    public void updateAge(Long memberId) throws SQLException {
        Connection connection = dataSource.getConnection();
        try (connection) {
            connection.setAutoCommit(false); // 트랜잭션 시작
            Member member = memberRepository.findById(connection, memberId);
            memberRepository.update(connection, memberId, member.getAge() + 1);
            connection.commit(); // 성공 시 반영
        } catch (SQLException e) {
            connection.rollback(); // 실패 시 복원
            throw e;
        } finally {
            connection.close(); // 커넥션 반환
        }
    }
}
```

### 📌 설명
- JDBC 트랜잭션은 수동 커밋 방식으로 처리됨 (`setAutoCommit(false)`)
- 코드 흐름에 명시적으로 `commit()` 또는 `rollback()` 호출 필요
- 동일 트랜잭션 범위를 유지하려면 `Connection` 객체를 모든 DAO에 넘겨야 함

### 📌 단점
- 트랜잭션 처리를 개발자가 직접 관리해야 하므로 실수 가능성 증가
- 커넥션 객체의 의존성이 높아져 **서비스-DAO 결합도 증가**
- 기술 변경 시(JPA 등으로 변경) 서비스 계층까지 코드 수정 필요
- 반복적인 보일러플레이트 코드 (`try-catch-finally`)가 많아짐

---

### 1. 트랜잭션 동기화

```java
TransactionSynchronizationManager.initSynchronization();
Connection conn = DataSourceUtils.getConnection(dataSource);
conn.setAutoCommit(false);

try {
    // 비즈니스 로직
    conn.commit();
} catch (Exception e) {
    conn.rollback();
    throw e;
} finally {
    DataSourceUtils.releaseConnection(conn, dataSource);
    TransactionSynchronizationManager.clearSynchronization();
}
```

### 📌 설명
- `TransactionSynchronizationManager`를 통해 커넥션을 스레드 로컬에 저장하여 DAO 간 공유 가능
- 스프링에서 커넥션을 안전하게 관리하는 `DataSourceUtils`와 함께 사용
- 여전히 수동 코드 존재 → 완전한 분리는 아님

---

### 2. 트랜잭션 추상화

```java
TransactionStatus status = transactionManager.getTransaction(new DefaultTransactionDefinition());

try {
    // 비즈니스 로직
    transactionManager.commit(status);
} catch (Exception e) {
    transactionManager.rollback(status);
    throw e;
}
```

### 📌 설명
- Spring은 `PlatformTransactionManager`를 통해 다양한 트랜잭션 기술을 추상화함
- JDBC, JPA, Hibernate 등을 동일한 방식으로 다룰 수 있음
- 트랜잭션 코드가 여전히 서비스 로직에 남아있음

---

### 3. 선언적 트랜잭션 처리 (@Transactional)

```java
@Transactional
public void updateAge(Long memberId) {
    Member member = memberRepository.findById(memberId);
    memberRepository.update(memberId, member.getAge() + 1);
}
```

### 📌 설명
- AOP 기반 프록시가 메서드 호출 전후로 트랜잭션을 처리
- 핵심 로직만 작성하면 되고 트랜잭션 관련 코드는 사라짐
- 가장 권장되는 방식

---

## @Transactional 어노테이션

### 📌 개념
- 트랜잭션 경계를 명시적으로 어노테이션으로 선언
- Spring AOP에 의해 트랜잭션 시작/종료가 자동으로 제어됨

### 📌 사용
- Spring에서 ServiceLayer에 해당 어노테이션을 추가하여 트랜잭션 처리를 진행
- 서비스 클래스 혹은 메서드 단위에 어노테이션을 추가하여 사용
- @Transactional 어노테이션을 사용하라면 설정에 @EnableTransactionManagement를 추가해야 하는데, 스프링 부트에서는 자동으로 설정되어 있음
### 📌 속성

| 속성 | 설명 |
|------|------|
| `propagation` | 트랜잭션 전파 방식 설정 |
| `isolation` | 격리 수준 설정 |
| `rollbackFor` | 롤백할 예외 지정 |
| `noRollbackFor` | 롤백하지 않을 예외 지정 |
| `timeout` | 제한 시간 초과 시 롤백 |
| `readOnly` | 읽기 전용 트랜잭션 설정 |

---

## 🧷 isolation 격리 수준

| 수준 | 설명 |
|------|------|
| `READ_UNCOMMITTED` | Dirty Read 허용. 성능↑ 정합성↓ |
| `READ_COMMITTED` | 커밋된 데이터만 읽기 (Oracle, PostgreSQL 기본) |
| `REPEATABLE_READ` | 동일 트랜잭션 내 동일 쿼리 결과 보장 (MySQL 기본) |
| `SERIALIZABLE` | 완전 직렬화, 가장 엄격. 성능↓ |

---

## ➕ @Transactional(readOnly = true)

```java
@Transactional(readOnly = true)
public List<Member> getAllMembers() {
    return memberRepository.findAll();
}
```

### 📌 설명
- flush 생략됨 (쓰기 작업 없음)
- dirty checking, 스냅샷 저장 생략 → 성능 최적화
- 단순 조회 메서드에서 유용

---

## ⚙️ 동작 원리

- `@Transactional`이 붙은 클래스는 **프록시 객체**로 생성됨
- 메서드 실행 시 → 트랜잭션 시작 → 종료 시 commit/rollback
- 기본 동작:
  - `RuntimeException` 또는 `Error` 발생 시 → 자동 rollback
  - `CheckedException`은 rollback 안 됨 (옵션 필요)

---

## 🧪 테스트에서의 @Transactional

```java
@SpringBootTest
@Transactional
class MemberServiceTest {
    @Test
    void testServiceLogic() {
        // 테스트 완료 후 DB 자동 롤백
    }
}
```

### 📌 설명
- 테스트마다 트랜잭션이 시작되고 종료 시 자동 롤백됨
- 테스트 격리성 확보, 상태 보존 불필요 → 테스트 작성 효율 증가

---

## ✅ 요약

- `@Transactional`은 스프링이 제공하는 AOP 기반 선언형 트랜잭션 처리 방식
- 다양한 속성으로 세밀한 트랜잭션 제어 가능
- 실무에서는 readOnly, propagation, rollbackFor 등을 상황에 맞게 조합 사용
- 테스트 환경에서는 자동 롤백으로 DB 초기화 없이 반복 가능

## 참고 링크
- https://github.com/jmxx219/CS-Study/blob/main/spring/%40Transactional.md
