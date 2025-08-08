---
title: 2025-08-08
author: 조수아
date: 2025-08-08
category: TIL/조수아/2025/08
layout: post
---

# 오늘 배운 것

## 알고리즘

### 🔗 [D5_3421_수제버거장인](https://swexpertacademy.com/main/talk/solvingClub/problemView.do?solveclubId=AZgOVs9qcebHBISV&contestProbId=AWErcQmKy6kDFAXi&probBoxId=AZiG1qjarhvHBIT9+&type=PROBLEM&problemBoxTitle=250808&problemBoxCnt=++2+)
d5는 다르군요.. 바이너리로 조합의 종류를 나타내는걸 한번에 알아내지 못해서.. 결국 또 ai 힘을 빌렸습니당.. 비트마스킹 알고리즘은 항상 한번에 생각이 안드는 것 같아염.. 이문제는 다시푸는걸로..

## CSS

### 명시도 X-Y-Z
- 선택자를 이용해서 우선 순위를 정하기 위한 값
    - X : ID 선택자의 개수
    - Y : class 선택자, 속성 상태자, 가상 클래스 선택자의 개수
    - Z : 타입 선택자, 가상 요소 선택자의 개수

### 기본 선택자
- `* {}` : 전체 선택자
    - 명시도 0-0-0
    - HTML 문서 내 모든 요소를 선택하며, 주로 user agent의 css를 reset하는 용도로 사용
- `p {}` : 태그 선택자
    - 명시도 0-0-1
    - 지정된 태그명을 가진 요소를 선택
- `.header {}` : 클래스 선택자
    - 명시도 0-1-0
    - 특정 클래스 속성 값을 가진 요소를 선택. 하나의 태그에 여러 클래스 선언 가능
- `#logo {}` : ID 선택자
    - 명시도 1-0-0
    - 특정 id 속성 값을 가진 요소를 선택

### 복합 선택자
- `section > p {}` : 자식 선택자
    - 명시도 각 선택자의 합(예시 -> 0-0-2)
    - 부모 요소의 직계 자식 요소를 선택
- `section p {}` : 자손 선택자
    - 명시도 각 선택자의 합(예시 -> 0-0-2)
    - 부모 요소의 하위 모든 요소(자손)를 선택
- `header + p {}` : 인접 형제 선택자
    - 명시도 각 선택자의 합(예시 -> 0-0-2)
    - 지정된 요소의 바로 다음 형제 요소를 선택
- `header ~ p {}` : 일반 형제 선택자 선택자
    - 명시도 각 선택자의 합(예시 -> 0-0-2)
    - 지정된 요소의 다음에 오는 모든 형제 요소 선택

### 속성 선택자
- `[data-type] {}` : `[attr]` 선택자
    - 명시도 0-1-0
    - 특정 속성을 가진 요소를 선택
- `[type="text"] {}` : `[attr=val]` 선택자
    - 명시도 0-1-0
    - 특정 속성 값이 정확히 일치하는 요소를 선택
- `[lan|="ko"] {}` : `[attr|=]` 선택자
    - 명시도 0-1-0
    - 특정 속성 값이 정확히 일치하거나 해당 값 뒤에 하이픈(-)이 오는 경우
- `[href^="https"] {}` : `[attr^=val]` 선택자
    - 명시도 0-1-0
    - 특정 속성 값으로 시작하는 요소를 선택
- `[href$=".pdf"] {}` : `[attr$=val]` 선택자
    - 명시도 0-1-0
    - 특정 속상 값으로 끝나는 요소를 선택
- `[class~="button"] {}` : `[attr~=val]` 선택자
    - 명시도 0-1-0
    - 특정 속성 값을 단어로 포함하는 요소를 선택 (해당 속성 값 앞뒤에 바로 글자가 오면 안되고 공백으로 구분되어야함)
- `[class*="dark"] {}` : `[attr*=val]` 선택자
    - 명시도 0-1-0
    - 특정 속성 값을 포함하는 요소를 선택(이건 그냥 포함!)

### 가상 클래스 선택자
- 실제로 class 형태로 존재하지는 않지만 상황에 따라 적용되는 class를 지정하기 위한 것
- 클래스 이름 앞에 : 추가
- 개발자가 태그에 가상 클래스를 지정할 필요는 없음

- 구조적 가상 클래스
    - `li:first-child {}` : `E:first-child` 선택자
        - 명시도 0-1-0
        - E가 첫번째 자식으로 등장한 요소
    - `li:last-child {}` : `E:last-child` 선택자
        - 명시도 0-1-0
        - E가 마지막 자식으로 등장한 요소
    - `tr:nth-child(2n) {}` : `E:nth-child(n)` 선택자
        - 명시도 0-1-0
        - 앞에서 n 번째 자식 요소
        - 처음 요소는 1
        - n 은 0부터 시작하는 정수값이며 2n, 2n+1 등 연산식 사용 가능
        - 짝수 번째, 홀수 번째를 의미하는 evne, odd사용 가능
    - `tr:nth-last-child(even) {}` : `E:nth-last-child(n)` 선택자
        - 명시도 0-1-0
        - 뒤에서 n 번째 자식 요소

### box model
- 텍스트, 이미지 등의 모든 콘텐츠를 사각의 박스 형태로 관리하는 모델
- 구성 요소
    - content
    - padding
    - border
    - margin

### display 속성
- block 요소는 width와 height를 갖지만 inline 요소는 무시됨

### box-sizing
- width와 height를 측정할 때의 기준 설정
    - box-sizing : content-box;
        :  content만 해당 width, height
    - box-sizing : border-box;
        : content + padding + border 합해서 해당 width, height

### line-height
- 줄 간격을 지정하는 속성으로 블록 요소내 컨텐트의 세로 중앙 정렬에 자주 사용됨

# 내일 할 것
- 토스 코딩 테스트 치고 꼭 내용 기록하기
- css2, js1, js2 복습
- Real mySQL 챕터 4 읽고 기록하기
- 대규모 설계 시스템 책 읽고 기록

