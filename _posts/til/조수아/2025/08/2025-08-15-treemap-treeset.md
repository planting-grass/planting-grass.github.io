---
title: 2025-08-15
author: 조수아
date: 2025-08-15
category: TIL/조수아/2025/08
layout: post
---

# 오늘 배운 것

## 자료구조
- 성적조회하기 문제를 해결할 때 TreeMap 자료구조를 통해 구현하면 시간 복잡도를 감소 시킬 수 있었다.
- TreeMap과 TreeSet 모두 정렬된 자료구조를 제공하고 둘다 내부적으로 레드 블랙 트리 기반으로 구현되어 있다.
    - 따라서 O(logN) 시간 복잡도로 탐색, 삽입, 삭제가 가능핟.
- 데이터 조회, 삽입, 삭제가 빈번하게 사용되는 경우 해당 자료구조의 사용을 고려해봐야한다.

### TreeMap
- 개념
    - Key-Value 쌍을 저장하는 Map 인터페이스 구현체
    - 키를 기준으로 오름차순 정렬 (기본: Comparable 기준, 또는 Comparator 지정 가능)
    - Null 키는 허용하지 않음, 값은 null 가능
- 주요 특징
    - 자동 정렬: 삽입할 때마다 키 순서대로 정렬됨
    - 범위 검색: subMap, headMap, tailMap 메서드로 부분 조회 가능
    - 탐색 메서드:
        - firstKey() / lastKey() : 가장 작은/큰 키
        - ceilingKey(k) : k 이상인 최소 키
        - floorKey(k) : k 이하인 최대 키

### TreeSet
- 개념
    - 중복 없는 데이터를 저장하는 Set 인터페이스 구현체
    - 원소를 정렬 상태로 유지
    - 내부적으로 TreeMap을 키만 사용해서 구현됨
    - null 저장 불가 (Comparator가 null을 허용하지 않음)
- 주요 특징
    - 자동 정렬: 삽입 시 정렬 유지
    - 범위 검색: subSet, headSet, tailSet
    - 탐색 메서드:
        - first() / last() : 가장 작은/큰 값
        - ceiling(e) : e 이상 최소값
        - floor(e) : e 이하 최대값