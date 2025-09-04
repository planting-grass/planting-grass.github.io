---
title: 2025-09-05
author: 조수아
date: 2025-09-05
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

## 좌표 해싱

### 방법
2가지 방법으로 좌표를 정의해서 HashSet이나 HashMap에 활용
1. 2D -> 1D
```
// (r, c) → long
static long encode(int r, int c) {
    return (((long) r) << 32) ^ (c & 0xffffffffL);
}

// 다시 풀어내기
static int decodeRow(long key) {
    return (int)(key >> 32);
}
static int decodeCol(long key) {
    return (int)(key & 0xffffffffL);
}
```

2. 커스텀 객체
```
static class Point {
    int r, c;
    Point(int r, int c) { this.r = r; this.c = c; }
    @Override public boolean equals(Object o) {
        if (!(o instanceof Point)) return false;
        Point p = (Point) o;
        return r == p.r && c == p.c;
    }
    @Override public int hashCode() {
        return Objects.hash(r, c);
    }
}
```
- 반드시 equals랑 hashCode 재정의해줘야됌

## 공간해싱

### 맨해튼 거리
- 두 점 (x1, y1), (x2, y2) 사이 거리:
```
dist = |x1 - x2| + |y1 - y2|
```
- 격자에서 상하좌우만 이동할 수 있는 경우의 최소 이동 거리와 같음.

### 1) 핵심 아이디어

- 원점 좌표 (x, y) → 셀 좌표 (cx, cy)로 변환:
```
cx = floor(x / L), cy = floor(y / L)
```

- 같은 셀 또는 인접 8셀(총 3×3) 안에만 후보가 있으므로, 전 공간이 아니라 3×3 셀만 뒤져서 후보를 모으고, 마지막에 정확 거리 조건(맨해튼 ≤ D) 로 필터링.

- 왜 3×3이면 충분?
- 셀 한 변이 L일 때, 중심 점에서 맨해튼 거리 D ≤ L 로 제한하면, 그 범위에 들어올 수 있는 다른 점은 같은 셀 또는 인접 셀에만 존재 (그 밖의 셀에 있는 점은 최소 맨해튼 거리가 > L ≥ D 가 됨)
- 팁: L = D 로 두면 3×3 이웃만 보면 충분하고, 더 큰 반경 kD를 보고 싶다면 이웃 범위를 ±k 로 확장.

### 2) 안전한 키 변환 (음수 좌표 포함)

- 자바 정수 나눗셈은 음수에 취약하니, Math.floorDiv 를 꼭 사용
```
// 셀 크기 L로 (x,y) → (cx,cy) 변환
static int cell(int x, int L) { 
    return Math.floorDiv(x, L); // 음수 안전
}

// (cx, cy) → long 키 (충돌 거의 없음)
static long bucketKey(int cx, int cy) {
    return (((long) cx) << 32) ^ (cy & 0xffffffffL);
}
```
### 3) 자료구조 설계 (버킷 맵)

- 각 셀 키 → 해당 셀의 점 목록(또는 ID 목록)을 저장
```
Map<Long, List<int[]>> buckets = new HashMap<>();
// 점 (x,y) 삽입
void addPoint(int x, int y, int L) {
    int cx = cell(x, L), cy = cell(y, L);
    long key = bucketKey(cx, cy);
    buckets.computeIfAbsent(key, k -> new ArrayList<>()).add(new int[]{x, y});
}
```
### 4) 맨해튼 거리 D 이내 질의 (3×3만 확인)

```
// 맨해튼 거리 D 이내에 있는 점들을 찾기 (L = D 권장)
List<int[]> queryManhattan(int qx, int qy, int D, int L, Map<Long, List<int[]>> buckets){
    int cqx = cell(qx, L), cqy = cell(qy, L);
    List<int[]> res = new ArrayList<>();
    for (int dx = -1; dx <= 1; dx++){
        for (int dy = -1; dy <= 1; dy++){
            long key = bucketKey(cqx + dx, cqy + dy);
            List<int[]> cand = buckets.get(key);
            if (cand == null) continue;
            for (int[] p : cand){
                int dist = Math.abs(p[0] - qx) + Math.abs(p[1] - qy);
                if (dist <= D) res.add(p); // 최종 정확 검사
            }
        }
    }
    return res;
}
```
- 포인트: 후보는 3×3 셀에서만 수집, 마지막에 맨해튼 거리로 필터링해서 오차 없이 정답만 남김.
- 반경을 D가 아니라 2D, 3D처럼 더 넓게 보고 싶으면, 이웃 루프를 dx,dy ∈ [-k, k] 로 늘리고 마지막 조건을 dist ≤ k*D로 검사하면 됨.

## 페르마의 소정리 & 모듈러 연산

### 1) 모듈러 연산

- a ≡ b (mod m) → “a와 b를 m으로 나눈 나머지가 같다”는 의미

### 2) 모듈러 성질

>
- *(a + b) % m = ((a % m) + (b % m)) % m*
- *(a - b) % m = ((a % m) - (b % m) + m) % m* (음수 보정 필요!)
- *(a × b) % m = ((a % m) × (b % m)) % m*
- *(a^k) % m = 빠른 거듭제곱(분할정복)* 으로 구해야 효율적 (O(log k))

### 3) 모듈러 곱셈 역원

- 나눗셈은 모듈러에서 직접 안 됨 → 역원 이용
- *a x a^(-1) ≡ 1 (mod m)*
- 즉, *b / a % m* 을 하고 싶으면 → *b × a^(-1) % m* 으로 바꿔야 함

### 4) 페르마의 소정리

- 정리: p가 소수이고, a가 p로 나누어 떨어지지 않는 정수일 때
```
a^(p-1) ≡ 1 (mod p)
```
- 따라서
```
a^(p-2) ≡ a^(-1) (mod p)
```

### 5) 빠른 거듭 제곱

```
static long modPow(long a, long b, long mod){
    long res = 1;
    a %= mod;
    while(b > 0){
        if ((b & 1) == 1) res = (res * a) % mod;
        a = (a * a) % mod;
        b >>= 1;
    }
    return res;
}
```

### 6) 페르마 역원 구하기

```
// a의 역원 구하기 (mod p), p는 소수
static long modInverse(long a, long p){
    return modPow(a, p-2, p); // 페르마의 소정리
}
```