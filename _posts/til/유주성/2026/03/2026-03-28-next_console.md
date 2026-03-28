---
title: Next.js console
author: 유주성
date: 2026-03-28
category: TIL/유주성/2026/03
layout: post
---

프로젝트 개발을 하며, API 연동을 하다가 콘솔을 찍었는데 어떤 것은 vs 코드에 로그가 찍히고, 어떤 것은 브라우저에 찍힌다. 차이점이 궁금했다. 

# Next.js에서 console.log가 VSCode와 브라우저에 다르게 찍히는 이유

---

## 1. 문제 상황

Next.js에서 `console.log`를 찍었을 때

어떤 로그는 VSCode 터미널에 찍히고, 어떤 로그는 브라우저 개발자 도구에 찍힌다.

동일한 코드처럼 보이지만 출력 위치가 달라 혼란이 발생했다.

---

## 2. 원인

Next.js는 하나의 프로젝트 안에서

서버 코드와 클라이언트 코드가 동시에 실행되는 구조이기 때문이다.

즉, `console.log`는 실행되는 위치에 따라 출력되는 곳이 달라진다.

---

## 3. 실행 위치에 따른 로그 출력

### 3.1 서버에서 실행되는 경우

- Node.js 환경에서 실행됨
- VSCode 터미널에 출력됨

```jsx
export async function getServerSideProps() {
  console.log("서버 로그");
  return { props: {} };
}
```

특징

- SSR 시 실행됨
- API Route, 서버 컴포넌트 포함

---

### 3.2 클라이언트에서 실행되는 경우

- 브라우저 환경에서 실행됨
- 개발자 도구 콘솔에 출력됨

```jsx
"use client";

useEffect(() => {
  console.log("클라이언트 로그");
}, []);
```

특징

- 사용자 PC에서 실행됨
- 이벤트, useEffect 등

---

## 4. 헷갈리는 케이스

### 4.1 같은 코드처럼 보이지만 다른 경우

```jsx
export default function Page() {
  console.log("로그 위치?");
  return <div>Hello</div>;
}
```

- 서버 컴포넌트 → VSCode 출력
- "use client" 추가 → 브라우저 출력

---

### 4.2 로그가 두 번 찍히는 경우

```jsx
"use client";

export default function Page() {
  console.log("렌더링");
  return <div>Hello</div>;
}
```

결과

- 서버에서 1번 (SSR)
- 브라우저에서 1번 (Hydration)

원인

Next.js는 초기 HTML을 서버에서 생성한 뒤,

브라우저에서 다시 React를 실행(hydration)하기 때문이다.

---

## 5. 핵심 정리

- VSCode에 찍힘 → 서버 실행
- 브라우저에 찍힘 → 클라이언트 실행
- 둘 다 찍힘 → SSR + Hydration 구조

---

## 6. 실무에서의 활용

- API 응답 확인 → 서버 로그 확인
- UI 동작 확인 → 브라우저 로그 확인
- window, document 관련 에러 → 클라이언트 코드인지 확인
- 로그가 2번 찍히면 → SSR 여부 체크

---

## 7. 느낀 점

결국 vs 코드에 찍히는 것은 나의 경우 백엔드에 요청을 보내거나 응답을 받을 때, 중간 서버를 두어서 걔를 가지고 백엔드랑 통신을 하는데, 거기다 콘솔을 찍으면 vs 코드에 찍히는 것 같다.