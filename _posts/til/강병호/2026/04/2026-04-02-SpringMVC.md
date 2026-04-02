 ---
 title: 2026-04-02
 author: 강병호 (이름)
 date: 2026-04-02 (날짜)
 category: TIL/강병호/2026/04 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


Spring MVC는 **프론트 컨트롤러(Front Controller) 패턴**을 기반으로 동작하며, 모든 요청의 중앙 관제탑 역할을 하는 `DispatcherServlet`을 중심으로 유연하게 설계되어 있습니다. 응답 방식(HTML View 또는 JSON 데이터)에 따른 실행 흐름을 정리했습니다.

---

### 1. View를 응답하는 경우 (SSR 방식)

클라이언트의 요청을 받아 적절한 HTML 페이지를 생성하여 반환하는 전통적인 흐름입니다.



1.  **DispatcherServlet 요청 수신**: 모든 HTTP 요청은 프론트 컨트롤러인 `DispatcherServlet`이 가장 먼저 받습니다.
2.  **핸들러 조회 (HandlerMapping)**: URL에 매핑된 적절한 컨트롤러(핸들러)를 찾습니다.
3.  **핸들러 어댑터 조회 (HandlerAdapter)**: 찾은 핸들러를 실제로 실행할 수 있는 어댑터를 결정합니다.
4.  **핸들러 실행**: 어댑터가 컨트롤러의 메서드를 호출하여 비즈니스 로직을 수행합니다.
5.  **ModelAndView 반환**: 컨트롤러는 결과 데이터(`Model`)와 이동할 페이지 이름(`View Name`)을 반환합니다.
6.  **뷰 리졸버 호출 (ViewResolver)**: 반환된 뷰 이름을 해석하여 실제 물리적인 뷰 경로(JSP, Thymeleaf 등)를 찾습니다.
7.  **뷰 렌더링 및 응답**: 찾은 뷰를 실행하여 최종 HTML을 생성하고 클라이언트에게 응답합니다.

---

### 2. 데이터를 응답하는 경우 (API 방식)

최근 백엔드 개발에서 주로 사용하는 방식으로, `JSON`이나 `XML` 데이터를 응답할 때의 흐름입니다. 이때는 **HttpMessageConverter**가 핵심 역할을 합니다.



1.  **DispatcherServlet 요청 수신**: 동일하게 `DispatcherServlet`이 요청을 받습니다.
2.  **핸들러 조회 및 어댑터 실행**: URL에 맞는 컨트롤러를 찾고 어댑터를 통해 메서드를 실행할 준비를 합니다.
3.  **ArgumentResolver와 요청 데이터 처리**: `RequestMappingHandlerAdapter`가 컨트롤러의 파라미터를 생성할 때 `ArgumentResolver`를 호출합니다. 이때 `@RequestBody`나 `HttpEntity`가 있다면 `HttpMessageConverter`를 사용해 HTTP 메시지 바디의 데이터를 객체로 변환합니다.
4.  **비즈니스 로직 수행**: 컨트롤러는 변환된 객체를 사용해 서비스 로직을 처리합니다.
5.  **ReturnValueHandler와 응답 데이터 처리**: 컨트롤러가 결과를 반환할 때 `@ResponseBody`나 `HttpEntity`가 있다면 `ReturnValueHandler`가 동작합니다. 이때 다시 `HttpMessageConverter`가 호출되어 객체를 JSON 등의 메시지 형식으로 변환하여 응답 바디에 직접 기록합니다.

---

### 3. 핵심 컴포넌트 요약

| 컴포넌트 | 역할 |
| :--- | :--- |
| **DispatcherServlet** | HTTP 요청을 받아 적절한 컴포넌트로 전달하는 프론트 컨트롤러 |
| **HandlerMapping** | URL과 컨트롤러 메서드를 매핑하는 정보 보관소 |
| **HandlerAdapter** | 다양한 형태의 컨트롤러를 실행할 수 있게 해주는 어댑터 |
| **ViewResolver** | 논리적인 뷰 이름을 실제 뷰 기술(Thymeleaf 등)로 연결 |
| **HttpMessageConverter** | HTTP 메시지 바디와 자바 객체 간의 변환을 담당 (JSON/Text 등) |

---

### 요약 및 결론

Spring MVC는 **ViewResolver**를 통한 화면 응답과 **HttpMessageConverter**를 통한 데이터 응답 흐름이 분리되어 있지만, 전체적인 제어 흐름은 `DispatcherServlet`이 일관되게 관리합니다. 특히 API 통신 시에는 `Accept` 헤더와 반환 타입을 분석하여 최적의 컨버터를 선택하는 전략적 유연함이 큰 장점입니다.

Spring MVC에서 컨트롤러의 중복되는 로직을 공통 처리하기 위한 **Interceptor**나 **AOP**의 적용 시점에 대해서도 더 자세히 다뤄볼까요?
