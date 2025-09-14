---
title: 2025-09-11
author: 조수아
date: 2025-09-11
category: TIL/조수아/2025/09
layout: post
---

# 오늘 배운 것


# 📘 KMP 알고리즘 (Knuth–Morris–Pratt Algorithm)

## 1. 개념

- 문자열 탐색 알고리즘 중 하나.
- **텍스트(Text)** `T` 안에서 **패턴(Pattern)** `P`를 효율적으로 찾는 방법.
- 단순 비교 알고리즘(Naive)보다 빠른 시간복잡도를 가짐.
- 불일치가 발생했을 때, 패턴의 앞부분 정보를 활용해 불필요한 비교를 줄임.

---

## 2. 시간 복잡도

- **O(N + M)**
    - `N`: 텍스트 길이
    - `M`: 패턴 길이
- 전처리(부분 일치 테이블 계산) + 탐색 과정 모두 선형 시간에 수행.

---

## 3. 핵심 아이디어

1. **부분 일치 테이블(PI, Prefix Table, LPS 배열)**
    - 패턴 내에서 **접두사(Prefix)와 접미사(Suffix)**가 일치하는 최대 길이를 저장.
    - 불일치가 발생했을 때, 패턴을 처음부터 다시 비교하지 않고 PI 테이블을 참조해 점프.
2. **탐색 과정**
    - 텍스트와 패턴을 비교하다가 불일치가 발생하면, PI 테이블을 참조하여 패턴을 몇 칸 옮길지 결정.

---

## 4. 부분 일치 테이블 (LPS 배열) 예시

패턴: `ABABCABAB`

| 인덱스 | 문자 | 접두사/접미사 중 최대 일치 길이 |
| --- | --- | --- |
| 0 | A | 0 |
| 1 | B | 0 |
| 2 | A | 1 |
| 3 | B | 2 |
| 4 | C | 0 |
| 5 | A | 1 |
| 6 | B | 2 |
| 7 | A | 3 |
| 8 | B | 4 |

따라서 LPS 배열 = `[0,0,1,2,0,1,2,3,4]`

---

## 5. 알고리즘 절차

1. **PI 테이블 생성**
    - 패턴의 접두사 = 접미사 길이를 계산.
2. **탐색**
    - 텍스트와 패턴을 한 글자씩 비교.
    - 불일치 발생 시, PI 테이블을 참조하여 패턴 인덱스를 이동.
    - 일치하면 다음 인덱스로 계속 진행.

---

## 6. 의사코드 (Pseudo Code)

```
function KMP(text, pattern):
    pi = computeLPS(pattern)
    i = 0  // text index
    j = 0  // pattern index

    while i < len(text):
        if text[i] == pattern[j]:
            i++, j++
            if j == len(pattern):
                print("Match found at", i - j)
                j = pi[j-1]
        else:
            if j != 0:
                j = pi[j-1]
            else:
                i++

```

---

## 7. 자바 코드 예시

```java
public class KMP {
    public static int[] computeLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        int len = 0; // 이전 접두사 길이
        int i = 1;

        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(len)) {
                lps[i++] = ++len;
            } else {
                if (len != 0) {
                    len = lps[len - 1];
                } else {
                    lps[i++] = 0;
                }
            }
        }
        return lps;
    }

    public static void search(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();
        int[] lps = computeLPS(pattern);

        int i = 0, j = 0;
        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++; j++;
                if (j == m) {
                    System.out.println("Match found at index " + (i - j));
                    j = lps[j - 1];
                }
            } else {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
    }

    public static void main(String[] args) {
        String text = "ABABDABACDABABCABAB";
        String pattern = "ABABCABAB";
        search(text, pattern);
    }
}

```