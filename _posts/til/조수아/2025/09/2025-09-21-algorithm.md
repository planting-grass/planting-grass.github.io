---
title: 2025-09-21
author: 조수아
date: 2025-09-21
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

# ⭐ A*

## 1. 개념

- **A***(A-star) 알고리즘은 **최단 경로 탐색 알고리즘**으로, **다익스트라(Dijkstra)**와 **휴리스틱 탐색(Heuristic Search)**를 결합한 방식.
- 그래프에서 **출발점 → 목표점**까지 가는 최적 경로를 효율적으로 찾음.
- **휴리스틱(Heuristic)**: 현재 상태에서 목표까지의 "추정 비용".
    
    → 탐색 범위를 줄이고, 빠른 경로 탐색 가능.
    

---

## 2. 공식

A*는 다음 함수 `f(n)`을 최소화하는 경로를 선택:

```
f(n) = g(n) + h(n)

```

- `g(n)` : 시작점 → 현재 노드까지 실제 비용
- `h(n)` : 현재 노드 → 목표 노드까지의 **추정 비용**(휴리스틱)
- `f(n)` : 총 예상 비용

👉 A*는 **g(n) + h(n)**이 가장 작은 노드부터 탐색.

---

## 3. 휴리스틱 함수 h(n)

휴리스틱은 문제 상황에 따라 달라짐:

- **맨해튼 거리(Manhattan Distance)**: |x1 - x2| + |y1 - y2|
- **유클리드 거리(Euclidean Distance)**: √((x1 - x2)² + (y1 - y2)²)
- **체비쇼프 거리(Chebyshev Distance)**: max(|x1 - x2|, |y1 - y2|)

⚠ 휴리스틱은 **낙관적(Admissible)**이어야 함 → 실제 비용을 절대 초과하면 안 됨.

---

## 4. 알고리즘 과정

1. **시작 노드**를 `Open List`에 넣음
2. `Open List`에서 `f(n)`이 가장 작은 노드를 꺼냄
3. 그 노드의 **이웃 노드들**을 확인
    - g(n) = 현재 노드 g + 이동 비용
    - h(n) = 휴리스틱 값
    - f(n) = g(n) + h(n)
4. 더 최적 경로가 있으면 갱신
5. 목표 노드에 도착하면 경로 복원
6. 도착 못할 경우 Open List가 빌 때까지 반복

---

## 5. 자료구조

- **Open List**: 우선순위 큐(Priority Queue, Min-Heap) → f(n) 최소값 기준
- **Closed List**: 이미 방문/확정된 노드 저장 (HashSet, boolean[][] 등)

---

## 6. 장단점

✅ 장점

- 다익스트라보다 빠름 (불필요한 탐색 줄임)
- 휴리스틱을 잘 고르면 매우 효율적

❌ 단점

- 휴리스틱이 부정확하면 성능 저하
- 메모리 사용량이 많음

---

## 7. 시간 복잡도

- **최악의 경우**: O(E) (다익스트라와 유사)
- **실제는** 휴리스틱 덕분에 훨씬 빠름

---

## 8. 자바 코드 예시

```java
import java.util.*;

class Node implements Comparable<Node> {
    int x, y;
    int g, h, f;
    Node parent;

    Node(int x, int y, int g, int h, Node parent) {
        this.x = x; this.y = y;
        this.g = g; this.h = h;
        this.f = g + h;
        this.parent = parent;
    }

    @Override
    public int compareTo(Node other) {
        return this.f - other.f;
    }
}

public class AStar {
    static int[][] dirs = { {1,0},{-1,0},{0,1},{0,-1} };

    static int manhattan(int x1, int y1, int x2, int y2) {
        return Math.abs(x1-x2) + Math.abs(y1-y2);
    }

    public static List<Node> aStar(int[][] grid, int[] start, int[] goal) {
        int n = grid.length, m = grid[0].length;
        boolean[][] visited = new boolean[n][m];
        PriorityQueue<Node> open = new PriorityQueue<>();

        Node startNode = new Node(start[0], start[1], 0,
                                  manhattan(start[0], start[1], goal[0], goal[1]), null);
        open.add(startNode);

        while(!open.isEmpty()) {
            Node cur = open.poll();

            if(cur.x == goal[0] && cur.y == goal[1]) {
                return reconstructPath(cur);
            }

            visited[cur.x][cur.y] = true;

            for(int[] d : dirs) {
                int nx = cur.x + d[0], ny = cur.y + d[1];
                if(nx<0||ny<0||nx>=n||ny>=m||grid[nx][ny]==1||visited[nx][ny]) continue;

                int g = cur.g + 1;
                int h = manhattan(nx, ny, goal[0], goal[1]);
                Node next = new Node(nx, ny, g, h, cur);
                open.add(next);
            }
        }
        return new ArrayList<>(); // 경로 없음
    }

    private static List<Node> reconstructPath(Node node) {
        List<Node> path = new ArrayList<>();
        while(node != null) {
            path.add(node);
            node = node.parent;
        }
        Collections.reverse(path);
        return path;
    }
}

```