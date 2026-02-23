 ---
 title: 2026-02-23
 author: 강병호 (이름)
 date: 2026-02-23 (날짜)
 category: TIL/강병호/2026/02 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


## RDB 페이징 쿼리의 이해와 성능 최적화

RDB에서 페이징 쿼리는 전체 데이터를 일정한 단위로 나누어 조회하는 방식이다. 대량의 데이터를 한꺼번에 불러올 때 발생하는 리소스 과부하를 방지하고, 응답 시간을 단축하기 위해 사용된다.

---

### 1. LIMIT, OFFSET 방식의 특징과 한계

MySQL 등 대중적인 RDB에서 가장 보편적으로 사용하는 방식은 `LIMIT`과 `OFFSET` 구문을 조합하는 것이다.

```sql
SELECT * FROM subscribe
ORDER BY id DESC
LIMIT 10 OFFSET 100;

```

#### 성능 저하의 원인

이 방식의 결정적인 단점은 **뒤쪽 페이지로 갈수록 조회 속도가 선형적으로 느려진다**는 점이다. `OFFSET 1,000,000, LIMIT 10`이라는 쿼리가 실행될 때, 데이터베이스 엔진은 내부적으로 다음과 같이 동작한다.

1. 정렬 기준에 따라 앞선 **1,000,000개의 레코드를 전부 읽어서 메모리에 적재**한다.
2. 읽어 들인 1,000,000개의 데이터를 순차적으로 **버린다**.
3. 그다음 위치한 10개의 데이터만 결과로 반환한다.

결과적으로 사용자가 보지 않는 데이터까지 전부 읽어야 하므로, 데이터 양이 많아질수록 디스크 I/O와 CPU 사용량이 급격히 증가한다.

---

### 2. No-Offset (Keyset Pagination)을 통한 최적화

`OFFSET`의 성능 문제를 해결하기 위해 **마지막으로 조회된 데이터의 식별자(Key)**를 기준으로 다음 페이지를 조회하는 방식을 사용한다. 이를 'No-Offset' 또는 'Keyset Pagination'이라고 부른다.

#### 구현 예시

특정 시점에 구독을 해제한 사용자를 조회하는 시나리오를 가정한다.

* **테이블 구조 및 인덱스**
```sql
CREATE TABLE subscribe (
    id INT NOT NULL AUTO_INCREMENT,
    deleted_at DATETIME NULL,
    PRIMARY KEY(id),
    KEY idx_deleted_at_id(deleted_at, id)
);

```


* **첫 번째 페이지 조회**
```sql
SELECT * FROM subscribe
WHERE deleted_at >= ? AND deleted_at < ?
ORDER BY deleted_at, id
LIMIT 10;

```


* **이후 페이지 조회 (No-Offset)**
이전 페이지의 마지막 데이터가 `deleted_at = '2024-01-01 00:00:00'`, `id = 78`인 경우, 해당 지점 이후부터 조회를 시작한다.
```sql
SELECT * FROM subscribe
WHERE (deleted_at = '2024-01-01 00:00:00' AND id > 78)
   OR (deleted_at > '2024-01-01 00:00:00' AND deleted_at < ?)
ORDER BY deleted_at, id
LIMIT 10;

```



#### 최적화 원리

No-Offset 방식은 인덱스에 저장된 순서를 따라가다가 **조건에 맞는 위치로 즉시 점프(Seek)**하여 데이터를 읽기 시작한다. 버려지는 데이터가 없으므로 페이지 번호가 뒤로 밀려나도 응답 시간이 일정하게 유지된다(O(1) 성능).

---

### 3. 방식별 비교 및 결론

| 구분 | LIMIT & OFFSET | No-Offset (Keyset) |
| --- | --- | --- |
| **조회 성능** | 뒤로 갈수록 느려짐 (O(N)) | 항상 일정함 (O(1)에 수렴) |
| **데이터 정합성** | 조회 중 데이터 추가/삭제 시 중복/누락 발생 가능 | 데이터 변화에 안정적임 |
| **사용 편의성** | 임의의 페이지(N번) 이동 가능 | 이전 페이지 정보 필수, 순차 이동만 가능 |
| **적합한 사례** | 데이터가 적거나 일반적인 게시판 형태 | 대용량 데이터, 무한 스크롤, API 처리 |

대용량 시스템의 백엔드 설계 시에는 서비스 기획 요건을 고려하되, 성능 최적화가 필수적인 구간에서는 `OFFSET`을 제거한 페이징 처리를 우선적으로 검토해야 한다.
