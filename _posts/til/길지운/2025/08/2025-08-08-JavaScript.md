---
title: 2025-08-08
author: 길지운
date: 2025-08-08
category: TIL/길지운/2025/08
layout: post
---

### 1일 1아티클
#### 데보션
###### Cursor AI를 활용하여 Backend API 손쉽게 개발하기
**Cursor AI**
  - 개발자의 생산성을 높이는 AI 코드 작성 도구
  - Claude, GPT, Gemini 등 모델 기반
  
  **팁**
  - 명확한 지시어 사용
  - 작은 단위로 요청
  - 반복적인 피드백
  
> 핵심은 [8월 2일 TIL - 아티클](2025-08-02-아티클.html) 에서 나왔던 프롬프트/컨텍스트 엔지니어링  
> 프론트 쪽에서 이미 VS Code로 편하게 쓰고 있고, 백엔드 또한 VS Code로 환경 세팅이 쉽게 되는 걸로 알고 있었는데 기회가 된다면 기존 프로젝트의 레거시 코드의 클린코딩을 Cursor AI로 시도해 봐야겠다.
  
### 오늘 배운 것  
1. JavaScript
  
    > **Element 조회**
    - document를 통한 조회
    - ``` getElementById(id) ``` : 지정된 ID를 가진 **요소** 반환
    - ``` getElementsByClassName(class) ``` : 지정된 클래스 가진 요소들의 **유사배열** 반환
    - ``` getElementsByTagName(tag) ``` : 지정된 태그 가진 요소들의 **유사배열** 반환
    - ``` getElementsByName(name) ``` : 지정된 이름 가진 요소들의 **유사배열** 반환
    - ``` querySelector(selector) ``` : CSS 선택자와 일치하는 **첫 요소** 반환
    - ``` querySelectorAll(selector) ``` : CSS 선택자와 일치하는 요소들의 **배열** 반환
  
    > **Node 생성**
    - 문자열 활용
    - **innerHTML** : 요소 내부 콘텐츠를 가져오거나 설정(파싱)
    - **innerText** : 요소 내부 콘텐츠르 가져오거나 설정(파싱 X)
  
    > **Storage**
    - key, value (모두 문자열 형식, 묵시적 형변환) 형태로 관리
    - Windows 객체의 속성
      - localStorage : 브라우저 닫아도 유지
      - sessionStorage : 브라우저 닫으면 삭제
    - ``` setItem(key, value) / getItem(key) / removeItem(key) / clear() ```
      - ``` length / storage['key'] or storage.key ``` (python array와 비슷)
    - toString() 재정의되지 않은 일반적인 객체의 문자열 형식 → [object Object]
    - 직렬화 : ``` JSON.stringify() ```
    - 역직렬화 : ``` JSON.parse() ```
  
### 내일 할 일
1. 정처기 필기
2. 시간 될때 깃허브 블로그 사용자수 집계 구현
  
### 참고자료
- [Cursor AI를 활용하여 Backend API 손쉽게 개발하기](https://devocean.sk.com/blog/techBoardDetail.do?ID=167662&boardType=techBlog&searchData=&searchDataMain=DEV_FRW&page=&subIndex=&searchText=&techType=&searchDataSub=&comment=&p=BLOG)
- [8월 2일 TIL - 아티클](2025-08-02-아티클.html)