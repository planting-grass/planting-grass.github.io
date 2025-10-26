 ---
 title: 2025-10-26
 author: 강병호
 date: 2025-10-26
 category: TIL/강병호/2025/10
 layout: post
 ---

**Q:** 다음 중 트랜잭션의 **격리 수준(Isolation Level)** 에 대한 설명으로 **옳지 않은 것**은?

A. Read Uncommitted에서는 커밋되지 않은 데이터를 읽을 수 있다. ⇒ Dirty Read 가능

B. Read Committed에서는 커밋된 데이터만 읽을 수 있다. ⇒ Non-Repeatable Read 가능

C. Repeatable Read에서는 같은 쿼리를 여러 번 실행해도 항상 같은 결과를 얻는다. ⇒ Phatom Read 가능, 동일 행에 대한 일관된 값 보장

D. Serializable은 동시에 실행 중인 트랜잭션의 병행성을 극대화한다. ⇒ 가장 엄격한 격리 

정답 : D

Serializable은 병행성을 높이지 않고 줄인다.

### 트랜잭션 격리 수준

여러 트랜잭션이 동시에 수행될 때, 서로의 중간 결과를 얼마나 허용할지 정하는 규칙으로 동시성과 일관성 사이의 트레이드오프를 조정

- Read Uncommitted : 가장 낮은 수준으로 커밋되지 않은 데이터도 읽는다
    - 허용되는 이상현상 : Dirty Read, Non-repeatable Read, Phantom Read
- Read Committed : 오라클에서 기본값으로 되어 있으며 커밋된 데이터만 읽는다.
    - 허용되는 이상현상 : Non-repeatable Read, Phantom Read
- Repeatable Read : 같은 트랜잭션 내에서 같은 행 재조회 시 일관성이 유지된다.
    - 허용되는 이상현상 : Phantom Read
- Serializable : 완전히 직렬화된다. 모든 트랜잭션을 순차적으로 실행한 것과 동일한 결과를 보장한다. 다만 병행성은 매우 안좋아진다.

### 주요 이상현상

- Dirty Read : 커밋되지 않은 데이터를 읽는다.
    - e.g. T1이  수정 중인데 T2가 그 데이터를 읽는다.
- Non-repeatable Read : 같은 쿼리를 두 번 실행했을 때 값이 다름
    - e.g. T1이 읽는 동안 T2가 그 행을 수정한다.
- Phantom Read : 같은 조건으로 다시 조회했을 때 새로운 행이 생긴다.
    - e.g. T1이 조회한 후, T2가 새로운 행을 INSERT
