
 ---
 title: 2026-02-20
 author: 강병호 (이름)
 date: 2026-02-20 (날짜)
 category: TIL/강병호/2026/02 (파일 경로 : TIL/{이름}/{연}/{월})
 layout: post (자유)
 ---


## 1. 문제 상황

JPA에서 Fetch Join과 페이징(Pageable)을 함께 사용할 경우, 의도와 달리 DB 레벨이 아닌 애플리케이션 메모리에서 페이징이 처리될 수 있다.

이 경우 다음과 같은 문제가 발생한다.

* 전체 데이터를 한 번에 조회
* 애플리케이션 메모리에서 페이징 처리
* 데이터 양이 많을 경우 OutOfMemoryError(OOM) 발생 가능

---

## 2. 왜 이런 문제가 발생할까?

### 원인: 컬렉션 Fetch Join

`@OneToMany`와 같은 컬렉션 연관관계에 Fetch Join을 사용하면 조인으로 인해 결과 row 수가 증가한다.

이때 JPA는 다음과 같이 동작한다.

1. 조인된 결과를 모두 조회
2. 중복 엔티티를 제거
3. 메모리에서 페이징 적용

즉,

* DB에서 limit/offset으로 자르는 것이 아니라
* 애플리케이션에서 전체 조회 후 잘라내는 방식으로 동작한다

이 때문에 데이터가 많아질수록 메모리 사용량이 급격히 증가한다.

---

## 3. 문제가 되는 예시

```java
@Query("select o from Order o join fetch o.orderItems")
Page<Order> findAllWithItems(Pageable pageable);
```

컬렉션 Fetch Join과 Pageable을 함께 사용하는 전형적인 위험 코드다.

---

## 4. 해결 방법

### 4-1. Fetch Join 제거

* DB 레벨 페이징 정상 동작
* 하지만 지연 로딩으로 인해 N+1 문제가 발생할 수 있다

---

### 4-2. Batch Fetch 전략 사용 (권장)

설정 예시:

```yaml
spring:
  jpa:
    properties:
      hibernate:
        default_batch_fetch_size: 100
```

또는

```java
@BatchSize(size = 100)
```

이 방식의 장점은 다음과 같다.

* DB에서 정상적으로 페이징 처리
* N+1 문제 완화
* 메모리 과부하 방지

---

## 5. 정리

* 컬렉션 Fetch Join + Pageable 조합은 메모리 페이징을 유발한다
* 단일 값 연관관계(`@ManyToOne`, `@OneToOne`) Fetch Join은 비교적 안전하다
* 실무에서는 Batch Fetch 전략을 사용하는 것이 안정적이다

---

## 결론

컬렉션 Fetch Join과 페이징은 함께 사용하지 않는 것이 좋다.
대신 Batch Fetch 전략을 활용해 N+1 문제와 메모리 문제를 함께 해결하는 방향이 바람직하다.
