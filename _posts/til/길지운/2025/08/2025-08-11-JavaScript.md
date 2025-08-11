---
title: 2025-08-11
author: 길지운
date: 2025-08-11
category: TIL/길지운/2025/08
layout: post
---

### 1일 1아티클
#### 요즘IT
###### 맥에서 쿠버네티스 클러스터 구성하기
**상황**
  - VM웨어 퓨전 : 사전 준비 및 사용에 불편 요소, 라이선스 문제, 회원 가입 필요
  - **버추얼박스(VirtualBox)** : arm64(맥) 지원 시작
    - 초기 arm 지원 버전은 버그 많았으나, 2024년 9월 **v7.1**부터 정상화
  
**설치 소프트웨어**
  - 버추얼박스(VirtualBox)
  - 베이그런트(Vagrant)
  - 타비(Tabby)
  
**추가 작업**
  - 헤드램프(Headlamp)
    - GUI 형태로 클러스터 상태 확인
  
### 오늘 배운 것  
1. JavaScript
  
    > **이벤트 객체**
    - 마우스 이벤트
      - ``` offsetX, offsetY ``` : 현재 요소 기준
      - ``` clientX, clientY ``` : 브라우저 기준 (스크롤바 무시)
      - ``` pageX, pageY ``` : 브라우저 기준 (스크롤바 포함)
      - ``` screenX, screenY ``` : 화면 모니터 기준
    - 키보드 이벤트
      - ``` key ``` : 사용자가 누른 키의 값 (Shift, a, ㅁ, etc.)
      - ``` code ``` : 실제 누른 키의 위치 (ShiftLeft, ShiftRight, etc.)
    - 전체 이벤트
      - ``` currentTarget ``` : 이벤트 리스너가 등록된 요소 (포괄적인 부분)
      - ``` target ``` : 이벤트가 실제로 발생한 요소 (구체적인 부분)
  
    > **이벤트가 겹칠 때**
    - capturing : 이벤트가 외부 요소부터 내부 요소로 파고 들어가며 발생
      - addEventListener 함수에서 useCapture 파라미터를 true로 지정해야 함
    - **bubbling** : 이벤트가 내부 요소부터 외부 요소로 거품처럼 올라오며 발생
      - default 형태
      - Java는 버블링만 가능
  
    > **이벤트 진행 제어 함수**
    - ``` event.stopPropagation() ``` : 이벤트 전파 막기
    - ``` event.preventDefault() ``` : 요소의 기본 동작 막기 (``` <a></a>, <form></form>, etc.```)
  
    > **invalid 이벤트**
    - 해당 이벤트에서 문제 없으면 submit
    - ``` <form> </form> ``` 에서 novalidate 제공
    - invalid 이벤트는 **bubbling 발생 X, capturing에서 처리**

    > **BOM (Browser Object Model)**
    - 브라우저 요소 객체화
  
    > **window**
    - 브라우저 객체의 최상위 객체
    - 모든 객체들은 window의 property들, 일반적으로 생략하여 사용  
      >  
      > 일반 함수
      - ``` open() ``` : 열기, opener 속성 사용 시 자식 창에서 부모 창 참조
      - ``` reference.close() ``` : 닫기
      - ``` alert(message) ``` : 경고창
      - ``` prompt(text, value) ``` : text 질의에 대한 응답 창, value는 기본값
      - ``` confirm(message) ``` : 확인/취소 창  
      >  
      > **주기 작업 및 예약 작업**
      - **``` setInterval(func, delay) ```** : 주기적인 delay 두고 func 실행
      - **``` clearInterval(intervalID) ```** : 반복 작업 중지
      - **``` setTimeout(func, [delay]) ```** : delay(기본 0) 이후 func 실행 (예약)
      - **``` cleartTimeout ```** : 예약 작업 중지
  
    > **screen**
    - ``` width ``` : 가로
    - ``` availWidth ``` : 작업표시줄 제외 가로
    - ``` height ``` : 세로
    - ``` availHeight ``` : 작업표시줄 제외 세로
    - ``` colorDepth ```
  
    > **location**
    - 사용자 브라우저 주소창의 url 정보 및 새로고침 기능 제공
    - ``` href ``` : 새로운 페이지로 이동 (일반적인 방식)
    - ``` replace() ``` : 새로운 페이지로 덮어쓰기 (이전 페이지로의 접근 금지, ex. 단계별 회원가입)

    > **history**
    - 사용자가 방문한 곳에 대한 목록 (뒤로 가기, 앞으로 가기)

    > **navigator**
    - 브라우저, 운영체제 정보 제공
    - geolocation api로 위치 정보 확인 제공
  
### 내일 할 일
1. 인프런 대규모 트래픽 강의 듣기

### 참고자료
- [새로 산 맥북으로 "더 쉽게" 쿠버네티스 클러스터 구성하기](https://yozm.wishket.com/magazine/detail/3284/)