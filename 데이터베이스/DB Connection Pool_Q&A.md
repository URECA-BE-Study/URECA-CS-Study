## 이소원

<details>
<summary>DBCP를 왜 써야 할까요? 그리고 사용할 때의 이점은 뭘까요?</summary>

DBCP는 DB 연결을 매번 새로 생성하고 닫는데 드는 비용을 줄이기 위해 사용됩니다.

데이터베이스와의 연결은 TCP 기반 통신으로, 초기 연결 시 3-way handshake, 인증과정, 세션 설정 등으로 시간과 리소스가 많이 소모됩니다.

그래서 DBCP를 사용하면 미리 일정 수의 커넥션을 생성해 pool에 보관하고 요청이 올 때마다 그 중 하나를 빌려서 사용하고, 사용 후 반납합니다.

이 방식은 재사용이 가능해 성능이 크게 향상되며, 커넥션 수를 제한하여 DB 과부하도 방지할 수 있습니다.

따라서 성능 최적화와 자원 관리 측면 모두에서 DBCP는 필수적인 구성요소라고 할 수 있습니다.

</details>

<details>
<summary>Connection Pool을 크게 설정했을 때의 장단점을 설명해주세요</summary>

장점 : 커넥션의 수가 많아 부족 현상이 덜하여 사용자 대기 시간이 줄어듦니다.

단점 : 커넥션의 수가 많으면 그만큼 많은 수의 스레드의 요청을 처리하게 되고 이로 인해 문맥 교환이 증가하여 DB 성능이 저하됩니다.

</details>

<details>
<summary>왜 spring-boot는 항상 HikariCP를 우선적으로 사용하나요?</summary>

Spring Boot 2.x부터 HikariCP가 성능과 안정성 면에서 우수한 커넥션 풀로 평가되기 때문에 기본적으로 사용됩니다.

HikariCP만 HandOffQueue 를 사용합니다.

Spring Boot는 spring-boot-starter-jdbc나 spring-boot-starter-data-jpa 의존성을 추가하면 자동으로 DataSource를 구성하는데, 이때 classpath에 HikariCP가 있으면 기본 구현체로 자동 설정됩니다.

이전에는 Apache DBCP나 Tomcat JDBC Pool도 사용되었지만, HikariCP가 더 나은 **Connection 획득 시간, 경량성, 안정성** 등을 보여주기 때문에 지금은 공식적으로도 권장되고 있습니다.

만약 HikariCP를 사용하지 않고 다른 풀을 사용하고 싶다면, spring.datasource.type 설정을 통해 명시적으로 변경할 수 있습니다.

• HikariCP는 바이트코드 수준까지 극단적으로 최적화되어있기에 가장 많이 사용됩니다.

</details>

<details>
<summary>어플리케이션 단에서 connection을 얻어오고 반납하는 작업 구현을 어떤 인터페이스를 통해 할 수 있나요?</summary>

Data Source

</details>

<details>
<summary>DBCP 교착상태는 언제 발생할까요?</summary>

DBCP에서의 교착상태는 모든 커넥션이 소진되고 반납되지 않아 새로운 요청들이 무한히 커넥션을 (스레드가) 기다리는 상태를 의미합니다.

커넥션 누수방지, 적절한 커넥션 풀 크기, 타임아웃 설정 등으로 예방할 수 있습니다.

</details>

---

## 박상윤

<details>
<summary>데이터베이스에 연결의 생애 주기에 대해서 설명해주세요.</summary>

1. 데이터베이스 드라이버를 사용해 데이터베이스 연결을 엽니다.  
2. 데이터를 읽고 쓰기 위한 TCP 소켓을 엽니다.  
3. TCP 소켓을 사용해서 데이터를 통신합니다.  
4. 데이터베이스 연결을 닫습니다.  
5. TCP 소켓을 닫습니다.

</details>

<details>
<summary>데이터베이스에서는 어떠한 통신 방법을 사용하고, 해당 통신 방법은 어떠한 단점이 존재할까요?</summary>

DB는 TCP기반 통신 방법을 사용하고, 매번 커넥션을 열고 닫을때 3-way-handshake와 4-way-handshake로 인해 시간적인 비용 발생으로 서비스 성능이 저하됩니다.

</details>

<details>
<summary>DB Connection Pool에서 교착상태가 발생할 수 있는데, 왜 발생하나요?</summary>

스레드가 서로의 DB Connection이 반납되기만을 무한정 대기하게 되며 발생합니다.

</details>

<details>
<summary>DB Connection 동작 방식 중 HikariCP 처리 방식 순서에 대해 말해주세요.</summary>

1. Thread가 Connection을 요청하여 Connection Pool에 유휴 Connection을 찾아 반환합니다.  
2. 가능한 Connection이 존재하지 않으면, **HandOffQueue를 Polling하면서 다른 Thread의 반납을 기다립니다.**  
3. 사용한 Connection이 반납되면, Connection Pool이 Connection 사용내역을 기록하고, HandOffQueue에 반납된 Connection을 넣습니다.  
4. HandQueue를 Polling하던 대기 Thread가 Connection을 획득하고 작업을 이어나갑니다.

</details>

<details>
<summary>Connection Pool을 늘리면 어느정도 성능이 향상되지만, 무조건 늘리는 것은 좋지 않습니다. 왜그럴까요?</summary>

커넥션 수가 많아지면, 운영체제는 각 쓰레드간 Context Switching을 자주 해야 하며, 이로 인해 CPU의 자원이 소모되어 오버헤드가 발생합니다.

</details>

<details>
<summary>HikariCP의 주요 파라미터 중 wait_timeout은 어떤 의미를 가질까요?</summary>

wait_timeout은 DB 서버에서 커넥션이 **유휴 상태**일 때, 얼마나 오랫동안 대기할지를 설정하는 파라미터입니다.

설정된 시간 동안 커넥션에서 아무 활동이 없는 경우, 해당 커넥션을 자동으로 종료시킵니다.

</details>

<details>
<summary>HikariCP 유래</summary>

히카리CP의 이름 유래는 크게 두 가지 측면에서 해석할 수 있습니다.

첫째, "히카리(光)"는 일본어로 "빛"을 의미하며, 이는 데이터베이스 커넥션 풀이 데이터베이스와 웹 애플리케이션 사이를 빛처럼 빠르게 연결해 준다는 의미에서 유래되었다고 볼 수 있습니다.

둘째, "Hikari"는 최소한의 필요한 코드만 사용하여 가볍고 효율적인 구현을 추구한다는 의미를 담고 있다고 합니다.

![image.png](attachment:8a983f71-f35e-4dc7-9edc-5648c982f4d7:image.png)

</details>

---

## 김수훈

<details>
<summary>DBCP가 등장하게 된 이유가 무엇인가요?</summary>

데이터베이스와 애플리케이션 사이의 데이터 통신에 TCP를 사용합니다.  
하지만 TCP기반 통신 시, 매번 connection을 열고닫는 시간적인 비용 발생으로 서비스 성능저하가 발생하고, 시스템의 많은 부하를 주는 해당 비용을 해결하기 위해 DBCP 등장했습니다.

</details>

<details>
<summary>Connection Pool을 크게 설정할 때와 작게 설정할 때 어떤 장단점이 있는지 설명해주세요.</summary>

Connection Pool을 크게 설정하면 스레드가 사용하는 connection 외에 남은 connection은 메모리 공간만 차지하기 때문에 메모리 소모가 큰 반면에 사용자 대기시간은 줄어듭니다.  
반대로 Connection Pool을 작게 설정하면 메모리 소모량은 적지만 사용자 대기시간이 늘어납니다.

</details>

<details>
<summary>JDBC가 뭔가요?</summary>

JDBC는 자바에서 데이터베이스에 연결하고 SQL을 실행할 수 있도록 도와주는 표준 API입니다.  
자바와 DB 간의 통신을 중계해주는 역할을 합니다.

</details>

<details>
<summary>Connection Pool을 많이 늘려도 성능적인 한계가 존재하는 이유가 무엇인가요?</summary>

Context Switching으로 인한 오버헤드가 더 많이 발생하기 때문입니다.

</details>

<details>
<summary>Spring Boot에 기본적으로 내장되어있는 DBCP 프레임워크의 종류는 무엇인가요?</summary>

HikariCP입니다.

</details>

---

## 신예지

<details>
<summary>데이터베이스 연결의 생애주기를 설명해주세요</summary>

먼저 데이터베이스 드라이버를 사용하여 데이터베이스 연결을 엽니다.  
다음으로 데이터를 읽고 쓰기 위한 TCP 소켓 열고, TCP 소켓을 사용해서 데이터 통신을 합니다.  
이후 데이터베이스 연결을 먼저 닫고 TCP 소켓을 닫습니다. (DB를 닫으면 TCP도 자동으로 닫힘)

</details>

<details>
<summary>DBCP(HikariCP)의 실행 방식을 설명해주세요</summary>

Thread가 Connection을 요청하여 Connection Pool에 유휴 Connection을 찾아 반환합니다.  
가능한 Connection이 존재하지 않으면, HandOffQueue를 Polling하면서 다른 Thread의 Connection 반납을 기다립니다.  
만약 이때 지정한 TimeOut 시간이 만료되면 예외를 던집니다.  
사용한 Connection이 반납되면 Connection Pool이 Connection 사용내역을 기록하고, HandOffQueue에 반납된 Connection을 넣습니다.  
이를 통해 HandOffQueue를 Polling하던 대기 Thread가 Connection을 획득하고 작업을 이어나간다.

</details>

<details>
<summary>Connection Pool의 크기를 작게 설정하면 메모리 소모는 (크고 / 작고), 사용자 대기 시간은 (길어집니다 / 줄어듭니다).</summary>

작고, 길어집니다

</details>

<details>
<summary>JNDI에 대해 설명해주세요</summary>

Java 애플리케이션에서 리소스, 객체 및 서비스에 대한 표준 네이밍 및 디렉터리 서비스에 접근할 수 있게 해주는 API 및 스펙입니다.

</details>

<details>
<summary>일반적인 JDBC 방식의 단점은 무엇인가요?</summary>

일반적인 JDBC는 Database Pool 방식을 사용하지 않고 DB에서 정보를 가져올때마다 Driver를 로드하고 커넥션을 열고 닫기 때문에 시간이 오래 걸리고 비효율적이라는 단점이 있습니다.

</details>

---

## 변하영

<details>
<summary>DBCP가 HTTP 요청 처리 성능에 어떤 영향을 줄까요?</summary>

DBCP은 매 요청마다 DB연결을 새로 만드는 오버헤드를 줄여줍니다.  
미리 생성해놓은 Connection을 재사용함으로써 요청 처리 속도가 빨라지고, 동시에 많은 요청이 들어와도 효율적으로 처리할 수 있습니다.

</details>

<details>
<summary>Connection Pool의 크기를 너무 크게 설정하면 어떤 문제가 발생할까요?</summary>

Connection이 많아질수록 DB 서버 입장에서는 더 많은 스레드를 관리해야 하므로 Context Switching 오버헤드가 커집니다.  
또한, 메모리 사용량도 늘어납니다.

</details>

<details>
<summary>HikariCP의 HandOffQueue의 역할은 무엇이며, 이로 인한 장점은 무엇인가요?</summary>

HandOffQueue는 커넥션이 부족할 때 다른 스레드가 반납하는 커넥션을 기다리는 큐입니다.  
이를 통해 커넥션을 효율적으로 재사용할 수 있습니다.

</details>

<details>
<summary>Deadlock이 DBCP에서 발생할 수 있는 이유와 방지 방법은 무엇인가요?</summary>

Deadlock은 서로 다른 스레드가 커넥션을 점유한 채 반납만을 기다릴 때 발생합니다.  
Timeout 설정을 통해 무한대기를 피할 수 있습니다.

</details>

<details>
<summary>JNDI를 활용한 DBCP 구성의 장점은 무엇인가요?</summary>

DB Pool을 WAS 수준에서 설정하고 static 객체처럼 관리할 수 있어 효율적입니다.  
설정을 외부화하여 여러 애플리케이션에서 공유도 가능하고 확장성도 뛰어납니다.

</details>

