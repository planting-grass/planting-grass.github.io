---
title: 2025-09-15
author: 조수아
date: 2025-09-15
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

## 트리 DP

### 개념

- 트리 DP는 그래프 DP의 특수 케이스로, 사이클이 없는 트리 구조에서만 적용
- 각 노드를 루트로 잡아 서브트리에 대한 값을 정의하고, 부모-자식 관계를 이용해 점화식을 세움
- 보통 DFS 탐색과 함께 계산

### 대표적인 트리 DP 유형

- 서브 트리 크기 구하기
  - 정의 : `dp[u] = u를 루트로 하는 서브트리의 크기`
  - 점화식 : `dp[u]=1+∑​dp[v]` (v∈child(u))
- 독립 집합 문제 (트리 위에서의 최대 독립 집합)
  - 정의 : dp[u][0] = u를 선택하지 않을 때 최대, dp[u][1] = u를 선택했을 때 최대
  - 점화식 :
  - `dp[u][0] = Σ max(dp[v][0], dp[v][1])`
  - `dp[u][1] = 1 + Σ dp[v][0]`
- 트리의 지름
  - 정의 : dp[u] = u를 루트로 하는 서브트리에서 가장 긴 경로
  - 계산 : 각 노드에서 자식들의 최댓값 2개를 합쳐 최대 지름 후보를 갱신
- 루트 변경 트릭
  - 트리 DP에서 루트를 바궈가며 전체 답을 구하는 방식
  - 예 : 각 노드를 루트로 했을 때 서브트리 크기, 경로 길이 등을 계산

### 자바 템플릿

```
import java.io.*;
import java.util.*;

public class TreeDP {
    static int N;
    static List<Integer>[] g;
    static int[][] dp; 
    static boolean[] visited;

    // 예시: 트리 독립집합 (dp[u][0] = 선택X, dp[u][1] = 선택O)
    static void dfs(int u) {
        visited[u] = true;
        dp[u][0] = 0; 
        dp[u][1] = 1; // 자기 자신 선택

        for (int v : g[u]) {
            if (!visited[v]) {
                dfs(v);
                dp[u][0] += Math.max(dp[v][0], dp[v][1]); // u 선택X → v 자유
                dp[u][1] += dp[v][0]; // u 선택O → v는 선택 불가
            }
        }
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        g = new ArrayList[N+1];
        dp = new int[N+1][2];
        visited = new boolean[N+1];

        for (int i = 1; i <= N; i++) g[i] = new ArrayList<>();

        // 간선 입력
        for (int i = 0; i < N-1; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            g[a].add(b);
            g[b].add(a);
        }

        dfs(1); // 1번을 루트로 DFS
        System.out.println(Math.max(dp[1][0], dp[1][1])); // 전체 최댓값
    }
}
```