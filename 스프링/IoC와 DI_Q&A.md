## 이소원

<details>
  <summary>스프링 빈 이벤트 라이프 사이클은 어떻게 되나요?</summary>
  <div>
    스프링 IoC 컨테이너 생성 → 빈 생성 → 의존관계 주입 → 초기화 콜백 메서드 호출 → 사용 → 소멸 전 콜백 메서드 호출 → 스프링 종료
  </div>
</details><br>

<details>
  <summary>왜 Bean 생명주기에서 콜백이 필요한가요?</summary>
  <div>
    의존관계 주입 이후 초기화 시점 보장과, 스프링 종료 직전 안전한 종료 작업을 위해 콜백이 필요합니다.
  </div>
</details><br>

<details>
  <summary>왜 프로토타입 스코프는 @PreDestroy와 같은 종료 콜백 메서드가 호출되지 않나요?</summary>
  <div>
    스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하며, 이후 생명주기를 관리하지 않기 때문입니다.
  </div>
</details><br>

<details>
  <summary>IoC가 뭔가요?</summary>
  <div>
    객체의 제어권을 스프링 컨테이너가 가지는 제어의 역전(Inversion of Control)을 의미합니다.
  </div>
</details><br>

<details>
  <summary>@PostConstruct와 @PreDestroy 어노테이션의 역할은 무엇인가요?</summary>
  <div>
    @PostConstruct는 빈 생성 후 의존관계 주입이 끝난 시점에 초기화 메서드를 실행하고, @PreDestroy는 빈 소멸 직전 정리 작업을 수행합니다.
  </div>
</details><br>

---

## 김수훈

<details>
  <summary>스프링 부트에서 스프링 빈이 등록되는 과정을 @SpringBootApplication와 @ComponentScan과 @Component를 사용하여 설명해주세요.</summary>
  <div>
    @SpringBootApplication은 @ComponentScan을 포함하고 있어, @Component가 붙은 클래스를 스캔해 스프링 빈으로 등록합니다. @Configuration도 @Component가 포함된 메타 어노테이션이므로 빈으로 등록됩니다.
  </div>
</details><br>

<details>
  <summary>Spring Container와 Spring Bean에 대해서 간단하게 설명해주세요.</summary>
  <div>
    Spring Container는 스프링에서 객체를 관리하는 공간이고, Spring Bean은 스프링에 의해 생성되고 관리되는 자바 객체입니다.
  </div>
</details><br>

<details>
  <summary>스프링 빈 스코프 중 싱글톤 빈의 범위를 설명해주세요.</summary>
  <div>
    싱글톤 빈은 컨테이너 생성 시 한 번만 생성되고, 종료될 때까지 같은 인스턴스를 공유합니다.
  </div>
</details><br>

<details>
  <summary>DI가 무엇인가요?</summary>
  <div>
    DI는 의존 객체를 개발자가 직접 생성하지 않고, 스프링 컨테이너가 대신 주입해주는 방식으로 결합도를 낮춥니다.
  </div>
</details><br>

<details>
  <summary>@Autowired는 어떤 역할을 하나요?</summary>
  <div>
    타입에 맞는 빈을 컨테이너에서 찾아 자동으로 주입해주는 어노테이션입니다.
  </div>
</details><br>

<details>
  <summary>@Autowired 쓰는 사람 보면 상윤이가 뒤통수 후린댔는데 그러면 생성자 주입은 어떻게 써야할까요?</summary>
  <div>
    생성자 주입은 `private final` 필드와 함께 `@RequiredArgsConstructor` 어노테이션을 사용해 구현합니다.
  </div>
</details><br>

---

## 신예지

<details>
  <summary>빈 생명주기를 설명해주세요</summary>
  <div>
    IoC 컨테이너 생성 → 빈 생성 → 의존관계 주입 → 초기화 콜백 호출 → 사용 → 소멸 콜백 호출 → 컨테이너 종료
  </div>
</details><br>

<details>
  <summary>빈 생명주기 콜백 메서드의 종류를 설명해주세요</summary>
  <div>
    1. InitializingBean, DisposableBean 구현<br>2. @Bean(initMethod, destroyMethod)<br>3. @PostConstruct, @PreDestroy (권장)
  </div>
</details><br>

<details>
  <summary>스프링 빈 스코프에서 싱글톤과 프로토타입의 차이점을 설명해주세요</summary>
  <div>
    싱글톤은 컨테이너 생성 시점부터 종료 시점까지 하나의 인스턴스를 재사용하며 소멸 콜백도 실행됩니다. 프로토타입은 요청마다 새로 생성되며, 이후 생명주기는 관리되지 않습니다.
  </div>
</details><br>

<details>
  <summary>의존관계 주입에서 Setter 주입과 필드 주입을 권장하지 않는 이유를 설명해주세요</summary>
  <div>
    Setter는 public 접근자로 인해 외부 변경 위험이 있고, 필드 주입은 테스트 어려움과 의존성 명확성 부족 문제가 있습니다.
  </div>
</details><br>

<details>
  <summary>SpringApplication의 run()을 실행했을 때의 빈 초기화까지의 과정을 설명해주세요</summary>
  <div>
    run() → ConfigurableApplicationContext 생성 → @ComponentScan으로 컴포넌트 탐색 → BeanDefinition 생성 → 빈 인스턴스 생성 및 의존성 주입 → @PostConstruct 등 콜백 호출
  </div>
</details><br>

---

## 박상윤

<details>
  <summary>Spring Bean의 생명주기에 대해 말해주세요.</summary>
  <div>
    컨테이너 생성 → 빈 생성 → 의존관계 주입 → 초기화 콜백 → 사용 → 소멸 콜백 → 컨테이너 종료
  </div>
</details><br>

<details>
  <summary>빈 생명 주기의 콜백함수가 왜 필요할까요?</summary>
  <div>
    의존관계 주입 완료 이후 안전하게 초기화 및 소멸 작업을 하기 위한 시점을 보장해줍니다.
  </div>
</details><br>

<details>
  <summary>싱글톤 스코프는 뭘까요?</summary>
  <div>
    컨테이너 내에서 단 하나의 인스턴스를 생성하고 전역적으로 공유하는 스코프입니다.
  </div>
</details><br>

<details>
  <summary>의존관계 주입이란 무엇인가요?</summary>
  <div>
    스프링이 객체를 생성하고 그 참조값을 전달하여 다른 객체와 연결해주는 과정입니다.
  </div>
</details><br>

<details>
  <summary>스프링 DI의 3가지 방식은 무엇일까요?</summary>
  <div>
    Setter Injection, Field Injection, Constructor Injection
  </div>
</details><br>

<details>
  <summary>스프링에서는 왜 생성자 주입을 가장 권장할까요?</summary>
  <div>
    final을 사용해 불변성을 보장하고, 순수 Java로 테스트가 가능하여 유지보수가 쉬운 구조를 만들 수 있습니다.
  </div>
</details><br>

---

## 변하영

<details>
  <summary>생성자 주입이 가장 권장되는 이유는 무엇인가요?</summary>
  <div>
    의존성을 final로 고정해 불변성을 보장하고, 테스트 코드 작성이 용이하기 때문입니다.
  </div>
</details><br>

<details>
  <summary>싱글톤 스코프와 프로토타입 스코프를 각각 언제 선택하나요?</summary>
  <div>
    상태를 공유해야 하면 싱글톤, 매번 새로운 인스턴스가 필요하면 프로토타입을 사용합니다.
  </div>
</details><br>

<details>
  <summary>빈 생명주기에서 의존관계 주입 이후 초기화 로직이 필요한 이유는 무엇인가요?</summary>
  <div>
    의존 객체가 완전히 주입된 후에야 초기화 작업을 안전하게 수행할 수 있기 때문입니다.
  </div>
</details><br>

<details>
  <summary>@ComponentScan은 어떤 역할을 하나요?</summary>
  <div>
    @Component가 붙은 클래스를 찾아 자동으로 스프링 빈으로 등록해주는 기능을 합니다.
  </div>
</details><br>

<details>
  <summary>스프링 빈 스코프에는 무엇이 있나요?</summary>
  <div>
    기본 싱글톤 외에 프로토타입, 웹 환경에서는 request, session, application, websocket 스코프가 있습니다.
  </div>
</details><br>
