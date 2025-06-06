# Servlet Filter와 Spring Interceptor

## 공통 관심사항 (Cross-cutting Concern)

웹 애플리케이션을 개발하다 보면 여러 기능에 반복적으로 적용되는 공통 로직이 존재한다. 예를 들어, 게시글 등록, 수정, 삭제, 조회 등 모든 요청에 대해 로그인 여부를 확인해야 하는 상황을 떠올릴 수 있다. 이 경우 모든 컨트롤러에 인증 관련 코드를 반복 작성하게 되는데, 이는 중복 코드로 이어지고 유지보수가 어려워진다.

이런 문제를 해결하기 위해 스프링은 공통 로직을 분리해 처리할 수 있도록 다양한 기능을 제공한다. 대표적으로 다음과 같은 세 가지 방식이 있다:

- **Filter**: 서블릿 컨테이너 레벨에서 동작
- **Interceptor**: 스프링 MVC 컨텍스트에서 동작
- **AOP**: 메서드 단위로 관심사를 분리해 적용

이 중 HTTP 요청과 응답 흐름을 제어하는 데 가장 적합한 방식은 Filter와 Interceptor이며, 두 기능 모두 HttpServletRequest 객체를 통해 HTTP 관련 정보를 자유롭게 다룰 수 있다는 공통점을 가진다.

---

## 서블릿 필터 (Servlet Filter)

![image](https://github.com/user-attachments/assets/0a7c003a-082f-43fd-b4de-1c3a017c26ee)

### 개념 및 역할

Filter는 자바 서블릿 스펙에 정의된 표준 기능으로, 서블릿이나 JSP와 같은 웹 리소스에 접근하기 전에 요청을 가로채 전처리를 수행하거나, 응답을 가공하는 후처리를 수행할 수 있다.

DispatcherServlet보다 앞단에서 작동하며, 이는 스프링 프레임워크에 의존하지 않고 톰캣과 같은 웹 컨테이너(WAS)에 의해 관리된다는 것을 의미한다.

### 동작 흐름

- 필터 흐름
  - `HTTP 요청` ➙ `WAS` ➙ `필터` ➙ `서블릿(디스패처 서블릿)` ➙ `컨트롤러`
    - 필터를 적용하면 필터가 호출된 후에 서블릿이 호출됨
    - 모든 고객의 요청 로그를 남기는 요구사항이 있다면 필터를 사용
    - 필터는 특정 URL 패턴에 적용할 수 있음(`/*`: 모든 요청에 필터 적용)
- 필터 제한
  - 로그인 사용자: `HTTP 요청` ➙ `WAS` ➙ `필터` ➙ `서블릿` ➙ `컨트롤러`
  - 비 로그인 사용자: `HTTP 요청` ➙ `WAS` ➙ `필터(적절하지 않은 요청이라 판단, 서블릿 호출 X)`
    - 필터는 로직에 의해서 적절하지 않은 요청이라고 판단하면 서블릿을 호출하지 않음
- 필터 체인
  - `HTTP 요청` ➙ `WAS` ➙ `필터 1` ➙ `필터 2` ➙ `필터 3` ➙ `서블릿` ➙ `컨트롤러`
    - 필터는 체인으로 구성되며 중간에 필터를 자유롭게 추가할 수 있음

### 주요 특징

- 모든 요청에 대해 동작하는 전역적인 기능
- 서블릿 컨테이너가 관리하며, 스프링과는 독립적으로 작동
- HTTP 요청과 응답을 직접 가공할 수 있음
- 필터 체인을 구성해 여러 필터를 순서대로 적용 가능
- 적절하지 않은 요청은 서블릿에 전달하지 않고 차단 가능

### 활용 예시

- 로그인 여부나 세션 확인
- 요청 URI, IP, User-Agent 등의 로그 수집
- 문자 인코딩 설정
- XSS 방지 및 보안 필터링
- 응답 캐싱이나 압축

---

## 스프링 인터셉터 (Spring Interceptor)

![image](https://github.com/user-attachments/assets/7a4ff0ec-a4a0-44ba-afae-c3ee81089e83)

### 개념 및 역할

Interceptor는 스프링 MVC에서 제공하는 기능으로, DispatcherServlet이 컨트롤러를 호출하기 전과 후, 그리고 요청 처리가 완전히 종료된 시점까지 다양한 위치에서 요청 흐름에 개입할 수 있다.

스프링 빈으로 관리되며, 스프링의 구조와 깊게 통합되어 있어서 DI(의존성 주입) 등을 통한 확장성이 뛰어나다.

### 동작 흐름

- 인터셉터 흐름
  - `HTTP 요청` ➙ `WAS` ➙ `필터` ➙ `서블릿(디스패처 서블릿)` ➙ `스프링 인터셉터` ➙ `컨트롤러`
    - 스프링 인터셉터는 디스패처 서블릿과 컨트롤러 사이에서 컨트롤러 호출 직전에 호출됨
    - 스프링 MVC가 제공하는 기능이기 때문에 디스패처 서블릿 이후에 등장
    - 스프링 인터셉터에도 URL 패턴 적용 가능, 서블릿 URL 패턴과 다르며 매우 정밀하게 설정 가능
- 인터셉터 제한
  - 로그인 사용자: `HTTP 요청` ➙ `WAS` ➙ `필터` ➙ `서블릿` ➙ `스프링 인터셉터` ➙ `컨트롤러`
  - 비 로그인 사용자: `HTTP 요청` ➙ `WAS`➙ `필터` ➙ `서블릿` ➙ `스프링 인터셉터(적절하지 않은 요청이라 판단, 컨트롤러 호출 X)`
    - 인터셉터도 로직에 의해 적절하지 않은 요청이라고 판단하면 컨트롤러를 호출하지 않음
- 인터셉터 체인
  - `HTTP 요청` ➙ `WAS` ➙ `필터` ➙ `서블릿` ➙ `인터셉터1` ➙ `인터셉터2` ➙ `컨트롤러`
    - 체인으로 구성되어 중간에 자유롭게 인터셉터를 추가할 수 있음

### 주요 특징

- DispatcherServlet과 Controller 사이에서 동작
- preHandle, postHandle, afterCompletion 세 단계로 구분된 처리 가능
- 스프링 빈으로 등록되어 다양한 컴포넌트와 연동 가능
- 특정 URL 패턴에만 정밀하게 적용할 수 있음
- 예외가 발생해도 afterCompletion은 반드시 호출되어 예외 처리나 자원 정리에 활용 가능

### 각 단계 설명

- **preHandle**: 컨트롤러가 실행되기 전에 호출되며, 요청을 차단하거나 필요한 정보를 가공할 수 있다.
- **postHandle**: 컨트롤러가 실행된 후, 뷰 렌더링 전에 호출되어 모델 데이터를 수정하거나 응답 정보를 추가할 수 있다.
- **afterCompletion**: 모든 요청이 완료된 뒤, 뷰가 렌더링된 이후에 호출되며, 예외 로그 출력이나 리소스 정리에 사용된다.

### 활용 예시

- 세션 기반 로그인 체크
- 관리자 권한 여부 확인
- JWT 파싱 및 사용자 인증 정보 저장
- API 응답 시간 측정 및 로깅
- View에 전달할 공통 데이터 삽입

---

### 스프링 인터셉터 호출 흐름

1. 클라이언트 - HTTP 요청
2. Dispatcher Servlet
   1. `preHandle` 호출
      - 컨트롤러 호출 전에 호출(정확히는 핸들러 어댑터 호출 전에 호출)
        - `preHandle`의 응답값이 `true`이면 다음으로 진행, `false`이면 더 진행 x
        - `false`인 경우, 나머지 인터셉터는 물론이고 핸들러 어댑터도 호출되지 않음
   2. 핸들러 매핑 정보에서 핸들러 조회
   3. 핸들러 어댑터 목록에서 핸들러를 처리할 수 있는 핸들러 어댑터 조회
   4. 핸들러 어댑터의 handle(handler) 호출
3. Handler Adapter
   1. handler(실제 컨트롤러) 호출
   2. ModelAndView 반환
4. Dispatcher Servlet
   1. `postHandle` 호출
      - 컨트롤러 호출 후에 호출됨(정확히는 핸들러 어댑터 호출 후에 호출)
5. ViewResolver - View 반환 
6. Dispatcher Servlet
   1. render(model) 호출 
   2. `afterCompletion` 호출 
      - 뷰가 렌더링 된 이후에 호출됨
7. View - 클라이언트에 HTML 응답

---

## Filter vs Interceptor

| 항목 | Filter | Interceptor |
|------|--------|-------------|
| 관리 주체 | 서블릿 컨테이너 (WAS) | 스프링 컨텍스트 |
| 동작 시점 | DispatcherServlet **이전** | DispatcherServlet **이후** |
| 조작 범위 | Request/Response 객체 자체 | 객체 내부 값은 수정 가능 |
| 처리 방식 | 단일 메서드 (`doFilter`) | 세분화된 메서드 (`preHandle`, `postHandle`, `afterCompletion`) |
| URL 설정 방식 | 서블릿 기준 URL 패턴 | 스프링 MVC 경로 매핑 방식 |
| 주 사용처 | 보안 필터링, 인코딩 설정, XSS 방지 | 인증, 인가, 사용자 정보 처리, API 로깅 등 |

---

### 서블릿 필터와 스프링 인터셉터의 '인증/인가 공통 작업' 예시 요약 및 비교

- **서블릿 필터**는 모든 요청(정적 리소스 포함)을 전역적으로 선제 차단할 때 적합하다.  
  - ex) XSS 방지, JWT 존재 여부 확인, 전역 인코딩 처리

- **스프링 인터셉터**는 스프링 컨트롤러 실행 직전에 동작하므로, 사용자 정보 기반의 **세부 인증/인가** 로직에 적합하다.  
  - ex) 로그인 세션 확인, 사용자 권한(role) 체크, JWT 파싱 및 사용자 정보 저장

**차이 핵심**  
- 필터는 스프링 외부에서 단순 차단  
- 인터셉터는 스프링 내부에서 **컨트롤러별 정밀 제어** 가능


## 참고 링크
- https://github.com/jmxx219/CS-Study/blob/main/spring/Servlet%20Filter%EC%99%80%20Spring%20Interceptor.md

