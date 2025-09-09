---
title: 2025-09-09
author: 조수아
date: 2025-09-09
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

## 최소 공통 조상 (LCA, Lowest Common Ancestor)

- 최소 공통 조상 : 트리 구조에서 임의의 두 정점이 갖는 가장 가까운 조상 정점

### 알고리즘

- 전처리
  - depth[v] : 루트로부터 몇 칸 내려왔는지
  - up[k][v] : 노드 v 에서 2^k칸 위에 있는 조상 저장 없으면 0
- 질의
  1. 깊이 맞추기
    - 두 노드 중 더 깊은 쪽을 위로 올려서 둘의 depth를 똑같이 맞춤
  2. 같은 조상 만날 때까지 점프
    - 깊이가 같아졌으면, 제일 큰 점프부터 내려오면서 `up[k][u] != up[k][v]`인 동안 둘 다 점프
  3. 부모가 LCA
    - 마지막에 남은 부모가 둘의 최소 공통 조상
- 복잡도
  - 전처리 : O(NlogN)
  - 질의 : O(logN)

### 코드
```
import java.io.*;
import java.util.*;

public class Main {
    static int N, Q, LOG;
    static List<Integer>[] g;
    static int[][] up;       // up[k][v] = v의 2^k번째 조상 (없으면 0)
    static int[] depth;

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        Q = Integer.parseInt(st.nextToken());

        g = new ArrayList[N + 1];
        for (int i = 1; i <= N; i++) g[i] = new ArrayList<>();

        for (int i = 0; i < N - 1; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            g[a].add(b); g[b].add(a);
        }

        LOG = 1;
        while ((1 << LOG) <= N) LOG++;
        up = new int[LOG][N + 1];
        depth = new int[N + 1];

        // 전처리: root = 1
        dfs(1, 0);
        for (int k = 1; k < LOG; k++) {
            for (int v = 1; v <= N; v++) {
                int mid = up[k - 1][v];
                up[k][v] = (mid == 0) ? 0 : up[k - 1][mid];
            }
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < Q; i++) {
            st = new StringTokenizer(br.readLine());
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            sb.append(lca(u, v)).append('\n');
        }
        System.out.print(sb);
    }

    // 부모 p에서 자식 v로 내려가며 depth와 up[0] 채움 (재귀 DFS)
    static void dfs(int v, int p) {
        up[0][v] = p;
        depth[v] = (p == 0) ? 0 : depth[p] + 1;
        for (int to : g[v]) if (to != p) dfs(to, v);
    }

    // LCA(u, v)
    static int lca(int u, int v) {
        if (depth[u] < depth[v]) { int t = u; u = v; v = t; }
        // 1) 깊이 맞추기
        int diff = depth[u] - depth[v];
        for (int k = 0; k < LOG; k++) if (((diff >> k) & 1) == 1) u = up[k][u];
        if (u == v) return u;
        // 2) 동시에 점프 (바로 위가 다를 때까지)
        for (int k = LOG - 1; k >= 0; k--) {
            if (up[k][u] != up[k][v]) {
                u = up[k][u];
                v = up[k][v];
            }
        }
        // 3) 부모가 LCA
        return up[0][u];
    }
}

```