---
title: 2025-08-07
author: 조수아
date: 2025-08-07
category: TIL/조수아/2025/08
layout: post
---

# 오늘 배운 것

## 알고리즘

### 🔗 [D2*2001*파리퇴치](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV5PzOCKAigDFAUq)

누적합을 이용해서 해결했다. 일차원 배열 누적합은 금방금방 잘하는데 이상하게 dp도 그렇고 2차원부터는 한번에 떠오르지 않아서 시간이 오래걸렸다. N+1 크기의 prefix배열을 만들어서 이차원배열의 누적합을 계산하였다 그리고 해당 값을 탐색해 가면서 가장 최대로 파리 잡는 경우를 출력해줬다

### 🔗 [D3*5215*햄버거다이어트](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AWgv9va6HnkDFAW0)

부분집합으로 문제를 풀어서 탈출 조건은 인덱스가 끝에 도달했을 때로 하고 해당 음식을 포함할지 말지 여부는 합해도 최대 칼로리 이하면 포함하고 그리고 안포함하는 경우는 항상 적용되도록 풀었다. 이전에도 풀었던 문제라 특별하게 어렵지는 않았다.

## CSS

### 기본 문법

- 선택자와 선언부로 구성
  - 선택자 : 정의한 스타일을 적용할 대상
  - 선언부 : 선택자에 적용할 스타일로 `{}` 안에 작성하며 여러 개 일 경우 `;`으로 구분
    > h1 {color : blue; font-size : 12px;}
- 주석 : multiline 형태
  > /_ 주석 내용을 이렇게 적으면 됌~_/

### CSS 스타일 적용법

- 인라인 스타일
  - 개별 태그들에 `style= "property:value"` 형태로 지정
  - 별도로 선택자를 사용하지 않고 재사용이 불가 하다
  - 일반적으로 다른 스타일에 비해 강력한 우선순위를 가짐
- 내부 스타일
  - `<head>`에 `<style>` 태그를 추가하여 해당 태그 안에 작성
  - 해당 html 페이지 내에서는 선택자 기준으로 재사용 가능
  - 페이지에 특화된 내용 작성 시 사용한다
- 외부 스타일 시트 활용
  - 외부에 별도의 스타일 시트 파일(.css)을 만들고 `<link>` 태그를 이용해서 연결
  - 모든 페이지에서 재사용 가능
  - `@import`
    - link와 달리 style 태그 안에 설정하며 다른 CSS 파일 내부에서도 사용 가능
    - style 태그의 맨 상단에 위치해야 함
    - `@import url("file path");` 또는 `@import "file path";` 형태로 사용
