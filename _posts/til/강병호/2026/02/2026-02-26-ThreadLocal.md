 ---
 title: 2026-02-26
 author: 강병호 (이름)
 date: 2026-02-26 (날짜)
 category: TIL/강병호/2026/02 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---

## 1. ThreadLocal
특정 스레드 내에서만 접근 가능한 지역 변수를 선언할 수 있게 해주는 클래스입니다.

- ThreadLocalMap: 각 스레드는 고유한 ThreadLocalMap 객체를 내부 필드로 보유합니다.

- Key-Value 구조: ThreadLocal 인스턴스 자체가 Key가 되며, 저장하고자 하는 데이터가 Value가 됩니다.

- 스레드 격리: 동일한 ThreadLocal 변수를 여러 스레드가 동시에 참조하더라도, 실제 데이터는 각 스레드의 개별 메모리 공간(ThreadLocalMap)에서 관리되므로 별도의 동기화 로직 없이도 Thread-safe를 보장합니다.

## 2. 주요 활용 사례 (Spring Framework)
Spring은 인프라 스트럭처 레벨에서 스레드별 문맥(Context)을 유지하기 위해 이를 적극 활용합니다.

- TransactionSynchronizationManager: 동일 스레드 내에서 리소스(Connection)와 트랜잭션 상태를 동기화.

- SecurityContextHolder: 현재 요청을 처리 중인 스레드에 사용자 인증 정보(Principal)를 저장 및 공유.

- RequestContextHolder: Service나 Repository 계층에서도 현재 스레드의 HTTP 요청 속성에 접근 가능.

## 3. 장점
- 동시성 제어: 공유 자원을 경합하지 않으므로 synchronized나 Lock 사용에 따른 성능 저하와 데드락 위험이 없습니다.

- 설계 편의성: 비즈니스 로직 수행 시 필요한 공통 컨텍스트를 메서드 파라미터로 일일이 전달하지 않아도 되므로 코드가 간결해집니다.

## 4. 주의 사항 및 한계점
- Thread Pool 환경의 데이터 오염: WAS(Tomcat 등)는 스레드를 재사용합니다. 작업 종료 후 remove()를 호출하지 않으면, 재사용된 스레드가 이전 요청의 데이터를 그대로 참조하여 보안 사고나 로직 오류를 유발합니다.

- 비동기 전파 제약: @Async 등을 통해 새로운 스레드가 생성될 경우, 부모 스레드의 ThreadLocal 데이터는 상속되지 않습니다. 이를 해결하려면 TaskDecorator를 통해 컨텍스트를 복사하는 추가 설정이 필요합니다.

## 5. 대체 기술 및 확장 클래스
- @RequestScope: Spring 빈의 스코프를 HTTP 요청 단위로 제한하여 스레드 안전하게 데이터를 관리합니다.

- ConcurrentHashMap: 공용 저장소로 활용 가능하나, 수동으로 스레드 식별자를 관리해야 하는 번거로움이 있습니다.

- NamedThreadLocal: Spring에서 제공하는 확장 클래스입니다. 이름을 부여할 수 있어 디버깅 시 어떤 용도의 ThreadLocal인지 명확히 식별할 수 있습니다.
