 ---
 title: 2025-08-31
 author: 강병호
 date: 2025-08-31
 category: TIL/강병호/2025/08
 layout: post
 ---


# **프로세스 스케줄링 알고리즘에는 어떤 것들이 있나요?**

- FIFO : First-Com First-Served 로 먼저 도착 한 프로세스를 먼저 실행하는 로직
- SJF  : Shortest Job First의 약자로 실행 시간이 짧은 프로세스를 우선 실행하는 로직이다.
- SRTF (Shortest Remaining Time First) : 남은 실행 시간이 가장 짧은 프로세스를 우선 실행을 SJF에서 선점형의 로직을 추가한 것이다.
- RR (Round Robin) : 고정된 Time Slice단위로 프로세스를 교대로 실행.
- Priority Scheduling : 프로세스마다 우선순위를 부여해 높은 우선순위를 먼저 실행한다.
- Multi-level Queue : 여러 개의 큐를 두고 각각 다른 스케줄링 정책 적용한다.
- Multi-level Feedback Queue : MLQ의 단점을 보완한 것으로 실행 시간/응답 시간을 기반으로 큐 간 이동이 가능한 다단계 스케줄링이다.
- RR을 사용할 때, Time Slice에 따른 trade-off를 설명해 주세요.
    
    RoundRobin 시에 Time Slice를 통해 각 프로세스들이 스케줄링 된다.
    
    이러한 Time Slice가 너무 짧은 경우 다른 프로세스로의 전환이 자주 이루어지기 때문에 Context Switching이 자주 발생하여 오버헤드가 증가하게 된다. 심각한 경우, 실제 프로세스 처리 시간보다 스케줄링 과정에서의 시간이 더 오래걸리는 경우도 발생할 수 있다.
    
    반대로 Time Slice가 너무 긴 경우에는 응답 시간이 너무 길어져 인터랙티브 프로세스가 필요한 경우의 응답성이 저하되는 문제가 발생한다.
    
- 싱글 스레드 CPU 에서 상시로 돌아가야 하는 프로세스가 있다면, 어떤 스케쥴링 알고리즘을 사용하는 것이 좋을까요? 또 왜 그럴까요?
    
    프로세스가 상시적으로 실행되도록 하려면 우선순위가 높고 다른 프로세스로 부터 자원을 선점당해서는 안된다. 이러한 프로세스처리에서는 주로 Prioirty Scheduling, Real-Time Scheduling을 사용하여처리한다.
