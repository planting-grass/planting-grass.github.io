 ---
 title: 2026-02-22
 author: 강병호 (이름)
 date: 2026-02-22 (날짜)
 category: TIL/강병호/2026/02 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---

널 오브젝트 패턴 (Null Object Pattern) 정리
1. 개요
정의: 객체가 존재하지 않는 상태를 null로 처리하는 대신, 아무런 동작도 하지 않는 '유효한 객체'를 반환하여 처리하는 디자인 패턴입니다.

배경: 코드 곳곳에 산재한 if (obj == null) 형태의 방어적인 조건문을 줄이고 객체 간의 협력을 단순화하기 위해 사용합니다.

2. 구조 및 구성 요소
클라이언트는 추상화된 인터페이스를 통해 객체와 소통하며, 구체적인 구현체가 실제 로직을 수행하는지 아니면 아무것도 하지 않는지 알 필요가 없습니다.

Abstract Object (Interface/Abstract Class): 객체가 수행해야 할 공통 행위를 정의합니다.

Real Object: 실제 비즈니스 로직을 수행하는 구현체입니다.

Null Object: 인터페이스를 구현하지만 메서드 호출 시 아무런 행위를 하지 않거나, 의미 없는 기본값을 반환합니다.

3. 코드 적용 전후 비교
[AS-IS] 매번 Null 체크가 필요한 구조
Java
public void process(User user) {
    if (user != null) {
        user.authorize();
    }
    // 서비스 로직 전반에 걸쳐 null 확인 절차가 반복됨
}
[TO-BE] 널 오브젝트 패턴 적용
Java
// 1. 인터페이스 정의
interface User {
    void authorize();
}

// 2. 널 객체 구현 (아무 일도 하지 않음)
class GuestUser implements User {
    @Override
    public void authorize() {
        // No-op: 권한 부여 로직 없음
    }
}

// 3. 클라이언트 코드 (체크 로직 제거)
public void process(User user) {
    // user가 GuestUser인 경우 자연스럽게 아무 일도 일어나지 않음
    user.authorize();
}
4. 주요 특징 및 응용 사례
응용 사례: * 시스템 권한이 없는 '게스트 사용자' 처리

로그 출력 설정이 꺼져 있을 때의 '빈 로거(Null Logger)'

초기값이 0이거나 비어 있는 자료구조(예: EmptyList, ZeroCapacityStack)

장점:

코드 가독성 향상: 반복적인 조건문이 사라져 비즈니스 로직이 선명해집니다.

안정성 확보: NullPointerException 발생 가능성을 구조적으로 차단합니다.

단점 및 주의사항:

오류 식별의 어려움: 실제 시스템 오류로 인해 객체가 생성되지 않았음에도 널 객체가 동작하여 버그 탐지가 늦어질 수 있습니다.

관리 비용: 단순한 처리를 위해 별도의 클래스를 추가로 생성하고 관리해야 합니다.

5. 결론
널 오브젝트 패턴은 객체지향의 다형성을 활용해 null의 예외적인 상황을 일반적인 상황으로 편입시키는 기법입니다. 다만, 무분별한 사용은 논리적인 오류를 감출 수 있으므로 **'아무것도 하지 않는 것이 도메인적으로 타당한 경우'**에 한해 선택적으로 사용하는 것이 바람직합니다.
