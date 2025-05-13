## 김수훈

<details>
<summary><b>Redis는 무엇의 약자일까요?</b></summary>
<div>

Remote Dictionary Server

</div>
</details>

<details>
<summary><b>Redis가 빠른 이유는 무엇인가요?</b></summary>
<div>

Redis는 모든 데이터를 메모리에 저장하고 처리하기 때문에 디스크 접근 지연이 없고,  
싱글 스레드 기반으로 동작해서 context switching 비용이 발생하지 않기 때문에 속도가 빠릅니다.

</div>
</details>

<details>
<summary><b>Redis는 휘발성인데, 데이터는 어떻게 보존하나요?</b></summary>
<div>

기본적으로는 메모리에 저장되기 때문에 휘발성이 있지만, Redis는 두 가지 영속성 옵션을 제공합니다.  
첫 번째는 RDB 방식으로, 주기적으로 스냅샷을 찍어 디스크에 저장하는 방식이고,  
두 번째는 AOF 방식으로, 모든 write 연산을 로그 파일로 기록해 복구하는 방법이 있습니다.

</div>
</details>

<details>
<summary><b>Redis에서 O(N) 명령어를 조심해야 하는 이유는 무엇인가요?</b></summary>
<div>

Redis는 싱글 스레드로 동작하기 때문에, O(N)처럼 오래 걸리는 명령어는 전체 서버를 블로킹시킬 수 있기 때문입니다.

</div>
</details>

<details>
<summary><b>DEL과 UNLINK의 차이는 무엇인가요?</b></summary>
<div>

DEL은 키를 즉시 삭제하는 명령어로, 삭제하는 데이터가 많으면 blocking이 발생할 수 있습니다.  
반면 UNLINK는 삭제 작업을 비동기로 처리해서 Redis의 메인 쓰레드가 영향을 받지 않도록 해줍니다.

</div>
</details>

<details>
<summary><b>Redis의 고가용성을 위한 아키텍처에는 어떤 것들이 있나요?</b></summary>
<div>

세 가지 정도로 정리할 수 있습니다.  

첫째, Replication은 Master-Slave 구조로 읽기 성능을 분산시켜주고, 장애 시 Slave를 통해 데이터를 보존할 수 있습니다.  
둘째, Sentinel은 장애 발생 시 자동으로 새로운 Master를 선출하고 클라이언트를 리디렉션해주는 역할을 합니다.  
마지막으로 Cluster는 데이터를 여러 노드에 분산 저장하여 수평 확장성과 고가용성을 동시에 제공합니다.

</div>
</details>

---

## 이소원

<details>
<summary><b>Redis는 메모리 기반의 데이터베이스로 휘발성이란 특징 있습니다. 데이터 손실을 해결하기 위해  Redis는 어떤 기능을 제공하나요?</b></summary>
<div>

AOF 로그 파일을 사용하여 모든 쓰기 연산을 AOF 파일에 기록하는 방식을 사용합니다.

</div>
</details>

<details>
<summary><b>왜 Redis에서 key 명령어를 사용하는 것을 지양하나요?</b></summary>
<div>

Redis는 싱글 스레드 서버이기 때문에 O(N) 시간복잡도 명령어를 사용하지 않아야 합니다.  
따라서 O(N)의 시간복잡도를 갖는 key 명령어 보다는 scan 명령어로 대체하여 사용하는 것이 권장됩니다.

</div>
</details>

<details>
<summary><b>왜 Redis를 사용할까요?</b></summary>
<div>

Redis는 Collection을 제공함으로, 메모리에서 바로 가져와 데이터를 사용할 수 있기 때문에 디스크에 직접 접근하지 않아도 되고,  
정렬된 상태의 자료구조를 지원하기 때문에 데이터를 가져와서 사용자에게 보여줄 수 있습니다.  
Redis는 Atomic한 자료구조를 가지고 싱글 스레드로 동작하기 때문에 race condition을 방지할 수 있습니다.

</div>
</details>

<details>
<summary><b>Redis 활용 사례를 설명해주세요</b></summary>
<div>

- I/O가 빈번히 발생하는 데이터  
- 세션 캐싱 및 인증 토큰등 저장  
- 전체 페이지 캐시  
- 메시지 대기열 어플리케이션  
- 랭킹 보드  
- 좋아요 같은 빠른 데이터 처리를 요하는 시스템  

</div>
</details>

<details>
<summary><b>고가용성을 보장하기 위해 Redis는 어떤 아키텍쳐를 사용하나요?</b></summary>
<div>

Replication, Sentinel, Cluster  

- Replication  
  - Master-Slave 분리 : Slave 서버는 Master 서버의 데이터 복제  
  - Master 는 쓰기 작업을 처리하고 Slave 는 읽기 작업을 분산함으로써 데이터 가용성을 높였습니다.  

</div>
</details>

---

## 변하영

<details>
<summary><b>Redis Sentinel의 역할은 무엇인가요?</b></summary>
<div>

Redis Sentinel은 마스터 노드를 모니터링하고, 장애 발생 시 자동으로 슬레이브를 새로운 마스터로 승격시켜 Failover를 수행합니다.

</div>
</details>

<details>
<summary><b>Replication 방식의 Redis 구조에서 Master와 Slave의 역할은 무엇인가요?</b></summary>
<div>

Master는 쓰기 작업을 담당하고, Slave는 Master의 데이터를 복제하여 주로 읽기 작업을 처리해 부하를 분산시킵니다.

</div>
</details>

<details>
<summary><b>DEL 명령어를 잘 사용하지 않는 이유는 무엇인가요?</b></summary>
<div>

DEL은 동기식 삭제로 오래 걸릴 수 있는 반면, UNLINK는 비동기로 백그라운드에서 삭제하므로 성능 저하를 줄일 수 있습니다.

</div>
</details>

<details>
<summary><b>Memcached 대신 Redis를 사용하는 이유는 무엇인가요?</b></summary>
<div>

Memcached는 LRU만 지원하는 반면, Redis는 LRU, LFU, TTL 기반 삭제 등 다양한 정책을 제공해 메모리 관리에 유리합니다.

</div>
</details>

<details>
<summary><b>Redis 사용 시 주의해야 할 O(N) 명령어는 어떤 것이 있으며, 대안은 무엇인가요?</b></summary>
<div>

KEYS, FLUSHALL, FLUSHDB 등은 O(N) 명령어로 사용 시 서비스 성능에 악영향을 줄 수 있습니다.  
SCAN 명령어를 사용해 대체하는 것이 좋습니다.

</div>
</details>

---

## 신예지

<details>
<summary><b>AOF란 무엇인가요?</b></summary>
<div>

모든 쓰기 연산을 로그로 저장하는 방식을 말합니다.

</div>
</details>

<details>
<summary><b>Redis는 어떻게 영속성을 보장하나요?</b></summary>
<div>

RDB(Redis DataBase) 스냅샷과 AOF(Append-Only File) 로그 파일을 통해 영속성을 보장합니다.

</div>
</details>

<details>
<summary><b>Redis를 사용할 때 사용하지 말아야 할 명령어를 말해주세요</b></summary>
<div>

keys, flushAll, flushDB, delete collections, get all collections

</div>
</details>

<details>
<summary><b>key-value 구조의 장점은 무엇인가요?</b></summary>
<div>

데이터의 고속 읽기와 쓰기에 최적화 돼있습니다.

</div>
</details>

<details>
<summary><b>PUB/SUB은 주로 어디에 사용되나요?</b></summary>
<div>

실시간 채팅, 실시간 스트리밍, 실시간 알림, SNS 피드 등에서 사용됩니다.

</div>
</details>
