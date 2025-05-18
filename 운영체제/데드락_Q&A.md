## 신예지

<details>
  <summary><b>데드락 발생조건 4가지를 말하고, 각각이 무엇인지 설명해주세요</b></summary>
  <div>
    <br>
    - 상호배제(mutual exclusion): 자원은 한 번에 한 프로세스만 사용 가능<br>
    - 점유 대기(hold and wait): 최소한 하나의 자원을 점유하면서 다른 프로세스에 할당되어 사용하고 있는 자원을 추가로 점유하기 위해 대기<br>
    - 순환 대기(circular wait): 프로세스의 집합에서 순환 형태로 자원을 대기<br>
    - 비선점(no preemption): 다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 뺏을 수 없음
  </div>
</details><br>

<details>
  <summary><b>데드락의 예방 전략 중 점유대기를 부정하면 어떤 문제가 발생하나요?</b></summary>
  <div>
    <br>
    점유대기를 부정하면 프로세스 실행 전에 모든 자원을 할당하기 때문에 필요하지 않을 때에도 자원을 점유하고 있습니다.<br>
    따라서 자원 낭비와 무한 대기 현상이 발생할 수 있습니다.
  </div>
</details><br>

<details>
  <summary><b>safe state란 무엇인가요?</b></summary>
  <div>
    <br>
    safe state란 모든 프로세스가 정상 종료 가능한 상태를 말합니다.
  </div>
</details><br>

<details>
  <summary><b>은행원 알고리즘에서 어떻게 데드락을 회피하나요?</b></summary>
  <div>
    <br>
    프로세스가 자원을 요구할 때 자원을 할당한 후에도 safe state로 남아있게 되는지 사전에 검사하여 교착 상태 회피합니다.<br>
    safe state라면 자원을 할당하고, unsafe state면 다른 프로세스에서 자원을 해지할 때까지 대기합니다.
  </div>
</details><br>

<details>
  <summary><b>데드락 회피와 데드락 탐지의 차이점을 설명해주세요</b></summary>
  <div>
    <br>
    데드락 회피는 최악의 경우를 생각하는 반면 데드락 탐지는 최선의 경우를 생각합니다.<br>
    또 데드락 회피 전략은 데드락이 발생하지 않지만 데드락 탐지 전략은 데드락이 발생할 수 있습니다.
  </div>
</details><br>

<details>
  <summary><b>Resource Preemption 에서 Checkpoint-restart method는 왜 필요한가요?</b></summary>
  <div>
    <br>
    어떤 프로세스로부터 자원을 뺏어오면 자원을 뺏긴 프로세스는 나중에 safe state로 롤백을 해야하기 때문입니다.
  </div>
</details><br>

## 박상윤

<details>
  <summary><b>Wait free와 Lock free의 각각 장단점을 말해주세요.</b></summary>
  <div>
    <br>
    Wait free의 장점은 모든 스레드가 한정된 시간 내에 진행 가능하고,<br>
    단점은 구현이 복잡하고 어렵고, Lock-Free에 비해 더 많은 오버헤드 발생 가능합니다.<br>
    Lock free 장점은 비교적으로 쉬운 구현을 가지고, 하나 이상의 스레드가 진행 가능함을 보장합니다.<br>
    또한, 스레드 간 상호작용 없이 안전하게 공유 자원 사용이 가능합니다.<br>
    단점으로는 일부 스레드가 블로킹될 가능성이 있어 높은 지연시간 발생할 수 있습니다.
  </div>
</details><br>

<details>
  <summary><b>데드락이란 무엇인가요?</b></summary>
  <div>
    <br>
    프로세스가 발생 가능성이 없는 이벤트를 기다리는 경우, 무한히 다음 자원을 기다리게 되는 상태,<br>
    한정된 자원을 여러 곳에서 사용하려고 할 때 발생<br>
    두 개 이상의 프로세스나 스레드가 서로 자원을 얻지 못해서 다음 처리를 하지 못하는 상태입니다.
  </div>
</details><br>

<details>
  <summary><b>데드락 발생 조건 4가지를 말하시오.</b></summary>
  <div>
    <br>
    상호 배제, 점유 대기, 비선점, 순환 대기
  </div>
</details><br>

<details>
  <summary><b>데드락 처리하는 방법중 예방은 어떻게 하는 건가요?</b></summary>
  <div>
    <br>
    상호배제 부정, 여러 프로세스가 공유 자원 사용합니다.<br>
    점유대기 부정, 프로세스 실행전 모든 자원을 할당합니다.<br>
    비선점 부정, 자원 점유 중인 프로세스가 다른 자원을 요구할 때 가진 자원 반납합니다.<br>
    순환대기 부정, 자원에 고유번호 할당 후 순서대로 자원 요구합니다.
  </div>
</details><br>

<details>
  <summary><b>RAG에서 어떤 방법으로 데드락 여부를 판별하나요?</b></summary>
  <div>
    <br>
    Graph Reduction 방식으로 판별하며, edge를 하나씩 제거하여<br>
    모든 edge가 제거되면 데드락이 없고, 지울 수 없는 edge가 남아 있다면 데드락이 존재함을 판별합니다.
  </div>
</details><br>

## 변하영

<details>
  <summary><b>데드락 예방 방법에는 무엇이 있나요? 각 방법들에 대해 설명해주세요.</b></summary>
  <div>
    <br>
    상호배제 부정은 여러 프로세스가 공유 자원을 사용하는 방법으로, 현실적으로 불가능합니다.<br>
    점유대기 부정은 프로세스 실행전 모든 자원을 할당합니다. 이는 자원 낭비와 무한대기 현상이 발생할 수 있습니다.<br>
    비선점 부정은 자원 점유 중인 프로세스가 다른 자원을 요구할 때 가진 자원을 반납합니다. 이는 현실적으로 불가능합니다.<br>
    순환대기 부정은 자원에 고유번호 할당 후 순서대로 자원을 요구합니다. 이는 자원 낭비가 발생할 수 있습니다.
  </div>
</details><br>

<details>
  <summary><b>은행원 알고리즘이란 무엇인가요?</b></summary>
  <div>
    <br>
    은행원 알고리즘은 자원을 요청받았을 때, 자원을 줘도 시스템이 safe state로 유지되는지 미리 시뮬레이션해보고,<br>
    안전하면 자원을 할당하는 데드락 회피 기법입니다.
  </div>
</details><br>

<details>
  <summary><b>데드락 회피와 탐지의 차이점을 설명해주세요.</b></summary>
  <div>
    <br>
    데드락 회피는 데드락이 일어날 경우를 고려하여 회피하는 것으로 데드락이 발생하지 않습니다.<br>
    반면에 데드락 탐지는 현재 상태만 고려해 데드락이 발생하는가를 파악하는 것으로, 데드락이 발생할 수 있습니다.<br>
    이때 데드락 발생 시 복구 과정이 필요합니다.
  </div>
</details><br>

<details>
  <summary><b>복구 방법을 설명해주세요.</b></summary>
  <div>
    <br>
    복구 방법 중 ‘프로세스 종료’는 데드락 상태에 있는 프로세스를 강제 종료해 자원을 회수하는 방식입니다.<br>
    예를 들어, 덜 중요한 프로세스를 종료해서 데드락을 풀 수 있습니다.<br>
    자원 선점은 특정 프로세스에서 자원을 강제로 회수해서 데드락을 해결하는 방식입니다.<br>
    예를 들어, 자원이 적게 필요한 프로세스에서 선점하여 자원을 더 많이 필요로 하는 프로세스에 재할당할 수 있습니다.
  </div>
</details><br>

<details>
  <summary><b>Lock-free와 Wait-free의 차이를 간단히 설명해 주세요.</b></summary>
  <div>
    <br>
    Lock-free는 최소한 하나의 스레드는 계속 작업을 진행할 수 있고,<br>
    Wait-free는 모든 스레드가 제한 시간 내에 반드시 작업을 끝낼 수 있는 구조입니다.
  </div>
</details><br>

## 김수훈

<details>
  <summary><b>데드락이 무엇인가요?</b></summary>
  <div>
    <br>
    데드락은 여러 프로세스가 서로 자원을 점유한 채, 다른 프로세스의 자원을 기다리면서 무한 대기 상태에 빠지는 현상입니다.<br>
    이로 인해 아무 작업도 진행되지 않는 교착 상태가 됩니다.
  </div>
</details><br>

<details>
  <summary><b>데드락 처리 방식의 예방 중 현실적으로 불가능한 예방 방법 두가지가 무엇일까요?</b></summary>
  <div>
    <br>
    상호배제 부정, 비선점 부정입니다.
  </div>
</details><br>

<details>
  <summary><b>은행원 알고리즘은 무엇인가요?</b></summary>
  <div>
    <br>
    은행원 알고리즘은 자원 할당 전에 시뮬레이션을 돌려서 데드락이 발생하지 않을 경우에만 자원을 할당하는 방식입니다.<br>
    Safe state만 유지하도록 설계되어 있지만, 오버헤드가 크고 실용성은 떨어집니다.
  </div>
</details><br>

<details>
  <summary><b>예방과 회피 방식의 차이점은 무엇인가요?</b></summary>
  <div>
    <br>
    예방은 데드락 조건 자체를 미리 제거해서 발생을 차단하는 반면,<br>
    회피는 자원을 할당하기 전에 시스템이 안전한 상태인지 확인해서 위험하면 자원 할당을 보류합니다.
  </div>
</details><br>

<details>
  <summary><b>Lock-free와 Wait-free의 차이는?</b></summary>
  <div>
    <br>
    Lock-free는 항상 하나 이상의 스레드가 진행 가능한 상태를 보장하고,<br>
    Wait-free는 모든 스레드가 제한 시간 내에 작업을 완료할 수 있도록 보장합니다.<br>
    Wait-free가 더 강력하지만 구현이 복잡하고 비용이 큽니다.
  </div>
</details><br>

## 이소원

<details>
  <summary><b>데드락 발생 조건 4가지는 뭔가요?</b></summary>
  <div>
    <br>
    상호배제, 점유대기, 비선점, 순환형 대기
  </div>
</details><br>

<details>
  <summary><b>데드락 복구 방법 중 process terminate와 resource preemption 의 차이점을 설명해주세요.</b></summary>
  <div>
    <br>
    process terminate 는 우선 순위에 따라 데드락 상태에 있는 프로세스를 종료시키면서 데드락의 원인이 되는 프로세스를 찾는 방법이고,<br>
    resource preemption 는 점유하고 있는 프로세스의 자원을 회수하여 데드락을 회복하는 방법입니다.
  </div>
</details><br>

<details>
  <summary><b>데드락을 탐지하기 위해 사용하는 기법은?</b></summary>
  <div>
    <br>
    Resource Allocation Graph (RAG)
  </div>
</details><br>

<details>
  <summary><b>데드락 회피(Avoidance) 기법 중 은행원 알고리즘의 동작 원리와 왜 사용하지 않는지 설명해주세요.</b></summary>
  <div>
    <br>
    프로세스에 자원을 할당하기 전에 미리 결정된 모든 자원들의 최대한 할당량으로 시뮬레이션을 진행하여,<br>
    안정상태에 있을 수 있다는 것을 확인 후 자원을 할당하는 기법 입니다.<br>
    그러나 프로세스와 자원 수를 고정하고 필요한 최대 자원 수를 알고 있어야 한다는 가정이 필요하여 실용적이지 못합니다.
  </div>
</details><br>

<details>
  <summary><b>데드락 복구 중 자원 선점에서 롤백 방법 2가지를 설명해주세요</b></summary>
  <div>
    <br>
    - safe state로 롤백하고 다시 재실행<br>
    - 처음 상태로 되돌림, 프로세스 강제 종료 후, 가장 최근의 check point에서 재시작
  </div>
</details><br>
