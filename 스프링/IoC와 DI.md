# IoC와 DI
## 객체지향 설계와 스프링
### 객체지향 설계 원칙과 DI의 필요성
객체지향 설계 원칙 중 **OCP(Open-Closed Principle)**, **DIP(Dependency Inversion Principle)**를 지키기 위해서는
- 구현체에 의존하지 않고
- 인터페이스에 의존하면서도
- 객체 생성과 연결을 유연하게 처리해야 함

### 순수 자바로 직접 설계하면 발생하는 문제
```java
public class OrderService {
    private final MemberService memberService = new MemberServiceImpl();
}
```
- OCP 위반: 구현체(MemverServiceImpl)에 강하게 의존
- DIP 위반: 구체 클래스에 의존하고 있음

### 해결책 = 의존성 주입(DI)
- 객체 생성과 연결을 외부로 분리
- 객체 간의 결합도를 낮춰 유지보수성과 확장성을 확보

### 스프링의 역할
- 객체를 대신 생성하고 관리하는 **IoC 컨테이너** 제공
- 의존성을 자동으로 주입해주는 **DI 기능** 제공
- 다형성 극대화, 유연한 구조 구현 가능

## 스프링 컨테이너와 스프링 빈

![img1.png](IoC와DI_image/img1.png)

### 스프링 컨테이너란?
- 객체를 생성하고 관리하는 중앙 저장소이자 제어 주체
- 가장 많이 쓰는 구현체: ApplicationContext
- 컨테이너 내부에는 객체들이 **빈(Bean)**으로 저장되어 관리됨

### 스프링 빈이란?
- @Component, @Configuration, @Bean 등으로 등록된 객체
- 스프링이 직접 생성하고 관리하는 객체
- 등록된 빈은 getBean() 또는 @Autowired 등을 통해 주입됨

### 빈 등록 방식
스프링 프로젝트를 생성하면 기본적으로 Application 클래스가 존재한다.

클래스에는 `@SpringBootApplication`가 붙어있고, 해당 어노테이션의 메타 어노테이션으로 `@ComponentScan`이 붙어 `@Component`를 자동으로 컨테이너에 등록해준다. `@Configuration`의 경우 메타 어노테이션에 `@Component`가 붙어 있어서 마찬가지로 자동으로 컨테이너에 등록해준다.
- @Component: 자동 등록(컴포넌트 스캔 대상)
- @Bean: 수동 등록(설정 클래스 내에서 직접 등록)
- @Configuration: 설정 클래스 지정(내부적으로도 @Componen 포함)

### 빈 생명주기

## 스프링 빈 스코프
### 스코프란?
> 빈이 생성되고 유지되는 범위를 지정하는 설정

### 주요 스코프 종류
- singleton(기본값)
    - 컨테이너당 하나의 객체만 생성
    - 모든 요청에 재사용됨
- prototype
    - 요청 시마다 새로운 객체 생성
    생성 이후 생명주기는 스프링이 관리하지 않음
- request
    - HTTP 요청마다 별도 객체 생성
- session
    - HTTP 세션마다 객체를 유지
- application
    - 서블릿 컨텍스트 범위에서 객체 유지

## 의존성 주입 방법
### 생성자 주입

### Setter 주입

### 필드 주입