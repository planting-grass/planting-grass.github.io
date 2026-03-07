### 핵심

- 컴파일 시점에 확인되는가?
- 예외 처리를 강제하는가?

## CheckedException

- 컴파일 시점에 확인되고, 반드시 처리해야 하는 예외
- `IOException`, `SQLException`
- Checked Exception을 유발하는 메서드를 호출하는 경우:
    - 메서드 시그니처에 `throws`를 사용해 호출자에게 예외를 위임
    - 메서드 내에서 try-catch를 사용하여 해당 예외를 반드시 처리

## UncheckedException

- 런타임 시점에 발생하는 예외라서, 컴파일러가 처리 여부를 강제하지 않음
- `RuntimeException` 을 상속한 예외들
- 발생원인: 프로그래머의 실수나 코드 오류

## **각각 언제 사용해야 할까요? 🤔**

> Checked Exception
> 
- 외부 환경과의 상호작용에서 발생할 가능성이 높은 예외에 적합함
- 예시: 파일 입출력, 네트워크 통신 등에서 발생할 수 있는 예외
- 이유: 이러한 예외는 예측 가능하며, 호출하는 쪽에서 적절히 처리할 수 있는 여지가 있음
  
> Unchecked Exception
> 
- 코드 오류, 논리적 결함 등 프로그래머의 실수로 인해 발생할 수 있는 예외에 적합함
- 예시: null 참조 또는 잘못된 인덱스 접근
- 이유: 호출자가 미리 예측하거나 처리할 수 없음

## **Error와 Exception의 차이는 무엇인가요? 🤓**

> Error
> 
- JVM에서 발생하는 심각한 문제로 `OutOfMemoryError`, `StackOverflowError` 등 시스템 레벨에서 발생하는 오류
- 일반적으로 프로그램에서 처리하지 않고, **회복이 어려운 오류**에 속하며, 애플리케이션 코드에서 복구할 수 없는 심각한 문제임.
   
> Exception
> 
- 프로그램 실행 중 발생할 수 있는 오류 상황
- 대부분의 경우 **회복 가능성**이 있고, 프로그램 내에서 예외 처리를 통해 오류 상황을 제어함
  
**참고 자료:**
  
- [[10분 테코톡] 케로의 예외처리](https://youtu.be/mrrEPbGF6hQ?si=CKhaV9Gwu4Qx97Y1)
- [기억보단 기록을 - 좋은 예외(Exception) 처리](https://jojoldu.tistory.com/734)
- [custom exception을 언제 써야 할까?](https://tecoble.techcourse.co.kr/post/2020-08-17-custom-exception/)
