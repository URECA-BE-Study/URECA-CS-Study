## 신예지

<details>
<summary><strong>REST의 구성요소를 말해주세요</strong></summary>

Resource(자원), Verb(행위), Presentation(표현)이 있습니다.

</details>

<details>
<summary><strong>REST의 특징을 설명해주세요</strong></summary>

1. Uniform Interface(인터페이스 일관성)
2. Stateless(무상태)
3. Cacheable(캐시 처리 가능)
4. Layered System(계층화)
5. Server-Client(서버-클라이언트 구조)

</details>

<details>
<summary><strong>REST란 무엇인가요?</strong></summary>

자원의 이름(자원의 표현)으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미합니다.

</details>

---

## 변하영

<details>
<summary><strong>REST와 RESTful의 차이는 무엇인가요?</strong></summary>

REST는 자원을 URI로 식별하고, HTTP 메서드를 통해 자원에 대한 행위를 정의하는 아키텍처 스타일입니다. RESTful은 그 원칙을 잘 지킨 시스템을 말합니다.
단순히 REST 방식으로 API를 만든다고 해서 RESTful하다고 할 수는 없고, URI 설계나 HTTP 메서드 사용 규칙을 제대로 지켜야 RESTful한 시스템이라고 할 수 있습니다.

</details>

<details>
<summary><strong>REST 아키텍처 스타일의 핵심 개념과 구성 요소는 무엇인가요?</strong></summary>

REST는 자원을 URI로 식별하고, HTTP 메서드를 통해 자원에 대한 행위를 정의하는 아키텍처 스타일입니다.
구성 요소로는 자원(Resource), 행위(Method), 표현(Representation)이 있습니다.

</details>

<details>
<summary><strong>HTTP 메서드 중 PUT과 PATCH의 차이를 설명하고, 각각 언제 사용하는 것이 적절한가요?</strong></summary>

PUT은 자원을 전체 교체할 때 사용하고, PATCH는 자원의 일부만 수정할 때 사용합니다.
예를 들어, 사용자 정보를 전부 바꾸려면 PUT, 일부 필드만 수정할 경우에는 PATCH가 적절합니다.

</details>

<details>
<summary><strong>URL과 URN의 차이점을 설명하세요</strong></summary>

URL은 어떻게 리소스를 얻고 어디에서 가져올지 명시하는 URI이고, URN은 경로와 리소스 자체를 특정하는 것을 목표로 하는 URI입니다.

</details>

---

## 김수훈

<details>
<summary><strong>REST란 무엇이고, 그 특징에 대해 설명해주세요.</strong></summary>

REST는 Representational State Transfer의 약자로, HTTP URI로 자원을 명시하고 HTTP 메서드로 CRUD를 수행하는 아키텍처 스타일입니다.

특징으로는 인터페이스 일관성, 무상태성, 캐싱 지원, 계층 구조, 그리고 클라이언트-서버 분리를 들 수 있습니다.

이를 통해 확장성과 다양한 클라이언트 간 일관된 API 사용이 가능해집니다.

</details>

<details>
<summary><strong>REST API와 RESTful API의 차이점은 무엇인가요?</strong></summary>

REST API는 REST 원칙을 따르는 API를 의미하지만, RESTful API는 이를 더 엄격히 준수하는 경우를 말합니다.

예를 들어, HTTP 메서드의 올바른 사용, URI에 동사 대신 명사 사용, 상태 코드 활용 등이 지켜져야 RESTful하다고 합니다.

RESTful API의 목적은 성능보다는 일관성과 이해도, 호환성 향상에 있습니다.

</details>

<details>
<summary><strong>REST API 설계 시 중요한 원칙은 무엇인가요?</strong></summary>

REST API 설계 시 중요한 원칙으로는:

* URI는 자원을 명사로 표현
* 행위는 HTTP 메소드로 표현
* URI 경로는 계층 관계를 나타냄
* 소문자 사용, 언더바 대신 하이픈 사용
* URI 끝에 슬래시 포함하지 않음
* 파일 확장자 대신 Accept 헤더 사용
* 적절한 HTTP 상태 코드 반환

요청 처리 결과를 명확히 전달해야 합니다.

</details>

---

## 박상윤

<details>
<summary><strong>GET과 POST의 차이점은 무엇인가요?</strong></summary>

GET은 데이터를 조회하는 데 사용되며, 서버 상태를 변경하지 않습니다. 캐싱과 즐겨찾기 등에 유리하고, 쿼리 파라미터로 데이터를 전달합니다.

POST는 데이터를 생성하거나 처리할 때 사용되며, 서버의 상태를 변경합니다. 요청 본문에 데이터를 담습니다.

</details>

<details>
<summary><strong>PUT과 PATCH의 차이점은 무엇인가요?</strong></summary>

PUT은 자원의 전체를 대체하는 방식으로 동작하며, 요청 본문에 자원의 모든 필드를 포함해야 합니다. 일부 필드만 전달하면 나머지 필드는 제거될 수 있습니다.

PATCH는 자원의 일부만 수정할 때 사용하며, 변경이 필요한 필드만 포함하면 됩니다. 전체 자원을 보낼 필요가 없기 때문에 네트워크 부담이 적고, 실제 실무에서도 더 자주 사용됩니다.

</details>

<details>
<summary><strong>사용자 정보 수정 API를 만들고 있습니다. 이름(name)과 이메일(email), 비밀번호(password)를 포함한 전체 사용자 정보를 수정하려고 합니다. 이때 클라이언트가 일부 필드(name, email)만 전달했을 경우, 서버에서 나머지 필드(password)를 유지하고 싶습니다. 이때 적절한 HTTP 메서드는 무엇인가요?</strong></summary>

**PATCH**

PUT은 전체 자원을 덮어쓰는 방식이기 때문에 전달되지 않은 필드는 제거될 수 있습니다. 반면 PATCH는 전달된 필드만 업데이트하므로 나머지 필드를 유지할 수 있습니다.

</details>

---

## 이소원

<details>
<summary><strong>REST의 구성요소가 뭔가요?</strong></summary>

Resource(자원), Verb(행위), Representation(표현) 입니다.
모든 자원에는 고유한 ID(HTTP URI)가 존재하고, 자원은 서버에 존재합니다.
행위는 HTTP Method를 사용하고, 표현은 JSON, XML을 통해 클라이언트의 요청에 대한 응답을 보내는 것을 의미합니다.

</details>

<details>
<summary><strong>REST와 REST API 차이가 뭔가요?</strong></summary>

REST는 자원의 이름(표현)으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미합니다.
REST API는 REST를 기반으로 구현한 API로, REST 아키텍쳐를 따르는 시스템/애플리케이션 인터페이스(데이터 통신 방법)를 의미합니다.

</details>

<details>
<summary><strong>REST API의 장점은 뭔가요?</strong></summary>

요청 상태 그대로 추론이 가능하다는 장점이 있습니다.

</details>

<details>
<summary><strong>RESTful 은 뭔가요?</strong></summary>

얼마나 REST 원칙을 잘 지키고 있는지에 대한 상태나 특성을 나타내는 것을 의미합니다.
성능 향상보다 일관성이 중요할 때 사용하는 게 좋습니다.

</details>
