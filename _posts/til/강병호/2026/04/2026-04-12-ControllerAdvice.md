 ---
 title: 2026-04-12
 author: 강병호 (이름)
 date: 2026-04-12 (날짜)
 category: TIL/강병호/2026/04 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


# @ControllerAdvice

## 1. 주요 개념
`@ControllerAdvice`는 애플리케이션 내의 모든 컨트롤러(`@Controller`, `@RestController`)에 대해 **공통적인 기능(Aspect)**을 제공하는 어노테이션입니다. 여러 컨트롤러에 흩어진 중복 코드를 제거하고, 전역적인 정책을 적용할 때 사용합니다.

* **빈 등록:** 내부에 `@Component`가 포함되어 있어, 스프링 부트 실행 시 컴포넌트 스캔을 통해 자동으로 빈(Bean)으로 등록됩니다.
* **AOP의 활용:** 내부적으로 스프링 AOP(Aspect Oriented Programming) 기술을 사용하여, 컨트롤러 호출 전후에 특정 로직을 끼워 넣는 방식으로 동작합니다.



## 2. 주요 기능 (3가지 핵심 어노테이션)
`@ControllerAdvice`가 선언된 클래스 내부에서 다음 어노테이션들을 사용하여 기능을 구현합니다.

* **@ExceptionHandler:** 특정 예외가 발생했을 때 이를 가로채서 처리하는 로직을 정의합니다. 가장 많이 사용되는 기능입니다.
* **@InitBinder:** 컨트롤러로 들어오는 요청 파라미터를 바인딩하거나 검증할 때 사용할 `WebDataBinder`를 초기화합니다. (예: 날짜 형식 지정, 특정 필드 바인딩 제외 등)
* **@ModelAttribute:** 모든 컨트롤러의 뷰(View)에서 공통으로 사용할 데이터를 설정합니다. 메서드 위에 선언하면 해당 메서드가 반환하는 값이 자동으로 모델에 담깁니다.

---

## 3. @ControllerAdvice vs @RestControllerAdvice

실무에서는 주로 JSON 응답을 내보내는 REST API를 구축하므로 `@RestControllerAdvice`를 더 자주 사용합니다.

| 구분 | @ControllerAdvice | @RestControllerAdvice |
| :--- | :--- | :--- |
| **기본 구성** | `@Component` | `@ControllerAdvice` + **`@ResponseBody`** |
| **주 응답 형태** | HTML 뷰(View) 또는 데이터 | **JSON / XML 데이터** |
| **주요 용도** | SSR(Thymeleaf 등) 기반 웹 서비스 | **RESTful API 서비스** |



---

## 4. 도입 시 이점
* **코드 중복 제거:** 모든 컨트롤러마다 `try-catch`를 작성하거나 `@ExceptionHandler`를 일일이 선언할 필요가 없습니다.
* **유지보수성 향상:** 에러 메시지 규격이나 데이터 바인딩 정책이 변경될 때, 이 클래스 한 곳만 수정하면 전체 시스템에 반영됩니다.
* **비즈니스 로직 집중:** 컨트롤러는 성공 시나리오에만 집중하고, 예외 케이스는 Advice가 전담하여 코드가 간결해집니다.

