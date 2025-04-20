
## 이소원

<details>
<summary>Advice Annotation 5가지를 설명해주세요</summary>

- Before Advice (@Before) : 대상 객체의 **메소드 호출 전**에 공통 기능을 실행  
- Around Advice (@Around)  
    - 대상 객체의 **메소드 실행 전, 후 또는 Exception 발생 시점**에 공통 기능을 실행하는데 사용  
    - 다양한 시점에 원하는 기능을 삽입할 수 있기 때문에 Advice 중 가장 널리 사용  
- After Advice (@After)  
    - Exception 발생 여부에 상관없이 대상 객체의 **메소드 실행 후** 공통 기능을 실행  
    - try / catch / finally 의 finally 블록과 비슷  
- After Returning Advice (@AfterReturning)  
    - 대상 객체의 메소드가 **Exception 없이 정상적으로 실행된 이후**에 공통 기능을 실행  
- After Throwing Advice (@AfterThrowing)  
    - 대상 객체의 메소드를 실행하는 도중 **Exception이 발생한 경우** 공통 기능을 실행  

</details>

<details>
<summary>런타임 시점의 AOP 적용 방식의 특징을 설명해주세요 (3가지)</summary>

- 특별한 컴파일러나, 복잡한 옵션, 클래스 로더 조작기를 사용하지 않아도 스프링만 있으면 AOP를 적용할 수 있다.  
- 이미 main ( ) 가 실행 중이기 때문에 코드 조작이 어려움 (그래서 proxy를 통해 부가 기능을 적용)  
- 스프링빈에만 AOP를 적용할 수 있다.  

</details>

<details>
<summary>Dynamic Proxy를 사용한다는 것은 무슨 의미?</summary>

런타임에 `인터페이스 프록시 인스턴스` 또는 `클래스의 프록시 인스턴스` 또는 `리플렉션`을 이용한 클래스 자체를 만들어 사용하는 프로그래밍 기법을 사용한다는 의미  
이렇게 원하는 타겟 객체에 프록시를 적용시켜, 동적으로 프록시된 객체를 삽입하는 기술을 Runtime Weaving  

</details>

<details>
<summary>왜 JDK Dynamic Proxy 보다 CGLIB Proxy의 성능이 더 좋은가요?</summary>

CGlib은 바이트 코드로 조작하여 Proxy를 생성해주기 때문에 성능에 대한 부분이 JDK Dynamic Proxy보다 좋습니다.  

</details>

<details>
<summary>AOP와 프록시의 차이점을 설명해주세요</summary>

1. **Spring AOP는 '기능/개념'**

- AOP(관점 지향 프로그래밍)를 구현하기 위한 **프레임워크 레벨 기능**
- @Aspect, @Before, @Around 같은 걸 통해 부가기능(Advice)을 코드 밖으로 빼서 모듈화

2. **프록시는 '기술적 도구'**

- 실제 객체를 대신해 **중간에서 가로채는 대리 객체**
- Spring AOP도 결국 이 **프록시를 이용해서 Advice 실행** 순서를 조절

Spring AOP는 프록시를 내부적으로 사용합니다.

Spring AOP = 개념적 틀, 프록시 = 실제 동작 구현 방식

</details>

---

## 신예지

<details>
<summary>핵심 관심사와 공통 관심사의 차이를 설명해주세요</summary>

- `핵심 관심사(core concern)`: 비즈니스 로직을 구현하는 과정에서 비즈니스 로직이 처리하려는 목적 기능  
- `공통 관심사(cross-cutting concern)`: 각각의 모듈들의 주 목적 외에 필요한 부가적인 기능들(로깅, 동기화, 오류 검사 등)로, 공통적으로 사용되는 기능  

</details>

<details>
<summary>AOP의 장점을 설명해주세요</summary>

- 공통 관심 사항을 핵심 관심 사항으로부터 분리시켜 핵심 로직을 깔끔하게 유지할 수 있음  
- 각각의 모듈에 수정이 필요하면 다른 모듈의 수정 없이 해당 로직만 변경하면 됨  
- 공통 로직을 적용할 대상을 선택할 수 있음  

</details>

<details>
<summary>Spring AOP는 어떤 방식으로 AOP를 구현하나요?</summary>

Spring AOP는 런타임 시점에서 프록시(Proxy)를 사용하는 방식으로 AOP를 구현함  

</details>

<details>
<summary>JDK Dynamic Proxy와 CGLIB Proxy의 차이점은 무엇인가요?</summary>

- JDK Dynamic Proxy는 인터페이스 기반 프록시입니다. 실제 객체가 구현한 인터페이스를 기반으로 동적 프록시를 생성하며, InvocationHandler를 오버라이드하여 동작을 제어합니다.  
- CGLIB Proxy는 클래스 상속 기반입니다. 인터페이스가 없는 경우 사용하며, 바이트코드를 조작해서 프록시를 생성하고, MethodInterceptor를 통해 메서드 호출 전후로 부가기능을 삽입합니다.  

스프링에서는 인터페이스가 있으면 JDK Proxy를, 없으면 CGLIB Proxy를 자동으로 사용합니다.  

</details>

<details>
<summary>Spring AOP와 AspectJ의 차이점은 무엇인가요?</summary>

- Spring AOP는 스프링 컨테이너가 관리하는 빈에 대해서만 AOP를 적용하며, 프록시 기반의 런타임 Weaving 방식을 사용합니다.  
- 반면에 AspectJ는 자바 전체 객체에 AOP를 적용할 수 있고, 컴파일 시점 또는 클래스 로딩 시점에 부가기능을 삽입하는 정적 Weaving 방식을 사용합니다.  

</details>

<details>
<summary>AOP 적용 시점 3가지를 얘기해주세요</summary>

컴파일 시점, 클래스 로딩 시점, 런타임 시점  

</details>

---

## 박상윤

<details>
<summary>AOP의 장점을 말하시오.</summary>

- **공통 관심 사항을 핵심 관심 사항으로부터 분리시켜 핵심 로직을 깔끔하게 유지**할 수 있다.  
    - **코드의 가독성과 유지보수**성 등을 높일 수 있다.  
- 각각의 모듈에 수정이 필요하면 **다른 모듈의 수정 없이 해당 로직만 변경**하면 된다.  
- **공통 로직을 적용할 대상**을 선택할 수 있다.  

</details>

<details>
<summary>Advice에서 제공하는 어노테이션 5개 설명</summary>

- @Before  
    **대상 객체의** **메서드 호출 전**에 공통 기능 실행  

- @Around  
    **대상 객체의 메서드 실행 전, 후 또는 익셉션 발생 시점에 공통 기능을 실행**하는데 사용  
    다양한 시점에 원하는 기능을 삽입할 수 있으므로 Advice중 가장 널리 사용됨  

- @After  
    **익셉션 발생 여부에 상관없이** 대상 객체의 **메소드 실행 후** 공통 기능을 실행  
    try-catch-finally의 finally 블록과 비슷  

- @AfterReturning  
    대상 객체의 메소드가 **익셉션 없이 정상적으로 실행된 이후**에 공통 기능을 실행  

- @AfterThrowning  
    대상 객체의 메소드를 **실행하는 도중 익셉션이 발생한 경우에 공통 기능**을 실행  

</details>

<details>
<summary>AOP 적용 시점 3가지</summary>

1. 컴파일 시점 : .java파일을 컴파일 할 때, **.class 파일을 만드는 시점에 부가 기능을 넣어서 컴파일하는 방식**, AspectJ 컴파일러가 Aspect를 확인해서 해당 클래스가 적용 대상인지 먼저 확인하고, 적용 대상인 경우 부가 기능 로직을 적용, 모든 지점 적용 가능  
2. 클래스 로딩 시점 : **JVM 내부의 클래스로더에 .class 파일을 올리는 시점**에 **바이트 코드를 조작**해서 부가 기능 로직을 추가하는 방식, 모든 지점 적용 가능  
3. 런타임 시점(프록시 사용) : **컴파일이 끝나고 클래스 로더가 이미 다 올라가 자바가 실행된 다음에 동작하는 런타임 방식**, 이미 프록시를 통해 부가 기능을 적용하는 방식,  런타임 시점에 부가 기능을 적용하는 방식은 메서드의 실행 지점으로 제한된다, 스프링 빈에만 AOP 적용 가능  

</details>

<details>
<summary>기존 프록시의 문제점을 말하시오</summary>

부가적인 기능을 추가할 때마다 프록시를 새롭게 만들어야 한다는 단점이 존재한다.  

</details>

<details>
<summary>Dynamic Proxy란?</summary>

**런타임에 특정 인터페이스들을 구현하는 클래스 또는 인스턴스를 만드는 기술**  
**리플렉션**을 사용해 직접 구현할 수 있다.  

</details>

---

## 변하영

<details>
<summary>AOP에 대해 설명해주세요</summary>

AOP는 관점지향 프로그래밍으로, 비즈니스 로직과 공통 관심사를 분리해 코드를 모듈화하는 방식입니다. 이를 통해 핵심 로직을 깔끔하게 유지하고, 중복 코드와 유지보수 비용을 줄일 수 있습니다. Spring에서는 프록시 기반으로 런타임에 AOP를 적용합니다.  

</details>

<details>
<summary>@Aspect는 어떻게 동작하나요?</summary>

@Aspect는 AOP 설정 클래스임을 나타내는 어노테이션입니다. 내부에 @Before, @After, @Around 같은 Advice를 정의하고, Pointcut 표현식을 통해 어디에 적용할지 지정합니다. Spring은 이를 프록시로 감싸서 해당 메서드 실행 시 Advice를 자동으로 호출하게 합니다.

- @Aspect 사용 예시
```java
@Aspect
@Component
public class LoggingAspect {

    @Around("execution(* com.example.demo.OrderService.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();

        Object result = joinPoint.proceed(); // 실제 메서드 실행

        long end = System.currentTimeMillis();
        System.out.println(joinPoint.getSignature() + " 실행 시간: " + (end - start) + "ms");

        return result;
    }
}
```


cf) @Pointcut 메서드 사용 예시

![](https://github.com/user-attachments/files/19827586/spring_aop_interview.md)

```java
@Slf4j
@Aspect
@Component
public class LoggingAspect {

    // service 패키지 내부 전체에 대해
    @Pointcut("execution(* ureca.com.study.aop.service.*.*(..))")
    private void allService(){};

    @Around("allService(){}")
    public Object aroundLogging(ProceedingJoinPoint joinPoint) throws Throwable {
        log.info("@Around: 실행(전)");
        Object result = joinPoint.proceed();
        log.info("@Around: 실행(후)");
        return result;
    }

    @Before("allService(){}")
    public void beforeLogging() {
        log.info("@Before: 실행");
    }

    @After("allService(){}")
    public void afterLogging() {
        log.info("@After: 실행");
    }

    @AfterReturning("allService(){}")
    public void afterReturningLogging() {
        log.info("@AfterReturning: 실행");
    }

    @AfterThrowing ("allService(){}")
    public void afterThrowingLogging() {
        log.info("@AfterThrowing: 실행");
    }
}
```

</details>

<details>
<summary>JDK Dynamic Proxy와 CGLIB Proxy의 차이점은 무엇인가요?</summary>

JDK 프록시는 인터페이스 기반으로 프록시를 만들고, CGLIB은 클래스를 상속받아 프록시를 만듭니다. JDK는 인터페이스가 필수지만, CGLIB은 클래스 기반이라 인터페이스 없이도 사용 가능하고 성능도 더 우수합니다.

</details>

<details>
<summary>Advice 관련 annotation 5가지에 대해 설명해주세요</summary>

@Before는 대상 메서드 실행 이전에 실행되고, @After는 예외 여부와 상관없이 실행 후 동작합니다.  
@AfterReturning은 정상 종료 시 실행되고, @AfterThrowing은 예외 발생 시 실행됩니다.  
@Around는 메서드 전·후 전체를 감싸며 로깅, 트랜잭션 관리 등에 가장 널리 사용되는 어노테이션입니다.

</details>

<details>
<summary>Spring AOP와 AspectJ의 차이점은 무엇인가요?</summary>

Spring AOP는 프록시 기반으로 런타임에 AOP를 적용하고, 스프링 빈에만 제한됩니다. 반면 AspectJ는 컴파일 또는 클래스 로딩 시점에 AOP를 적용하고, 다양한 JoinPoint에 적용 가능해 더 강력하지만 복잡합니다.

</details>

---

## 김수훈

<details>
<summary>AOP가 무엇인가요?</summary>

AOP는 관점 지향 프로그래밍의 약자입니다.  
객체지향 프로그래밍 패러다임을 보완하는 기술로, 메소드나 객체의 기능을 핵심 관심사와 공통 관심사로 나누어 프로그래밍하는 것을 말합니다.

</details>

<details>
<summary>Spring AOP의 JoinPoint 지점은 어디이며, 그 이유는 무엇인가요?</summary>

JoinPoint 지점은 메서드 실행 지점입니다. 그 이유는 Spring AOP는 프록시 기반이기 때문입니다.

</details>

<details>
<summary>AOP의 Aspect에 대해서 설명해주세요</summary>

Aspect란 Advice와 Pointcut을 모듈화 한 것으로, 실제 동작 코드를 의미하는 Advice와 작성한 Advice가 실제로 적용되는 메소드인 Pointcut을 합친 개념입니다.  
Aspect는 로깅, 보안, 트랜잭션 등의 부가기능을 나타내는 공통 관심사에 대한 추상적인 명칭입니다.

</details>

<details>
<summary>Dynamic Proxy가 쓰이는 이유를 기존 프록시의 문제점과 연결지어서 간단하게 설명해주세요</summary>

기존 프록시의 경우 부가적인 기능을 추가할 때마다 프록시를 새롭게 만들어야 한다는 단점이 존재했습니다. 이를 해결하기 위해 런타임 시점에 클래스 정보를 이용하여 어노테이션, 메서드에 따라 동적으로 다른 메서드 동작을 실행하도록 프록시를 만드는 Dynamic Proxy를 사용합니다.

</details>

<details>
<summary>JDK Dynamic Proxy보다 CGLIB Proxy의 성능이 더 좋은 이유가 무엇인가요?</summary>

CGLIB은 바이트코드 레벨에서 직접 클래스를 생성하기 때문에, 리플렉션을 사용하는 JDK Proxy보다 성능이 더 좋습니다.

</details>
