---
title: 2025-09-16
author: 조수아
date: 2025-09-16
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

# 📘 페르마의 소정리 (Fermat's Little Theorem)

## 1. 정리의 내용

- **정수 a, 소수 p** 에 대해:
    
    a^p ≡ a (mod p)
    
- 특히, a가 p로 나누어지지 않을 때:
    
    a^(p−1) ≡ 1 (modp)
    

## 2. 활용

1. **소수 판정**
    
    페르마 소수 판정법으로 활용할 수 있음. (단, 카마이클 수 같은 예외 존재)
    
2. **모듈러 역원 계산**
    - 어떤 수 aaa의 모듈러 역원은 a^−1 ≡ a^(p−2) (mod p) 로 구할 수 있음.
    - 즉, 나눗셈 연산을 곱셈과 거듭제곱으로 바꿔줌.

## 3. 알고리즘 포인트

- 빠른 거듭제곱 (Fast Exponentiation, Binary Exponentiation)을 사용해야 함.
- 시간 복잡도: O(log⁡p)O(\log p)O(logp)

# **🖥️ 자바 템플릿 코드**

```java
import java.io.*;
import java.util.*;

public class FermatLittleTheorem {
    
    // 빠른 거듭제곱 (a^b % mod)
    static long modPow(long a, long b, long mod) {
        long result = 1;
        a %= mod;
        while (b > 0) {
            if ((b & 1) == 1) result = (result * a) % mod;
            a = (a * a) % mod;
            b >>= 1;
        }
        return result;
    }

    // 모듈러 역원 (a^(p-2) % p)  단, p는 소수
    static long modInverse(long a, long p) {
        return modPow(a, p - 2, p);
    }

    public static void main(String[] args) throws Exception {
        long a = 5;
        long p = 13; // 소수

        // Fermat's little theorem 확인
        System.out.println("a^p ≡ a (mod p) : " + modPow(a, p, p) + " == " + (a % p));

        // 모듈러 역원 구하기 (5의 역원 mod 13)
        long inv = modInverse(a, p);
        System.out.println("Inverse of " + a + " mod " + p + " = " + inv);

        // 검증 (a * a^{-1} % p == 1)
        System.out.println("Check: " + (a * inv % p));
    }
}
```

# 📘 TSP (Traveling Salesman Problem, 외판원 순회 문제)

## 1. 문제 정의

- N개의 도시와 도시 간 이동 비용이 주어질 때, 한 도시에서 출발해서 모든 도시를 한 번씩 방문하고 다시 출발 도시로 돌아오는 최소 비용 경로를 찾는 문제.
- 조합 최적화 문제의 대표적인 예시이며, NP-Hard 문제로 알려져 있음.

---

## 2. 해결 방법

### (1) 완전 탐색

- 모든 도시 순서를 나열하는 순열을 만들어 최소 비용을 찾음.
- 시간 복잡도는 N! 이므로 N이 10 이하일 때만 가능.

### (2) 동적 계획법(DP) + 비트마스크 (Held-Karp 알고리즘)

- 핵심 아이디어: "현재 도시"와 "지금까지 방문한 도시 집합"을 상태로 정의.
- `dp[visited][cur]` = 지금까지 방문한 도시 집합이 `visited`일 때, 현재 도시가 `cur`일 경우의 최소 비용.
- 이전에 방문한 도시들을 하나씩 확인하면서 최적해를 갱신해 나감.
- 시간 복잡도는 N제곱 곱하기 2의 N승. 즉, 대략 O(N^2 * 2^N).
- 이 방식은 N이 16~20 정도까지 가능.

---

## 3. 알고리즘 흐름

1. 출발 도시를 0번으로 고정.
2. 방문한 도시 집합을 비트마스크(정수)로 표현.
    - 예: 0110 → 도시 1과 2를 방문한 상태.
3. 모든 도시를 다 방문했을 때 출발 도시로 돌아오는 비용까지 계산.
4. 모든 경우를 탐색하면서 dp 테이블을 채워나가 최소 비용을 찾음.

---

## 4. 자바 템플릿 코드

```java
import java.io.*;
import java.util.*;

public class TSP_DP {
    static final int INF = 1_000_000_000;
    static int N;
    static int[][] cost, dp;

    static int tsp(int visited, int cur) {
        // 모든 도시 방문 완료
        if (visited == (1 << N) - 1) {
            return (cost[cur][0] == 0) ? INF : cost[cur][0];
        }

        if (dp[visited][cur] != -1) return dp[visited][cur];

        int ans = INF;
        for (int next = 0; next < N; next++) {
            if ((visited & (1 << next)) != 0 || cost[cur][next] == 0) continue;
            ans = Math.min(ans, tsp(visited | (1 << next), next) + cost[cur][next]);
        }

        return dp[visited][cur] = ans;
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        cost = new int[N][N];
        dp = new int[1 << N][N];

        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                cost[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        for (int[] row : dp) Arrays.fill(row, -1);

        System.out.println(tsp(1, 0)); // 0번 도시에서 출발
    }
}

```

---

## 5. 핵심 요약

- TSP는 **순열 완전 탐색**으로는 N이 작을 때만 가능.
- 실질적으로는 **DP + 비트마스크** 방식이 가장 많이 쓰임.
- DP 정의: "방문 집합"과 "현재 도시" 조합으로 최소 비용을 저장.
- 시간 복잡도는 N제곱 곱하기 2의 N승.
- 보통 N이 16~20일 때까지 풀 수 있음.