## 이소원
<details>
  <summary><b>뮤텍스와 세마포어의 차이를 설명해주세요</b></summary>
  <div>
    <br>
    뮤텍스는 하나의 스레드만 자원에 접근할 수 있도록 락(Lock)을 걸어 독점적으로 자원을 소유하여 사용하는 방식이고,  세마포어는 카운터를 통해 여러 스레드가 제한된 수의 자원에 접근할 수 있도록 관리하는 방식입니다. 
    뮤텍스는 보통 단일 자원을 보호하며, 세마포어는 자원 자체에 직접 연결되기보다는 사용 가능한 자원의 개수(count)를 추적하는 형태로 간접적으로 제어합니다.  
  </div>
</details><br>

<details>
  <summary><b>모니터는 어떻게 뮤텍스와 세마포어의 문제점을 해결했나요?</b></summary>
  <div>
    <br>
    뮤텍스와 세마포어에서는 lock을 획득하고 해제하는 연산인 wait과 signal 연산의 순서가 뒤바뀔 수 있다는 문제점이 있습니다.  
    이 문제점을 해결하기 위해 모니터는 프로그래밍 언어에서 지원하는 추상 데이터 타입을 이용하여 타이밍 문제를 해결하였습니다.
  </div>
</details><br>

<details>
  <summary><b>Spin lock의 장단점은 무엇인가요? </b></summary>
  <div>
    <br>
    진입 코드를 계속 반복 수행하기 때문에 Lock 이 획득되자마자 바로 실행된다는 장점이 있지만, CPU 자원을 낭비한다는 단점이 있습니다.  
  </div>
</details><br>

<details>
  <summary><b>모니터의 구성요소는 뭔가요?</summary>
  <div>
    <br>
    모니터의 구성요소에는 monitor lock, 조건 변수, 조건 변수의 주요 동작이 있습니다.  
    모니터락은 임계 영역에서 상호 배제를 보장하고, 조건 변수는 동기화 도구이고, 조건 변수의 주요 동작으로는 wait, signal, broadcast가 있으며 스레드의 상태를 전환합니다.
  </div>
</details><br>

<details>
  <summary><b>세마포어를 사용할 때 발생할 수 있는 문제점은 뭐가 있나요?</summary>
  <div>
    <br>
    세마포어를 사용할 때 발생할 수 있는 주요 문제점은 데드락과 스레드의 무한 대기입니다.  
    카운팅 세마포어의 경우 모든 자원이 사용 중일 때 새로운 스레드가 계속해서 대기해야 하므로 무한 대기 상태가 발생할 수 있습니다.  
  </div>
</details><br>


## 신예지
<details>
  <summary><b>Mutex의 과정을 설명해주세요</b></summary>
  <div>
    <br>
    1. A 프로세스가 자원을 접근하기 위해 Key를 점유<br>
    2. A 프로세스가 키를 점유했기 때문에 공유 자원을 사용함<br>
    3. B 프로세스가 공유 자원을 사용하기를 원해서 Key를 점유하기 위해 대기함<br>  
    4. A 프로세스가 공유 자원을 다 사용하고 Key를 반환<br>  
    5. B 프로세스는 대기하고 있다가 반환된 Key를 점유하고 공유자원을 사용함<br>  
  </div>
</details><br>

<details>
  <summary><b>모니터는 어떻게 뮤텍스와 세마포어의 문제점을 해결했나요?</b></summary>
  <div>
    <br>
    뮤텍스와 세마포어에서는 lock을 획득하고 해제하는 연산인 wait과 signal 연산의 순서가 뒤바뀔 수 있다는 문제점이 있습니다.  
    이 문제점을 해결하기 위해 모니터는 프로그래밍 언어에서 지원하는 추상 데이터 타입을 이용하여 타이밍 문제를 해결하였습니다.
  </div>
</details><br>

<details>
  <summary><b>busy waiting 문제란 무엇인가요?</b></summary>
  <div>
    <br>
    한 프로세스가 락을 보유하고 있을 때, 임계 구역(critical section)에 진입하려는 다른 프로세스가 계속 acquire()를 호출하며 대기하는 것을 의미합니다.
  </div>
</details><br>

<details>
  <summary><b>세마포어에서 S 변수를 음수로 초기화 하면 안되는 이유를 설명해주세요 (응용)</summary>
  <div>
    S가 음수라는건 blocked queue에 프로세스가 존재한다는 의미인데, 실제로는 아무 프로세스도 존재하지 않아서 signal() 연산을 할 때 오류가 발생합니다.
  </div>
</details><br>

<details>
  <summary><b>다음은 세마포어의 block-wakeup 방식입니다. 빈칸에 들어갈 알맞은 코드를 작성해주세요</summary>
  <div>
    <br>
      <pre><code>typedef struct {
    int value;            /* semaphore */
    struct process *list; /* process wait queue */
} semaphore;

    void wait(semaphore *S) {
          S->value--;
          if (S->value < 0) {
              add this process to S->list;
              block(); 
          }
      }
</code></pre>
  </div>
</details><br>

<details>
  <summary><b>모니터의 구성 요소를 말해주세요/b></summary>
  <div>
    <br>
    모니터는 공유 자원(lock), 조건 변수, 주요 동작(wait, signal, broadcast)로 구성되어 있습니다.
  </div>
</details><br>


## 변하영
<details>
  <summary><b>모니터에서 condition variable의 역할은 무엇인가요? /b></summary>
  <div>
    <br>
    condition variable은 특정 조건이 충족될 때까지 스레드를 대기 상태로 만들고, 조건이 만족되면 다시 실행되게 합니다. 이를 통해 busy waiting 없이 조건 동기화를 구현할 수 있어 CPU자원을 절약할 수 있습니다.  
  </div>
</details><br>

<details>
  <summary><b>busy-waiting 방식과 block-wakeup을 사용하는 경우를 각각 말해주세요</b></summary>
  <div>
    <br>
    critical section의 길이가 긴 경우 block-wakeup이 적당합니다. 반면에 critical section의 길이가 매우 짧은 경우에는 block-wakeup의 오버헤드가 busy-waiting보다 커질 수 있어 busy-waiting이 적합합니다. 
  </div>
</details><br>

<details>
  <summary><b>세마포어와 모니터는 어떤 차이가 있나요?</b></summary>
  <div>
    <br>
    세마포어는 시스템 호출 기반이고 타이밍 오류가 발생할 수 있습니다. 반면에 모니터는 프로그래밍 언어 수준에서 동기화를 추상화해서 타이밍 오류를 줄이고, lock과 condition 변수를 자동으로 관리해줍니다.
  </div>
</details><br>

<details>
  <summary><b>세마포어의 signal 연산은 반드시 자원을 사용한 프로세스가 수행해야 하나요? </summary>
  <div>
    <br>
    아니요. 세마포어는 소유권 개념이 없어서 signal은 누구나 호출할 수 있습니다. 
  </div>
</details><br>

<details>
  <summary><b>위의 답변으로 인해 어떤 문제가 발생할 수 있나요?</summary>
  <div>
    <br>
    이로 인해 리소스를 사용하지 않은 프로세스가 signal을 호출할 수 있고, 잘못된 자원 관리로 race condition이나 deadlock이 발생할 수 있습니다. 
  </div>
</details><br>


## 박상윤
<details>
  <summary><b>두 프로세스가 뮤텍스로 공유자원에 접근하는 순서를 말해주세요.</b></summary>
  <div>
    <br>
1. A프로세스가 자원 접근을 위해 Key를 점유합니다.<br>
2. A프로세스가 키를 점유했으므로, 공유 자원을 사용합니다.<br>
3. B프로세스가 공유 자원을 사용하기를 원해서 Key를 점유하기 위해 대기합니다.<br>
4. A프로세스가 공유 자원을 다 사용하고 Key를 반환합니다.<br>
5. B프로세스는 대기하고 있다가 반환된 Key를 점유하고 공유자원을 사용합니다.<br>
  </div>
</details><br>

<details>
  <summary><b>아래 코드에서 발생할 수 있는 문제는 무엇일까요?</b></summary>
  <div>
    <br>
    <pre><code>
  class 맞춰봐! {
    private volatile boolean isLocked = false;

    public void acquire() {
        while (isLocked) {
            //..?무슨 문제
        }
        isLocked = true;
    }

    public void release() {
        isLocked = false;
    }
}
    </code></pre>
    acquire() 메서드는 busy waiting을 하며 CPU를 계속 사용합니다. lock을 오랫도안 쥐고 있는 경우 전체 시스템 성능에 악영향을 끼칠 수 있습니다.
  </div>
</details><br>

<details>
  <summary><b>세마포어와 모니터는 어떤 차이가 있나요?</b></summary>
  <div>
    <br>
    세마포어는 시스템 호출 기반이고 타이밍 오류가 발생할 수 있습니다. 반면에 모니터는 프로그래밍 언어 수준에서 동기화를 추상화해서 타이밍 오류를 줄이고, lock과 condition 변수를 자동으로 관리해줍니다.
  </div>
</details><br>

<details>
  <summary><b>두 프로세스가 세마포어로 공유자원에 접근하는 순서를 말해주세요(허용치:2)</summary>
  <div>
    <br>
1. A 프로세스가 공유 자원에 접근, 허용치 1로 감소<br>
2. B 프로세스가 공유 자원에 접근, 허용치 0로 감소<br>
3. C 프로세스가 공유 자원에 접근, 허용치가 0이므로 대기<br>
4. B 프로세스가 공유 자원을 다 사용한 후, 허용치 1로 증가<br>
5. C 프로세스는 대기하다가 허용치가 1로 증가되어 공유 자원에 접근, 허용치는 다시 0으로 감소<br>
  </div>
</details><br>

<details>
  <summary><b>뮤텍스와 세마포어의 차이점에 대해 말하시오.</summary>
  <div>
    <br>
    뮤텍스는 동기화 대상이 오직 하나이며, 세마포어는 동기화 대상이 하나 이상일 때 사용합니다.<br>

세마포어는 자원의 소유가 불가능하지만, 뮤텍스는 자원 소유가 가능하며 소유주가 이에 대한 책임을 가집니다.<br>

뮤텍스는 뮤텍스를 소유하고 있는 사용자만이 임계 영역을 나갈때, 뮤텍스를 해제하지만, 세마포어는 세마포어를 소유하고 있지 않은 사용자도 세마포어를 해제할 수 있습니다.<br>

세마포어는 시스템 범위에 걸쳐있고 파일 시스템 상의 파일 형태로 존재하지만, 뮤텍스는 프로세스 범위를 가지며 프로세스가 종료될 때 자동으로 해제됩니다.<br>
  </div>
</details><br>

<details>
  <summary><b>모니터 작동방식을 말해주세요.</b></summary>
  <div>
    <br>
1. 스레드가 모니터에 진입하려고 할 때, 락 획득합니다.<br>
2. 다른 스레드가 이미 모니터의 락을 획득한 상태라면, 현재 스레드는 대기합니다.<br>
3. 대기 중인 스레드는 모니터 내부의 대기 큐에 저장합니다.<br>
4. 레디 큐에서는 스레드를 일시 중지시키고, 대기 중인 스레드들의 목록을 유지합니다.<br>
5. 모니터 내의 조건 변수를 사용하여 특정 조건이 충족될 때까지 프로세스를 대기시킬 수 있습니다.<br>
    - 이 경우, 프로세스는 조건 대기 큐(condition wait queue)에서 대기합니다.<br>
6. 스레드가 일시 중지된 상태에서 해당 조건이 충족되면, 모니터는 대기 중인 스레드를 깨우고 다시 실행될 수 있도록 합니다.<br>
7. 스레드가 모니터를 빠져나갈 때, 락을 반납합니다. 이때 다른 스레드들이 모니터에 진입할 수 있습니다.<br>
  </div>
</details><br>

## 김수훈
<details>
  <summary><b>뮤텍스에서 Busy Waiting 혹은 SpinLock의 장점은 무엇인가요? </b></summary>
  <div>
    <br>
    락이 곧 풀릴 경우, 곧바로 다음 작업을 실행할 수 있어서 속도가 빠릅니다.
  </div>
</details><br>

<details>
  <summary><b>뮤텍스에서 SpinLock은 언제 사용하는게 좋을까요?</b></summary>
  <div>
    <br>
    락이 짧은 시간 안에 해제될 것 같을 때 사용합니다.
  </div>
</details><br>

<details>
  <summary><b>세마포어를 구현할 때 필요한 함수와 변수의 종류를 말하고 각각의 역할을 설명하세요.</b></summary>
  <div>
    <br>
    세마포어를 구현하기 위해서는 두 개의 함수와 하나의 변수가 필요합니다.<br>

wait() 함수는 쓰레드가 임계구역에 들어가기 위해 호출하는 연산으로, 다른 쓰레드가 임계구역에서 작업중이면 대기합니다.<br>

signal() 함수는 쓰레드가 임계구역에 작업을 마치고 세마포어를 해제하는 연산이며

마지막으로 하나의 정수 변수가 필요한데 현재 공유 데이터에 접근 가능한 허용치 개수를 나타냅니다.<br>
  </div>
</details><br>

<details>
  <summary><b>모니터의 monitor lock에 대해서 설명해주세요</summary>
  <div>
    <br>
    임계영역에서 상호 배제를 보장하는 장치로, 모니터 내부의 공유 자원에 접근하려는 스레드가 반드시 획득해야 하는 락을 뜻합니다. 
  </div>
</details><br>

<details>
  <summary><b>뮤텍스와 세마포어가 뭔가요?</summary>
  <div>
    <br>
    뮤텍스와 세마포어는 모두 공유 자원에 대한 동시 접근을 제어하기 위한 동기화 도구입니다.<br>
뮤텍스는 한 번에 하나의 스레드만 자원에 접근할 수 있도록 제한하는 반면, 세마포어는 자원의 개수를 지정하여 여러 스레드가 동시에 접근할 수 있도록 조절하는 역할을 합니다.<br>
  </div>
</details><br>
