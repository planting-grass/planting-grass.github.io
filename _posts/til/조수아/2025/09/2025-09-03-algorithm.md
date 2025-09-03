---
title: 2025-09-03
author: 조수아
date: 2025-09-03
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것

## Trie

### 개념
- 문자열 탐색 트리(Prefix Tree)
- 문자열들의 공통 접두사(prefix)를 기준으로 트리 형태로 저장하는 자료구조
- 검색/삽입 시간이 문자열 길이에 비례 -> O(문자열 길이)

### 구조
- 노드 구성
    - childlen: 문자 -> 다음 노드 (보통 배열 또는 HashMap)
    - isEnd : 단어의 끝 여부 표시
- 루트 노드 : 공백 노드(문자 없음)
> root
 ├─ c
 │   └─ a
 │       ├─ t (end)
 │       └─ r (end)
 └─ d
     └─ o
         └─ g (end)

### 주요 기능
1. insert(word)
   - 문자열 삽입
   - 없으면 새 노드 생성, 끝에 isEnd = true
2. search(word)
    - 문자열 전체가 존재하는지 확인
    - 루트에서 문자 순서대로 내려가며 탐색, 마지막 노드의 isEnd가 true인지 체크
3. startsWith(prefix)
    - 특정 접두사로 시작하는 단어가 있는지 확인
    - 문자열 전체가 끝까지 탐색만 되면 true
  
### 시간/공간 복잡도
- 삽입/검색 : O(L) (L : 문자열 길이)
- 공가 : 최악의 경우 모든 문자열의 합 길이 만큼 노드 필요 -> 공간 소모 큼

### 코드
```java
import java.util.HashMap;
import java.util.Map;

class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord;
}

class Trie {
    private final TrieNode root = new TrieNode();

    // 단어 삽입
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            node = node.children.computeIfAbsent(c, k -> new TrieNode());
        }
        node.isEndOfWord = true;
    }

    // 단어 검색
    public boolean search(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (!node.children.containsKey(c)) return false;
            node = node.children.get(c);
        }
        return node.isEndOfWord;
    }

    // 접두사 검색
    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            if (!node.children.containsKey(c)) return false;
            node = node.children.get(c);
        }
        return true;
    }
}
```

## 세그먼트 트리

### 개념
- 구간 질의와 구간 업데이트를 효율적으로 처리하기 위한 이진 트리 기반 자료구조
- 배열 크기를 N이라 할 때
  - 구간 합 / 최솟값 / 최댓값 / gcd 등 다양한 질의 가능
  - 단순 누적합은 업데이트 O(N)이 걸리지만, 세그트리는 O(logN)
  
### 시간 복잡도
- 트리 빌드 : O(N)
- 구간 질의 : O(logN)
- 원소 업데이트 : O(logN)
  
### 구조
- 세그먼트 트리는 보통 배열 기반 이진트리로 구현
- 크기 : tree[4*N]
- 루트 노드 : 구간 [1...N]
- 왼쪽 자식 : [l..mid]
- 오른쪽 자식 : [mid+1..r]

### 기본 연산

1. Build
```java
void build(int node, int start, int end) {
    if (start == end) {
        tree[node] = arr[start];
    } else {
        int mid = (start + end) / 2;
        build(node*2, start, mid);
        build(node*2+1, mid+1, end);
        tree[node] = tree[node*2] + tree[node*2+1];
    }
}
```
2. Query (구간합)
```java
int query(int node, int start, int end, int l, int r) {
    if (r < start || end < l) return 0;          // 겹치지 않음
    if (l <= start && end <= r) return tree[node]; // 완전히 포함
    int mid = (start + end) / 2;
    return query(node*2, start, mid, l, r)
         + query(node*2+1, mid+1, end, l, r);
}

```
3. Update (단일값 갱신)
```java
void update(int node, int start, int end, int idx, int val) {
    if (start == end) {
        tree[node] = val;
    } else {
        int mid = (start + end) / 2;
        if (idx <= mid) update(node*2, start, mid, idx, val);
        else update(node*2+1, mid+1, end, idx, val);
        tree[node] = tree[node*2] + tree[node*2+1];
    }
}
```

### 코드
```java
class SegmentTree {
    int[] arr;       // 원본 배열
    int[] tree;      // 세그먼트 트리
    int n;

    SegmentTree(int[] input) {
        n = input.length;
        arr = input;
        tree = new int[4 * n];
        build(1, 0, n-1);
    }

    void build(int node, int start, int end) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(node*2, start, mid);
            build(node*2+1, mid+1, end);
            tree[node] = tree[node*2] + tree[node*2+1];
        }
    }

    int query(int l, int r) { return query(1, 0, n-1, l, r); }

    int query(int node, int start, int end, int l, int r) {
        if (r < start || end < l) return 0;
        if (l <= start && end <= r) return tree[node];
        int mid = (start + end) / 2;
        return query(node*2, start, mid, l, r) +
               query(node*2+1, mid+1, end, l, r);
    }

    void update(int idx, int val) { update(1, 0, n-1, idx, val); }

    void update(int node, int start, int end, int idx, int val) {
        if (start == end) {
            tree[node] = val;
        } else {
            int mid = (start + end) / 2;
            if (idx <= mid) update(node*2, start, mid, idx, val);
            else update(node*2+1, mid+1, end, idx, val);
            tree[node] = tree[node*2] + tree[node*2+1];
        }
    }
}
```

### 확장 (Lazy Propagation)
- range update (ex: 구간에다가 +K)필요 -> lazy 배열 추가
- push 연산으로 필요할 때만 자식에게 갱신 전파
- 시간복잡도는 그대로 O(logN) 유지