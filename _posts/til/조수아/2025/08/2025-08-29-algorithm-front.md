---
title: 2025-08-29
author: 조수아
date: 2025-08-29
category: TIL/조수아/2025/08
layout: post
---

# 오늘 배운 것

## 순열, 조합, 부분집합

### 순열

- 서로 다른 것들 중 몇 개를 뽑아서 한 줄로 나열하는 것으로 N개의 요소들에대해서 n!개의 순열들이 존재한다.
- 예시로는 반에서 반장, 부반장, 총무를 순서대로 뽑는 상황을 예시로 들 수 있다.

### 조합

- 서로다른 N개의 원소 중  R개를 순서 없이 골라낸 것
- n! / ((n-r)! * r!)개의 조합들지 존재한다.
- 예시로는 반에서 대의원회 3명을 뽑는 상황을 예시로 들 수 있다.

### 부분집합

- 어떤 집합의 원 소 중 일부(또는 전부, 혹은 아무것도)로 이루어진 집합을 의미한다.
- 집합의 원소가 n개일 때 부분집합은 2^n개 존재한다
- 반에서 응원단을 뽑는 경우의 수로 아무도 없을 수도 있고 전부 응원단에 참여할 수 도 있는 예시를 들 수 있다.

## 분할 정복

### **BJ 2630 실버 2 색종이 만들기**

```java
package baekjoon.silver2;

import java.io.*;
import java.util.*;

public class Main_bj_2630_색종이만들기 {
    static int N;
    static int[][] g;
    static int white, blue;

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        N = Integer.parseInt(br.readLine());
        g = new int[N][N];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            for (int j = 0; j < N; j++) g[i][j] = Integer.parseInt(st.nextToken());
        }

        divide(0, 0, N);

        StringBuilder sb = new StringBuilder();
        sb.append(white).append('\n').append(blue);
        System.out.println(sb);
    }

    static void divide(int x, int y, int n) {
        if (isUniform(x, y, n)) {
            if (g[x][y] == 0) white++;
            else blue++;
            return;
        }
        int m = n / 2;
        divide(x, y, m);
        divide(x, y + m, m);
        divide(x + m, y, m);
        divide(x + m, y + m, m);
    }

    static boolean isUniform(int x, int y, int n) {
        int color = g[x][y];
        for (int i = x; i < x + n; i++) {
            for (int j = y; j < y + n; j++) {
                if (g[i][j] != color) return false;
            }
        }
        return true;
    }
}

```

### **BJ 1992 실버1 쿼드트리**

```jsx
package baekjoon.silver1;

import java.io.*;
import java.util.*;

public class Main_bj_1992_쿼드트리 {
	static int N;
	static int[][] g;
	static String answer;
	
	static void divide(int x, int y, int n) {
        if (isUniform(x, y, n)) {
            if (g[x][y] == 0) answer+="0";
            else answer+="1";
            return;
        }
        int m = n / 2;
        answer += "(";
        divide(x, y, m);
        divide(x, y + m, m);
        divide(x + m, y, m);
        divide(x + m, y + m, m);
        answer += ")";
    }

    static boolean isUniform(int x, int y, int n) {
        int color = g[x][y];
        for (int i = x; i < x + n; i++) {
            for (int j = y; j < y + n; j++) {
                if (g[i][j] != color) return false;
            }
        }
        return true;
    }
	
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		N = Integer.parseInt(br.readLine());
		g = new int[N][N];
		for (int i = 0; i < N; i++) {
			String[] line = br.readLine().split("");
			for (int j = 0; j < N; j++) {
				g[i][j] = Integer.parseInt(line[j]);
			}
		}
		answer = "";
		divide(0, 0, N);
		System.out.println(answer);
	}

}

```

### **BJ 1074 골드 5 z**

```jsx
package baekjoon.gold5;

import java.io.*;
import java.util.*;

public class Main_bj_1074_z {
    static int N, R, C;

    static int solve(int n, int r, int c) {
        if (n == 1) return 0;
        int half = n / 2;
        if (r < half && c < half) {
            return solve(half, r, c);
        } else if (r < half && c >= half) {
            return half * half + solve(half, r, c - half);
        } else if (r >= half && c < half) {
            return 2 * half * half + solve(half, r - half, c);
        } else {
            return 3 * half * half + solve(half, r - half, c - half);
        }
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        N = Integer.parseInt(st.nextToken());
        R = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());

        System.out.println(solve((int)Math.pow(2, N), R, C));
    }
}

```

## 유니온 파인드

### D4 7465 창용 마을 무리의 개수

```jsx
import java.io.*;
import java.util.*;

public class Solution {
	static int T, N, M;
	static int[] parent;
	
	static int find(int x) {
		if (parent[x] == x) return x;
		return parent[x] = find(parent[x]);
	}
	static void union(int x, int y) {
		x = find(x);
		y = find(y);
		if (x == y) return;
		if (x > y) {int t = x; x = y; y = t;}
		parent[y] = x;
	}
	
	public static void main(String[] args) throws Exception {
		//System.setIn(new FileInputStream("res/s_input.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st;
		T = Integer.parseInt(br.readLine());
		for (int t = 1; t <= T; t++) {
			st = new StringTokenizer(br.readLine(), " ");
			N = Integer.parseInt(st.nextToken());
			M = Integer.parseInt(st.nextToken());
			parent = new int[N+1]; for(int i = 0; i < N+1; i++) parent[i] = i;
			for (int i = 0; i < M; i++) {
				st = new StringTokenizer(br.readLine(), " ");
				int a = Integer.parseInt(st.nextToken());
				int b = Integer.parseInt(st.nextToken());
				union(a,b);
			}
			HashSet<Integer> set = new HashSet<>();
			for (int i = 1; i < N+1; i++) {
				find(i);
				set.add(parent[i]);
			}
			System.out.printf("#%d %d\n", t, set.size());
		}

	}

}
```

- 프림 PQ, 크루스칼 코드 공부

## HTML

## form 태그 전송방식

> form 태그 전송 방식은 **GET**과 **POST** 두 가지가 있으며,
GET은 주소창에 쿼리 스트링을 붙여 데이터를 전달하는 방식으로 브라우저에서 쉽게 확인 가능하고 요청 길이에 제한이 있습니다.
반면, POST는 데이터를 요청 본문에 담아 전달하는 방식으로 주소창에 노출되지 않고 용량 제한이 없다는 특징이 있습니다. 
GET은 주로 단순 조회나 검색 시 사용되고,
POST는 회원가입·로그인·글쓰기처럼 서버 데이터에 변화를 주는 요청에 사용됩니다.
> 

## CSS

## JS

## AJAX

## async await VS promise

> 둘 다 비동기 처리를 위한 방식이지만, 
**`Promise.then`**은 체이닝으로 순서를 보장하며 각 단계의 결과를 다음 단계로 넘기기 쉬운 장점을 가졌지만, 체인이 길어질수록 가독성이 떨어지고 에러 처리가 여러 구간에 흩어지기 쉬운 단점을 가졌습니다. 
반면 **`async/await`**은 비동기 코드를 동기 코드처럼 직관적으로 작성할 수 있어 가독성과 유지보수성이 높고 `try/catch`로 한 곳에서 에러를 다루기 쉬운 장점을 가졌지만, `await`를 남용하면 불필요하게 직렬 실행이 되어 느려질 수 있는 단점을 가졌습니다.
>