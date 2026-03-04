
 ---
 title: 2026-03-03
 author: 강병호 (이름)
 date: 2026-03-03 (날짜)
 category: TIL/강병호/2026/03 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


## 템플릿 메서드 패턴 (Template Method Pattern)

### 1. 개요
정의: 알고리즘의 구조를 메서드에 정의하고, 하위 클래스에서 알고리즘 구조의 변경 없이 특정 단계들을 재정의할 수 있게 하는 행위 디자인 패턴

핵심 역할: 전체적인 실행 흐름(뼈대)은 상위 클래스에서 결정하고, 구체적인 단계별 구현은 하위 클래스에 맡기는 방식

### 2. 구조 및 구성 요소
AbstractClass (추상 클래스): 템플릿 메서드를 정의하며, 알고리즘의 골격을 담당하는 메서드를 포함함

Template Method: 알고리즘의 뼈대가 되는 메서드로, 내부에서 추상 메서드들을 순서대로 호출함. 하위 클래스에서 오버라이딩하지 못하도록 final 키워드를 주로 사용함

ConcreteClass (구현 클래스): 상위 클래스의 추상 메서드를 구현하여 각 단계의 구체적인 로직을 완성함

### 3. 코드 예시 
```
public abstract class Student {
    // 하위 클래스에서 구현해야 할 단계별 로직
    public abstract void study();
    public abstract void watchYoutube();
    public abstract void sleep();

    // 템플릿 메서드: 전체적인 실행 순서를 제어함
    public final void doDailyRoutine() {
        study();
        watchYoutube();
        sleep();
    }
}

class BackendStudent extends Student {
    @Override
    public void study() {
        System.out.println("영한님 JPA 강의를 수강합니다.");
    }

    @Override
    public void watchYoutube() {
        System.out.println("개발바닥 유튜브를 시청합니다.");
    }

    @Override
    public void sleep() {
        System.out.println("7시간 잠을 잡니다.");
    }
}
```

### 4. 장단점 분석
#### 장점
- 코드 중복 제거: 공통 로직을 상위 클래스에 관리하여 중복을 최소화함

- 재사용성 향상: 동일한 알고리즘 구조를 가진 다양한 변형 클래스를 쉽게 추가할 수 있음

- 확장성: 핵심 로직의 틀을 유지하면서 특정 부분만 유연하게 변경 가능함

#### 단점
- LSP(리스코프 치환 원칙) 위반 가능성: 하위 클래스가 상위 클래스의 의도를 잘못 이해하고 로직을 변경할 경우 문제가 발생할 수 있음

- 높은 의존도: 상위 클래스의 설계 변경이 발생하면 이를 상속받은 모든 하위 클래스에 영향이 파급됨

- 가독성 저하: 로직의 흐름이 상위 클래스에 있어, 하위 클래스 코드만 봐서는 전체 동작 방식을 파악하기 어려울 수 있음
