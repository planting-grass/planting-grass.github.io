 ---
 title: 2026-04-07
 author: 강병호 (이름)
 date: 2026-04-07 (날짜)
 category: TIL/강병호/2026/04 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---



### 1. @RequestBody 상세 구조
클라이언트가 전송한 요청 본문(JSON)을 Java 객체로 변환(역직렬화)할 때 사용합니다.

- 동작 원리: 내부적으로 HttpMessageConverter를 거치며, JSON의 경우 ObjectMapper를 통해 객체로 변환됩니다.
- 필수 조건: 변환될 Java 객체에는 기본 생성자와 Getter 또는 Setter가 선언되어 있어야 합니다.

##### record 클래스의 예외적 동작

Java record는 기본 생성자를 자동으로 제공하지 않습니다.

하지만 Jackson 라이브러리가 일반 객체와 달리 record를 역직렬화할 때는 '모든 필드를 초기화하는 생성자'를 기본으로 사용하기 때문에 기본 생성자가 없어도 정상적으로 바인딩됩니다.

### 2. @ModelAttribute 상세 구조
클라이언트가 보낸 파라미터 데이터나 폼 데이터를 처리하거나, 뷰(View)에 데이터를 넘길 때 사용합니다. 위치에 따라 두 가지 용도로 나뉩니다.

##### 인자(Parameter) 단에서의 사용 (데이터 바인딩
쿼리 파라미터(?name=kim&age=20)나 multipart/form-data 형식의 데이터를 Java 객체로 변환합니다.
내부적으로 ModelAttributeMethodProcessor를 거치며, 지정된 클래스의 생성자를 찾아 객체로 변환하고 값을 할당합니다.

##### 메서드(Method) 단에서의 사용 (Model 속성 추가):
JSP나 Thymeleaf 같은 뷰 템플릿의 Model에 하나 이상의 속성을 전역적으로 추가하고 싶을 때 사용합니다.

예: model.addAttribute("속성 이름", "속성 값")
