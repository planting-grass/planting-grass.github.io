
 ---
 title: 2026-03-30
 author: 강병호 (이름)
 date: 2026-03-30 (날짜)
 category: TIL/강병호/2026/03 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---

### 1. @ExceptionHandler
@ExceptionHandler는 Spring MVC 애플리케이션에서 특정 컨트롤러 내부 또는 전역에서 발생하는 예외를 캐치하여 처리하기 위한 어노테이션입니다. 비즈니스 로직과 예외 처리 로직을 분리함으로써 코드의 가독성을 높이고 사용자에게 규격화된 에러 응답을 전달할 수 있게 해줍니다.

- 사용 위치: 특정 @Controller 클래스 내부(해당 컨트롤러에만 적용) 또는 @ControllerAdvice 클래스 내부(애플리케이션 전역 적용).
- 주요 기능: 발생한 예외 종류에 따라 실행될 메서드를 지정하고, HTTP 상태 코드나 에러 메시지를 자유롭게 구성합니다.

### 2. 동작 원리와 ExceptionResolver
Spring MVC에서 예외가 발생하면 DispatcherServlet은 예외를 처리할 수 있는 HandlerExceptionResolver들을 순차적으로 조회합니다.

ExceptionHandlerExceptionResolver (우선순위 1): 가장 먼저 동작하며, 발생한 예외와 매칭되는 @ExceptionHandler가 선언된 메서드가 있는지 확인합니다.

ResponseStatusExceptionResolver (우선순위 2): 예외 클래스에 @ResponseStatus 어노테이션이 있는지 확인하여 해당 HTTP 상태 코드를 설정합니다.

DefaultHandlerExceptionResolver (우선순위 3): Spring 내부의 기본 예외(예: Type Mismatch, Missing Parameter 등)를 처리합니다.
