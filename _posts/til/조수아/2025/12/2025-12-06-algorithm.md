---
title: 2025-12-06
author: 조수아
date: 2025-12-06
category: TIL/조수아/2025/12
layout: post
---

# 오늘 배운 것

# ✅ 1. **문자열(String) 관련 헷갈리는 메서드 정리**

## ✔ 부분 문자열

```java
s.substring(start);         // start ~ 끝
s.substring(start, end);    // start ~ end-1
```

## ✔ 문자열 분리

```java
String[] arr = s.split(" ");
String[] arr = s.split("\\."); // 정규식이라 .은 이렇게 이스케이프
```

## ✔ 문자열 합치기

```java
String joined = String.join(",", list);
```

## ✔ 문자열 ↔ 숫자

```java
int x = Integer.parseInt("123");
String s = String.valueOf(123);
```

---

# ✅ 2. **정렬(Sort) — 배열 vs 컬렉션 완전 정리**

# 🔹 A) **배열 정렬**

### ✔ 오름차순

```java
Arrays.sort(arr);
```

### ✔ 내림차순 (int 배열 직접 불가 → 박싱 필요)

```java
Integer[] arr = {1,2,3};
Arrays.sort(arr, Collections.reverseOrder());   // 내림차순
```

---

# 🔹 B) **List 정렬**

### ✔ 오름차순

```java
Collections.sort(list);
```

### ✔ 내림차순

```java
list.sort(Collections.reverseOrder());
```

### ✔ 사용자 정의 비교

```java
list.sort((a,b) -> a - b);          // 오름차순
list.sort((a,b) -> b - a);          // 내림차순
```

---

# 🔹 C) **정렬 시 실수하기 쉬운 포인트**

- `Collections.reverseOrder()`는 **List, Integer[]**만 가능
- **int[]**에는 reverseOrder() 못 씀
- int[] → Integer[]로 변환해야 함

---

# ✅ 3. **Arrays.sort()와 Collections.sort() 차이**

| 구분 | Arrays.sort | Collections.sort |
| --- | --- | --- |
| 대상 | 배열 | List |
| 기본 정렬 | Dual-Pivot QuickSort | TimSort |
| 내림차순 | 바로 불가(int) | 가능 |
| 속도 | 배열에 최적화 | 안정적, 실무에서 가장 많이 씀 |

---

# ✅ 4. **배열 ↔ 컬렉션 변환**

# 🔹 A) 배열 → List

### ✔ 1) 문자열/객체 배열

```java
String[] arr = {"a","b"};
List<String> list = Arrays.asList(arr); // 크기 변경 불가 리스트 값변경은 되는데 크기 변경은 안됨
List<String> list2 = new ArrayList<>(Arrays.asList(arr)); // 변경 가능
```

### ✔ 2) int[] → List<Integer>

```java
int[] arr = {1,2,3};
List<Integer> list = Arrays.stream(arr).boxed().toList();
```

---

# 🔹 B) List → 배열

### ✔ 문자열/객체

```java
String[] arr = list.toArray(new String[0]);
```

### ✔ Integer List → int[]

```java
int[] arr = list.stream().mapToInt(i -> i).toArray();
```

---

# 🔹 C) Collection 깊은 복사 / 얕은 복사

### ✔ List 깊은 복사(원시 타입)

```java
List<Integer> copy = new ArrayList<>(original);
```

### ✔ 배열 깊은 복사

```java
int[] copy = Arrays.copyOf(arr, arr.length);
```

---

# ✅ 5. **우선순위 큐(PriorityQueue) 정리**

## 🔹 오름차순 (기본)

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
```

## 🔹 내림차순

```java
PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
```

## 🔹 사용자 정의 PQ

```java
PriorityQueue<int[]> pq = new PriorityQueue<>(
    (a, b) -> a[1] - b[1]         // a[1] 기준 오름차순
);
```

---

# ✅ 6. **역순(Reversing) 정리**

# 🔹 A) 배열 역순

```java
Collections.reverse(list);   // List만 사용 가능
```

배열이면:

```java
int[] arr = {1,2,3};
for (int i=0; i<arr.length/2; i++) {
    int temp = arr[i];
    arr[i] = arr[n-1-i];
    arr[n-1-i] = temp;
}
```

---

# 🔹 B) 리스트 역순

```java
Collections.reverse(list);
```

---

# 🔹 C) 역순 정렬 (reverseOrder와 다름!)

- reverseOrder = 정렬 기준의 반대
- reverse = 순서를 뒤집기

둘 완전 다름

혼동하지 말기.

---

# ✅ 7. **배열 / 리스트 복사 총정리**

| 작업 | 코드 |
| --- | --- |
| 배열 깊은복사 | `Arrays.copyOf(arr, arr.length)` |
| 2D 배열 깊은복사 | 반복문으로 row별 copy 필요 |
| List 복사 | `new ArrayList<>(original)` |
| List 깊은복사(객체) | 각 요소 clone 필요 |
| int[] → List<Integer> | `Arrays.stream(arr).boxed().toList()` |
| List<Integer> → int[] | `list.stream().mapToInt(i->i).toArray()` |

# ✅ 0. 스트림 기본 흐름 (딱 1줄로 끝)

```
데이터 → stream() → 중간연산(여러 개) → 최종연산(1개)
```

---

# ✅ 1. 스트림 생성

## ✔ List → Stream

```java
list.stream()
```

## ✔ 배열 → Stream

```java
Arrays.stream(arr)       // int[], double[], long[] 전용
```

## ✔ 범위 Stream

```java
IntStream.range(0, n);        // 0~n-1
IntStream.rangeClosed(1, n);  // 1~n
```

---

# ✅ 2. 자주 쓰는 중간 연산

## ✔ map (값 변환)

```java
list.stream().map(x -> x * 2);
```

## ✔ filter (조건 필터링)

```java
list.stream().filter(x -> x > 10);
```

## ✔ distinct (중복 제거)

```java
list.stream().distinct();
```

## ✔ sorted (정렬)

기본 오름차순:

```java
list.stream().sorted();
```

내림차순:

```java
list.stream().sorted(Comparator.reverseOrder());
```

## ✔ limit / skip

```java
stream.limit(5);    // 앞 5개만
stream.skip(3);     // 앞 3개 건너뛰기
```

---

# ✅ 3. 최종 연산 (반드시 하나만)

## ✔ forEach

```java
list.stream().forEach(System.out::println);
```

## ✔ sum / max / min

```java
int sum = arr.stream().mapToInt(x->x).sum();
int max = arr.stream().mapToInt(x->x).max().orElse(0);
```

## ✔ collect List 만들기

```java
List<Integer> result = list.stream().collect(Collectors.toList());
```

## ✔ count

```java
long cnt = list.stream().filter(x->x>5).count();
```

---

# ✅ 4. 가장 헷갈리는 “타입 변환”

## ✔ int[] → List<Integer>

```java
List<Integer> list = Arrays.stream(arr).boxed().toList();
```

## ✔ List<Integer> → int[]

```java
int[] arr = list.stream().mapToInt(i -> i).toArray();
```

## ✔ String[] → List<String>

```java
List<String> list = Arrays.asList(arr);
```

## ✔ List<String> → String[]

```java
String[] arr = list.toArray(new String[0]);
```

---

# ✅ 5. 정렬 관련 스트림 패턴

## ✔ 객체 정렬 (람다)

```java
list.stream()
    .sorted((a,b) -> a.age - b.age)
    .toList();
```

## ✔ 다중 조건 정렬

```java
list.stream()
    .sorted(
        Comparator.comparing(Person::getAge)                // age 오름차순
        .thenComparing(Person::getName, Comparator.reverseOrder()) // name 내림차순
    )
    .toList();

```

---

# ✅ 6. 자주 쓰는 Flatten (2차원 → 1차원)

## ✔ List<List<Integer>> → List<Integer>

```java
list.stream()
    .flatMap(inner -> inner.stream())
    .toList();
```

---

# ✅ 7. 그룹핑 (코테에서는 가끔 등장)

```java
Map<String, Long> map =
    list.stream()
        .collect(Collectors.groupingBy(s -> s, Collectors.counting()));
```

---

# ✅ 8. 실전에서 가장 많이 쓰는 패턴 TOP 5

## 1) max/min 구하기

```java
int max = list.stream().mapToInt(i->i).max().orElse(0);
```

## 2) 조건 카운트

```java
long c = list.stream().filter(x->x>10).count();
```

## 3) 정렬 후 List로 받기

```java
List<Integer> sorted = list.stream().sorted().toList();
```

## 4) 배열 ↔ 리스트 변환

```java
List<Integer> list = Arrays.stream(arr).boxed().toList();
int[] arr = list.stream().mapToInt(i->i).toArray();
```

## 5) map 변환

```java
List<String> names = users.stream().map(User::getName).toList();
```

### ✔ 리스트/배열 정렬할 때 → `Collections.reverseOrder()`

### ✔ 스트림/Comparator 체인에서 → `Comparator.reverseOrder()`