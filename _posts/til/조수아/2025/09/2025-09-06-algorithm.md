---
title: 2025-09-06
author: 조수아
date: 2025-09-06
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

# 멀티소스 다익스트라(Multi-Source Dijkstra) 정리

## 개념 한 줄 요약

여러 **시작 정점**(source)에서 동시에 출발해, **모든 정점까지의 최단거리**를 한 번에 구하는 다익스트라 변형.

핵심은 **우선순위큐 초기화 시 여러 출발점을 함께 넣고 거리=0**으로 시작하는 것.

---

## 언제 쓰나

- 가장 가까운 편의시설/거점까지의 거리(가장 가까운 커피숍, 병원, 서버 등)
- 여러 진입점/포털이 있는 그래프의 최소 이동 비용
- 역방향 그래프에서 “가장 가까운 목적지” 계산(도착 집합을 소스로 두고 역변환)

---

## 전제 조건

- 간선 가중치 **비음수(≥0)**
- 그래프: 방향/무방향 모두 가능
- 자료구조: `dist[]`, `parent[]`(경로 복원 필요시), `PriorityQueue`(min-heap)

---

## 알고리즘 아이디어

1. `dist[]`를 `INF`로 초기화.
2. **모든 소스 s ∈ S**에 대해 `dist[s]=0`, `pq.offer( (0, s) )`.
3. 일반 다익스트라처럼 팝/릴랙스 반복.
4. 종료 후 `dist[v]` = v로의 최단거리(소스 중 하나로부터).

---

## 의사코드

```
MultiSourceDijkstra(G=(V,E), w, S):
    for v in V: dist[v] = INF, parent[v] = -1
    pq = min-heap

    for s in S:
        dist[s] = 0
        parent[s] = s           // 필요 시 자기 자신으로 표기
        pq.push( (0, s) )

    while pq not empty:
        (d, u) = pq.pop()
        if d != dist[u]: continue         // stale entry skip

        for (u -> v, w_uv) in adj[u]:
            nd = d + w_uv
            if nd < dist[v]:
                dist[v] = nd
                parent[v] = u
                pq.push( (nd, v) )

    return dist, parent

```

---

## 시간/공간 복잡도

- 시간: **O((N + M) log N)** (N=정점 수, M=간선 수)
- 공간: **O(N + M)**