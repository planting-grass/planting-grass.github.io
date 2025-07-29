---
title: Java의 람다식과 Stream
author: 곽영헌
date: 2025-07-29
category: TIL/곽영헌/2025/07
layout: post
---

### 오늘 배운 것
1.  **람다 표현식 (Lambda Expression)**
    *함수적 프로그래밍을 지원하기 위해 도입된 기능으로, 이름이 없는 함수(Anonymous Function)입니다. 
    * 기존의 익명 내부 클래스(Anonymous Inner Class)를 훨씬 간결하게 표현할 수 있습니다. 
    * 람다식은 `(매개변수) -> { 실행문 }` 형태로 작성하며, 매개변수의 타입은 추론 가능하므로 생략할 수 있습니다.  실행문이 하나일 경우 중괄호 `{}`와 세미콜론 `;`도 생략 가능합니다. 
        ```java
        // 기존 익명 내부 클래스 방식
        Arrays.sort(langs, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.compareTo(o2);
            }
        });

        // 람다 표현식
        Arrays.sort(langs, (o1, o2) -> o1.compareTo(o2));
        ```
    * 람다 실행문이 단일 메서드 호출로 끝나는 경우, **메서드 참조(Method Reference)** `::` 연산자를 사용하여 코드를 더욱 간결하게 만들 수 있습니다. 
        ```java
        // 람다식
        Arrays.sort(nums, (num1, num2) -> Integer.compare(num1, num2)); // static method 
        
        // 메서드 참조
        Arrays.sort(nums, Integer::compare); // static method 참조 
        ```

2.  **함수형 인터페이스 (Functional Interface)**
    * 람다식을 할당받을 수 있는, **단 하나의 추상 메서드(Abstract Method)** 만을 가진 인터페이스를 의미합니다. 
    * `@FunctionalInterface` 어노테이션을 붙이면, 컴파일러가 추상 메서드가 하나만 있는지 검사하여 안정성을 높여줍니다. 
    * `Object` 클래스의 public 메서드(e.g., `equals`)는 추상 메서드 개수 계산에서 제외됩니다. 
    * 자주 사용되는 함수 형태를 `java.util.function` 패키지에 표준 API로 정의해두었습니다. 
        * **`Consumer<T>`**: 파라미터를 받고 리턴값이 없음 (`void accept(T)`). 
        * **`Supplier<T>`**: 파라미터 없이 리턴값만 있음 (`T get()`). 
        * **`Function<T, R>`**: 파라미터(T)를 받아 처리 후 다른 타입의 결과(R)를 리턴 (`R apply(T)`). 주로 매핑에 사용됩니다. 
        * **`Operator<T>`**: `Function`과 유사하지만, 입력과 출력이 동일한 타입일 때 사용합니다. 
        * **`Predicate<T>`**: 파라미터를 받아 조건을 검사한 후 `boolean` 값을 리턴 (`boolean test(T)`). 

3.  **Stream API**
    * 배열 및 컬렉션의 요소를 하나씩 참조하며, 람다식을 활용해 데이터를 효율적으로 처리하기 위한 기능입니다. 
    * 데이터 소스에 대한 공통된 접근 방식을 제공하여 코드를 간결하게 만듭니다. 
    * 스트림은 **생성 → 중간 처리 → 최종 처리**의 파이프라인 구조를 가집니다. 
        * **중간 처리 (Intermediate Operation)**: `filter()`(필터링), `map()`(변환), `sorted()`(정렬) 등이 있으며, 여러 개를 연결할 수 있습니다. 
        * **최종 처리 (Terminal Operation)**: `forEach()`(반복), `collect()`(수집), `sum()`(합계) 등이 있으며, 스트림 처리를 시작시키고 최종 결과를 반환합니다. 
    * `map`과 `flatMap`의 차이:
        * `map`: 스트림의 요소를 1:1로 변환합니다. 
        * `flatMap`: 스트림의 각 요소를 여러 개의 요소로 구성된 새로운 스트림으로 변환합니다 (1:N 매핑). 
    * `collect()`는 스트림의 최종 결과를 List, Set, Map 등 다른 컬렉션으로 모으는 데 사용되며, 

---

### 내일 할 일
1. B형 시험 준비를 위해 pro문제를 풀어본다.
2. 지금까지 메모해놓은 Java 관련 내용을 훑어본다.

---

