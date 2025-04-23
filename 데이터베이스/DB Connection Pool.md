# DB Connection Pool

## Database Connection
- Connection : 애플리케이션과 데이터베이스의 연결
- 어플리케이션에서 데이터베이스에 접속하고 종료하는 일련의 과정을 의미

- 웹 어플리케이션과 데이터베이스는 서로 다른 시스템으로, 이를 연결하기 위해 데이터베이스 드라이버 사용
- 데이터베이스 연결의 생애 주기
  1. 데이터베이스 드라이버를 사용하여 데이터베이스 연결 열기
  2. 데이터를 읽고 쓰기 위한 TCP 소켓 열기
  3. TCP 소켓을 사용해 SQL 요청/응답 통신
  4. 데이터베이스 연결 닫기
  5. TCP 소켓 닫기 (JDBC가 소켓을 감싸는 구조이기 때문에 Connection을 닫으면 소켓도 함께 닫힘)
- TCP 기반 통신에서 매번 connection을 열고 닫는 (3-way handshake, 4-way handshake) 시간적인 비용 발생으로 서비스 성능이 저하된다.

효율적인 커넥션 관리는 성능과 안정성 측면에서 중요한 요인으로, 시스템의 많은 부하를 주는 커넥션을 여닫는 비용을 해결하기 위해 DBCP가 등장하였다.

<br>

## DBCP
### 개념
- DataBase Connection Pool의 약자로, DB와 connection을 맺고 있는 객체를 관리
- 웹 컨테이너가 실행되면서 DB와 미리 connection을 해놓은 객체를 pool에 저장
- 클라이언트 요청 시 connection을 빌려주고, 처리가 끝나면 해당 connection을 반납받아 pool에 저장 (재사용)

### 특징
- HTTP 요청에 따라 pool에서 connection 객체를 사용하고 반납
- 물리적인 데이터베이스 connection 부하를 줄이고, 연결을 관리
- pool에 미리 생성된 connection으로, 요청마다 connection을 생성하지 않아 연결 시간이 소비되지 않음
- connection을 계속 재사용하므로 생성되는 connection 수 제한적으로 설정
- DBCP 사용 시에도 교착상태(Deadlock)에 주의해야 한다.
  - 스레드가 서로의 DB Connection이 반납되기만을 무한정 대기하게 되며 발생
 
<br>

## DBCP 동작 방식 (HikariCP)
📌 STEP 1. Thread가 Connection을 요청하여 Connection Pool에 유휴 Connection을 찾아 반환한다.

![Image](https://github.com/user-attachments/assets/9d2f3388-d234-4794-8b8b-b0f8ca31149e)
- HikariCP의 경우, 이전에 사용한 connection 이 존재하는지 확인하여, 이를 우선 반환하는 특징이 있다.

📌 STEP 2. 가능한 Connection이 존재하지 않으면, HandOffQueue를 Polling하면서 다른 Thread의 Connection 반납을 기다린다.

![Image](https://github.com/user-attachments/assets/b6e5052b-a279-4602-8835-aeb2352c6ba7)
- 지정한 Timeout 시간까지 대기하다 시간이 만료되면 예외를 던진다. (polling 방식)

📌 STEP 3.
- 사용한 connection 이 반납되면 connection pool이 connection 사용 내역을 기록하고, HandOffQueue에 반납된 connection을 넣는다.
- 이를 통해 HandOffQueue를 Polling 하던 대기 Thread가 connection을 획득하고 작업을 이어나간다.

![Image](https://github.com/user-attachments/assets/6e6718ef-28fd-4c1d-9385-e9112d92485f)

<br>

### Connection Pool 크기
1. 크게 설정
- 성능 : 사용자 대기 시간 줄어듦
- 문제 : DB도 많은 connection을 처리하려고 더 맣은 스레드를 사용하게 되어 DB 성능 저하, 스레드가 사용하는 connection 외에 남은 connection은 메모리 공간만 차지
- 메모리 사용 : connection이 많으니까 많아진다. == 메모리 소모가 크다.

2. 작게 설정
- 성능 : 사용자가 기다리는 시간이 길어짐
- 메모리 사용 : connection이 적으니까 적어진다.
- 장점 : DB에 과한 부하가 가지 않음 (관리할 연결의 수가 적음)

<br>

## DBCP 프레임워크
종류
- Apache Commons DBCP
- Tomcat DBCP
  - Tomcat에서 내장되어 사용됨
  - Apache Commons DBCP 라이브러리를 바탕으로 만들어짐
- Oracle UCP
- HikariCP

### HikariCP
- 가벼운 용량으로 빠른 속도를 가지는 JDBC DataBase Connection Pool Framework
- spring-boot에 기본적으로 내장
- HikariCP는 바이트코드 수준까지 극단적으로 최적화되어있기 때문에 가장 많이 사용

#### 주요 DB 서버 파라미터 (HikariCP)
- max_connections
  - 클래아언트와 맺을 수 있는 최대 connection 수
  - 신규 서버 투입, DBCP Connection 수 늘릴 때 등 문제 없이 동작하기 위해서 적절한 값 설정
- wait_time
  - DB 서버에서 connection이 inactive 할 때, 다시 요청이 오기까지 얼마의 시간을 기다린 뒤에 close를 할 것인기 결정
  - 일정 시간을 기다린 후에도 요청이 오지 않을 경우 연결 해제
    - DBCP의 어떤 connection이 비정상적인 상태일 때, DB 서버는 이를 모르기 때문에 계속 대기하는 것을 방지하기 위함

<br>

## 관련 개념
1. Data Source
 - javax.sql.DataSource 인터페이스
 - 어플리케이션에서 DB 커넥션을 얻고 반환하는 역할을 수행.
 - 실제 커넥션 풀 생성, 관리 등의 세부 구현은 DataSource를 구현한 라이브러리(e.g. HikariCP, Apache DBCP 등)에 의해 처리된다.
 - 어플리케이션은 DataSource를 통해 DB 커넥션을 요청하고, 사용 후 반납만 하면 된다.

2. JDBC
 - 개념
   - JDBC(Java Database Connectivity):
   - 자바에서 DB와 연결하고 SQL을 실행하기 위한 표준 API
   - JDBC는 Connection, Statement, ResultSet 등의 인터페이스를 제공
 - 특징
   - 다양한 DBMS는 JDBC API를 사용할 수 있도록 자체 JDBC 드라이버를 제공
   - JDBC는 DBMS에 독립적인 방식으로 설계
   - 따라서 Oracle, MySQL 등 여러 DB에 대해 동일한 방식으로 프로그래밍 가능
 - 단점
   - 순수 JDBC 사용 시:
   - DB 요청할 때마다 → 드라이버 로딩 → 커넥션 생성 → SQL 실행 → 커넥션 종료
   - 매번 커넥션을 새로 생성/닫기 때문에 성능에 큰 부담이 생김
   - 이러한 비효율성을 해결하기 위해 Connection Pool(커넥션 풀) 사용 → 대표적인 구현이 DBCP, HikariCP 등
   - 실제 서비스에서는 성능 문제로 인해 순수 JDBC 방식은 거의 사용되지 않는다.

3. JNDI
- 개념
  - JNDI(Java Naming and Directory Interface) :
  - Java 어플리케이션에서 이름으로 리소스(ex. DataSource, EJB 등)에 접근할 수 있도록 해주는 API
  - 주로 WAS(Web Application Server)에서 설정된 자원을 Java 코드에서 쉽게 찾아서 사용하기 위해 사용
- 기능
  - 이름 기반 리소스 검색 (ex. "java:comp/env/jdbc/mydb")
  - 객체와 이름의 매핑
  - 분산 환경에서 객체 탐색 지원
- 특징 및 장점
  - JNDI를 통해 WAS에서 미리 설정한 DataSource(커넥션 풀)에 접근 가능
  - 커넥션 풀은 WAS에서 정적으로 관리되며, 애플리케이션에서는 공유된 객체를 반복 생성 없이 효율적으로 사용 가능
  - 확장성 향상 :
    - 서버 노드를 추가하거나 배포가 늘어나도, 동일한 JNDI 설정을 공유하면 된다.
  - JNDI를 활용하면 애플리케이션 코드 변경 없이 설정만으로 DB 연결 정보 교체 가능 (운영/개발 DB 전환 등)
