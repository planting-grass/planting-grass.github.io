---
title: 2025-08-04
author: 조수아
date: 2025-08-04
category: TIL/조수아/2025/08
layout: post
---

# 오늘 배운 것
## 알고리즘 문제
### 🔗 [BJ_20055_컨테이어벨트위의로봇](https://www.acmicpc.net/problem/20055)

처음에는 배열 자체를 돌릴려는 어리석은 생각을 했지만 인덱스를 돌려서 충분히 할 수 있었다..! 로직 상 내리는 위치 부터 먼저 체킹하고 벨트를 돌린 후에 다시 내리는 위치를 체킹하면 됐는데 이번에도 너무 섣불리 구현에 돌입만 한 것이 아닌지 싶다. 삼성 기출 문제라고 해서 풀었는데, 조금 구현이나 시뮬레이션 적이 부분이 많다는 것을 느껴서 무조건 알고리즘으로 풀 수 있을거야 라고 겁먹기보다는 구현을 어떻게 할지 고민해보는 것도 중요한 것 같다.

## 🔗 [D3_9299_한빈이와SpotMart](https://swexpertacademy.com/main/talk/solvingClub/problemView.do?solveclubId=AZgOVs9qcebHBISV&contestProbId=AW8Wj7cqbY0DFAXN&probBoxId=AZhyrab6StbHBIT9+&type=PROBLEM&problemBoxTitle=250804&problemBoxCnt=++2+)

투포인터를 활용하여 문제를 풀었고 특별히 어렵지 않았다. while 문으로 left가 right를 초과하기 전까지 검사하면서 가장 최댓값을 출력하도록 구현했다.

## 🔗 [D4_1233_사칙연산유효성검사](https://swexpertacademy.com/main/talk/solvingClub/problemView.do?solveclubId=AZgOVs9qcebHBISV&contestProbId=AV141176AIwCFAYD&probBoxId=AZhyrab6StbHBIT9+&type=PROBLEM&problemBoxTitle=250804&problemBoxCnt=++2+)

단순 조건으로 풀었다. 노드의 값이 연산자면 자식 노드가 2개 존재해야하고 노드의 값이 숫자면 자식 노드가 없는 리프노드 인지 체크하고 한번이라도 틀린 경우가 생기면 flag를 설정했다.

## HTML

### HTML
- 인터프리터 언어로 웹브라우저에 의해 해석됨
    - DOM 트리를 구성하고 화면에 표시

### Tag
- HTML 문서를 구성하는 요소
    - 여는 태그에서 닫는 태그까지의 모든 것은 element라고 함
    - content가 없는 태그는 축약형 사용 : `<br/>`, `<hr/>`
    - 일반적으로 `<html>`이 최상의 루트 태그
    - 태그들은 중첩되며 사용가능하나 꼬이면 안된다
    - html 태그는 대소문자를 가리지 않으나 관례사 소문자를 사용한다

### block 요소 VS inline 요소
- block
    - 태그를 사용하면 자동적으로 줄바꿈이 되는 요소
    - 주로 문서의 구조나 레이아웃을 정의하는데 사용
    - 종류
        - `address`
        - `blockquote`
        - `hr`
        - `center`
        - `dir`
        - `div`
        - `dl`
        - `fieldset`
        - `form`
        - `h1` ~ `h6`
        - `br`
        - `isindex
        - `menu`
        - `noframes`
        - `ol`
        - `p`
        - `pre`
        - `table`
        - `ul`
- inline
    - 태그를 사용하면 한 줄 안에서 다른 인라인 요소와 함게 표시됨
    - 주로 텍스트와 관련된 요소에 사용됨
    - 종류
        - `a`
        - `b`
        - `big`
        - `button`
        - `cite`
        - `code`
        - `em`
        - `i`
        - `iframe`
        - `img`
        - `input`
        - `label`
        - `q`
        - `select`
        - `small`
        - `span`
        - `strong`
        - `sub`
        - `sup`
        - `textarea`
        - `u`

### Attribute
- 태그에 대한 추가적인 정보 제공
    - 여는 태그에 기록
    - name = "value" 형태
        - 일부 속성은 값 없이 선언만으로도 사용 가능(readonly, selected)
    - "", '' 둘다 사용가능하나 기본적으로 "" 사용
    - 필요한 경우 여러 개의 attribute 사용 가능

### global attribute
- 모든 태그에서 사용할 수 있는 속성
- 종류
    - `id` : 요소의 고유 식별자 지정
    - `class` : 여러 요소에 공통적으로 적용할 클래스를 지정
    - `style` : 인라인 스타일을 정의함
    - `data-*` : 사용자 정의 데이터 속성을 추가
    - `title` : 요소에 대한 추가 정보를 제공함
    - `lang` : 요소의 언어를 정의함
    - `tabindex` : 요소의 포커스 순서를 지정함
    - `hidden` : 요소를 숨김

### 문서의 구조
```
<!--- 주석문 : 주석은 사용자에게 그대로 전달되기 때문에 민감한 내용이 들어가지 않도록 주의 --->
<!DOCTYPE html>
<!--- html 문서의 시작을 여는 태그, lang 속성으로 문서의 언어 지정(한국어에서 검색 기능 등에 활용) --->
<html lang="ko>
    <!--- 화면에는 보이지 않으나 브라우저가 알아야 할 정보 표시. css나 js같은 외부 파일에 대한 링크 등 --->
    <head>
        <!--- 문자 인코딩 및 문서 키워드, 요약 정보 표시 --->
        <meta charset="UTF-8">
        <!--- 제목 표시줄에 표시되는 내용으로 방문자나 검색 엔진에 정보 제공 --->
        <title>first html</title>
    </head>
    <body>
    <!--- 실제 내용이 들어가는 태그 --->
    </body>
```

### 특수문자와 공백
- 태그를 표현하는 문자(<, >) 등 일부 문자는 예약어로 지정되어 있어 사용 불가
- 종류
    - `&nbsp;` : non-breaking space(공백)
        - 공백은 개수와 상관 없이 공백 하나로 인정 -> 여러개 붙여도 한개만 출력
    - `&lt;` : less than(<)
    - `&gt;` : greater than(>)
    - `&amp;` : ampersnad(&)
    - `&dollar;` : ($)
    - `&won;` : (₩)
    - `&copy;` : copyright(©️)
    - `&reg` : registered trademark(®️)

### `<h1>` ~ `<h6>`
- 제목을 표시하는 태그로 block요소
- 검색 엔진은 heading을 이용해서 웹페이지의 구조와 콘텐츠를 색인화하므로 중요
- 중요도에 따라 1 ~ 6 사용

### list
- ol : ordered list
- ul : unordered list
- li : list item

### table
- 종류
    - `<table>`
    - `<tr>`
    - `<td>`
    - `<th>`
    - `<caption>`
    - `<thead>`
    - `<tbody>`
    - `<tfoot>`
    - `<col>`
    - `<colgroup>`
- tbody, thead, tfoot을 굳이 안붙여도 돔 기반으로 작동하기 때문에 자동으로 tbody로 들어감
    - 즉, css도 바로 적용이 안될 수 있음
- 셀병합
    - colspan = "2" : 가로로 2개의 셀 병합
    - rowspan = "2" : 세로로 2개의 셀 병합

### img
- `<img src = "이미지 파일경로" alt = "이미지를 설명하는 텍스트로 이미지가 로드 되지 않을 때 대체 텍스르톨 사용>"`

### link
- `<a>`
    - href : 링크의 목적지 URL
        - "#id" 형태로 동일한 페이지내 id 속성으로 대상 생성 가능 -> 목차
    - target = "_self" : 현재 화면에서 열림(기본값)
    - target = "_blank" : 새창이나 새탭으로 열림
        - 새창 또는 탭은 브라우저 정책으로 인해 결정할 수 없음
    - title : 링크에 대한 추가 정보
    - download : 링크를 클릭할 때 파일 다운로드 시작

### form
- 사용자로부터 데이터를 입력받아 서버로 전송하기 위해서 사용되는 요소
- 속성
    - action : 요청을 처리할 서버의 URL
    - method : 폼 데이터 전송 방법(get, post, dialog(대화형 다이얼로그 창 열 때 사용, 일반적인 form 전송 X))
    - enctype : 폼 데이터 인코딩 방식
        - application/x-www-form-urlencoded : url 쿼리 문자열 형식으로 인코딩
        - multipart/form-data : 파일 업로드나 자바스크립트의 폼 데이타 객체를 이용한 전송
        - text/plain : 인코딩 없이 전달
    - target : 폼 제출 후 결과를 열 위치 지정(_blank, _self)
    - name : 폼 이름
    - autocomplete : 자동완성 여부(on, off)
    - novalidate : 폼 유효성 검사를 비활성화 설정(값없음)
- 하위 태그
    - fieldset : 관련된 입력 요소를 그룹화하여 시각적으로 구분함
    - legend : fieldset의 제목을 정의함
    - label : 입력 필드에 대한 설명이나 이름을 제공
    - input : 사용자로부터 데이터를 입력받기 위한 필드
    - textarea : 여러 줄의 텍스트 입력을 위한 필드
    - select : 선택할 수 있는 옵션 목록을 제공하는 드롭다운 메뉴
    - option : select 태그 안에서 선택할 수 있는 갭려 옵션
    - button : 폼 제출을 위한 버튼을 생성

### input
- 공통 속성
    - type : gui 컴포넌트의 타입-> 기본값은 text
        - hidden : 사용자에 보이지 않지만 서버로 넘겨지는 값을 갖는 필드
        - text : 한줄짜리 텍스트를 입력할 수 있는 텍스트 상자
        - password : 비밀번호 입력
        - search : 검색 상자(검색 초기화 기능 -> x버튼 제공)
        - tel : 전화번호 입력 필드 -> 모바일 기기 가상 키보드 변화
        - url : url주소를 입력할 수 있는 필드 -> url 형식 체크(공백 x, 영문, 마침표, /로 구성)
        - email : 이메일 주소 입력할 수 있는 필드 -> 이메일 형식 체크(@ 필요)
        - number : 숫자를 조절할 수 있는 화살표 표시(min, max, value 속성)
        - range : 숫자를 조절할 수 있는 슬라이드 막대 표시
        - color : 색상표 삽입
        - checkbox : 2개 이상 선택 가능 항목
        - radio : 1가지만 선택 가능
        - date : 사용자 지역을 기준으로 날짜 입력 -> validation 체크
        - month : 사용자 지역을 기준으로 월 입력 -> validation 체크
        - week : 사용자 지역을 기준으로 주 입력 -> validation 체크
        - time : 사용자 지역을 기준으로 시간 입력 -> validation 체크
        - button : 동작할 수 있는 버튼 삽입
        - file : 파일을 첨부할 수 있는 버튼
        - submit : 폼의 내용을 서버로 전송
        - reset : 입력 필드들에 대한 초기화
    - name : input 요소의 이름으로 서버에 전달되는 parameter 이름
    - value : input 요소의 기본 값으로 parameter의 value로 서버로 전달
    - id : css, javascript에서 요소를 구별하기 위한 값으로 한 화면에서 유일한 값
- 기타 속성
    - name : 입력 필드의 이름 지정
    - value : 입력 필드의 기본 값 지정
    - placeholder : 입력 필드에 안내 텍스트 표시
    - required : 입력 필드의 필수 여부 지정
    - readonly : 입력 필드를 읽기 전용으로 설정
    - disabled : 입력 필드를 비활성화, 서버 전송 없음
    - minlength : 최수 문자수
    - maxlength : 최대 문자수
    - min : 최소값
    - max : 최댓값
    - step : 수자 입력 필드의 증가 단위
    - pattern : 정규 표현식 패턴 지정

### label
- input 등 요소에 설명을 추가하기 위해 사용
    - label 내부에 input 작성
        `<label>user:<input type="text" name="username value="cho" /></label>`
    - input 요소에 id를 지정하고 이를 통해 label과 연결
        ```
        <label for="user">user</label>
        <input type="text" name="username value="cho" />
        ```
        - css를 통해 간격이 동일한 라벨을 생성하기 위해서는 두번재 방법이 좋음

### Form 전송방식
- Get
    - 명시적으로 post라고 하지 않은 모든 경우 default
        - 주소창에 직접 경로명 입력 후 요청
        - a 태그를 통한 경우
        - form 에서 명시적으로 get 방식 전달
    - 주소창을 통해 query string으로 파라미터 전달
    - 브라우저별로 길이 제한 -> 상대적으로 적은 데이터만 전송 가능
    - 데이터가 노출될 수 있어 보안에 취약
    - 요청의 처리가 데이터의 영향을 주지 않을 경우 -> 단순 조회
    - 파라미터가 포함된 요청 자체를 북마크로 사용하고 싶을때
    - 검색, 글보기
- Post
    - 명시적으로 post라고 선언
    - 요청의 바디에 포함되어 전달
    - 용량 제한 없음
    - 데이터가 노출되지 않고 상대적으로 보안에 강함
    - 요청 처리가 서버의 데이터에 영향을 줄 경우
    - 자료 수정, 삭제, 추가
    - 보안이 필요한 경우, 파일 업로드 등
    - 회원가입, 로그인

### button
- input의 버튼과 그냥 버튼 태그 차이점
    - 버튼은 content 포함이 가능하기 때문에 이미지 등의 다양한 내용이 포함가능
    - input은 단순 문자열 형태의 값만 가질 수 있음
    - 버튼은 스크린 리더를 통한 접근성 향상이 가능함

### select
- 드롭다운 목록에서 값을 선택 시 사용
- 주요 속성
    - size : 기본 표시할 옵션의 개수
- 하위 태그
    - optgroup : 옵션 그룹을 묶어 표시하며 선택되는 항목이 아니며 label 속성으로 그룹의 제목 표현
    - option : 선택 가능한 항목 정의, 서버로 전송되는 값을 나타내는 value와 화면에 표시되는 laebl 속성 포함

### 공간 분할 태그
- div
    - block 요소
    - 페이지의 전체 틀 만들 때 사용
    - width, heigth를 이용한 크기 소유 가능
    - margin : 4방향 지정
- span
    - inline dyth
    - 특정 부분에 스타일 적용 목적으로 사용
    - 크기 지정 불가
    - margin : 좌우 지정

### semantic 태그
- div나 span으로는 어떤 용도로 사용되었는지 파악하기가 어렵고 개발자마다 사용하는 id가 다름
    -> 즉, 강제적인 태그 이름에는 의미가 없고 의미를 갖는 속성인 id는 강제성이 없어서 문제가 발생
- 이것을 해결하기 위해 태그 자체에 의미를 부여하는 semantic 태그 등장
- 컨텐츠의 의미를 나타내는 것이지 위치를 특정하거나 모양이 달라지는 것은 아님
- 또한 SEO, Screen Reader에서의 활용 할 수 있는 장점이 있음
- 종류
    - header : 무엇인가의 제목
    - nav : 문서를 연결하는 네비게이션 링크 표시
    - section : 주제별 컨텐츠 영역을 표시, 일반적으로 내부에 여러개 article 포함
    - article : 실제 컨텐츠의 내용 등록
    - aside : 본문 이외의 내용(광고, 링크 모음)
    - footer : 문서 말미에 제작 정보, 저작권 등의 정보 표시

# 내일 할 것
- 이력서, 포트폴리오 업데이트
- ACT 보기
- 오픈 알록 회의
- 트리 복습
- 화요일 공부 내용 복습
- (가능하면..?) 코드 배틀 1번 문제 풀기..