
 ---
 title: 2026-02-26
 author: 강병호 (이름)
 date: 2026-02-26 (날짜)
 category: TIL/강병호/2026/02 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


## ThreadPoolExecutor: 스레드 풀 포화 정책 정리

자바의 `ThreadPoolExecutor`는 효율적인 자원 관리를 위해 스레드 수와 대기열(Queue)을 제한합니다. 이때 **포화 상태**란 다음 조건을 모두 만족하여 더 이상 작업을 수용할 수 없는 상태를 의미합니다.

1. **corePoolSize**: 상시 유지되는 스레드가 모두 사용 중임
2. **workQueue**: 대기열이 가득 참
3. **maximumPoolSize**: 추가로 생성 가능한 최대 스레드 수까지 모두 생성되어 사용 중임

이처럼 풀이 포화 상태일 때 새로운 작업이 제출되면, `RejectedExecutionHandler`를 통해 정의된 **포화 정책**이 실행됩니다.

---

### 주요 포화 정책 4가지

Java에서 기본으로 제공하는 `ThreadPoolExecutor`의 내부 클래스 구현체는 다음과 같습니다.

| 정책명 | 동작 방식 | 특징 및 비고 |
| --- | --- | --- |
| **AbortPolicy** | `RejectedExecutionException`을 발생시킵니다. | **기본값(Default).** 작업 누락을 방지하기 위해 예외를 던져 시스템에 알립니다. |
| **DiscardPolicy** | 아무런 예외 없이 신규 작업을 조용히 버립니다. | 작업이 유실되어도 서비스에 큰 지장이 없는 경우에 사용합니다. |
| **DiscardOldestPolicy** | 대기열에서 가장 오래된 작업(Head)을 버리고 새 작업을 추가합니다. | 최신 정보가 중요한 서비스(예: 실시간 데이터 갱신)에 적합합니다. |
| **CallerRunsPolicy** | 작업을 요청한 스레드(Caller)가 직접 작업을 수행합니다. | 새 요청의 유입을 늦추는 효과(Backpressure)가 있어 시스템 전체의 안정성을 높입니다. |

---

### 포화 정책 실행 흐름도

1. **작업 제출**: `execute(task)` 호출
2. **스레드 확인**: 활성 스레드가 `corePoolSize`보다 적으면 즉시 실행
3. **큐 확인**: 큐가 가득 차지 않았다면 대기열에 삽입
4. **최대 스레드 확인**: 큐가 가득 찼고 스레드가 `maxPoolSize`보다 적으면 스레드 추가 생성 후 실행
5. **포화 정책 실행**: 위 모든 조건이 충족되지 않을 때 선택된 `RejectedExecutionHandler` 동작

---

### 사용자 정의 포화 정책 (Custom Policy)

기본 정책 외에 특수한 비즈니스 로직이 필요한 경우, `RejectedExecutionHandler` 인터페이스를 직접 구현하여 적용할 수 있습니다. 예를 들어, 거절된 작업을 별도의 로그로 남기거나 임시 저장소에 보관하는 등의 처리가 가능합니다.

```java
class CustomPolicy implements RejectedExecutionHandler {
    @Override
    public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
        // 1. 거절된 작업에 대한 로그 기록
        System.err.println("Task rejected: " + r.toString());
        
        // 2. 필요 시 외부 저장소 저장 또는 재시도 로직 구현
        // ...
    }
}

```

**설정 방법:**

```java
ThreadPoolExecutor executor = new ThreadPoolExecutor(
    coreSize, maxSize, keepAlive, unit, queue, new CustomPolicy()
);

```
