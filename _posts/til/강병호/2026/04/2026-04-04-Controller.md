 ---
 title: 2026-04-04
 author: 강병호 (이름)
 date: 2026-04-04 (날짜)
 category: TIL/강병호/2026/04 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


### @Controller vs @RestController의 역할 차이
1. @Controller (화면 반환용)

- 목적: 사용자에게 보여줄 뷰(View, 예: HTML 화면)를 반환할 때 사용합니다.
- 동작 방식: 컨트롤러 메서드가 반환하는 문자열(예: return "home";)을 **뷰 리졸버(View Resolver)**가 가로채서, 해당 이름과 일치하는 템플릿 파일(JSP, Thymeleaf 등)을 찾아 HTML로 렌더링한 뒤 클라이언트에게 응답합니다.

2. @RestController (데이터 반환용 API)

- 목적: 화면이 아니라 순수한 데이터(JSON, XML 등)를 프론트엔드나 모바일 앱 등에 반환하는 RESTful API를 만들 때 사용합니다.
- 동작 방식: 내부적으로 @Controller와 @ResponseBody가 결합된 형태입니다. 뷰 리졸버를 거치지 않고, 반환하는 객체나 데이터를 **HTTP 응답 본문(Body)**에 직접 꽂아 넣습니다. (객체를 반환하면 스프링이 알아서 JSON 형태로 변환해 줍니다).
