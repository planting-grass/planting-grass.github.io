---
title: Kakao Maps Script Loading Strategy
author: 유주성
date: 2026-03-17
category: TIL/유주성/2026/03
layout: post
---
## Kakao Maps Script Loading Strategy

### 1. 문제 상황

Kakao Maps를 Next.js에서 사용할 때 가장 먼저 부딪히는 문제는

**스크립트 로딩 타이밍과 지도 초기화 타이밍이 맞지 않는 것**이다.

대표적으로 다음과 같은 문제가 발생한다.

- `window.kakao is undefined`
- 지도 초기화 안됨
- 마커가 안 찍힘
- 간헐적으로 동작 (가장 위험)

이 문제는 대부분 **Script 전략 설정과 autoload 옵션을 제대로 이해하지 못해서 발생**한다.

---

### 2. Next.js Script 전략 개념

Next.js의 `<Script>` 컴포넌트는 외부 스크립트를 언제 로드할지 결정하는 옵션을 제공한다.

### 주요 전략 3가지

- beforeInteractive
- afterInteractive
- lazyOnload

---

### 3. beforeInteractive의 의미

```
<Script
strategy="beforeInteractive"
/>
```

### 동작 방식

- React hydration 이전에 실행
- 페이지가 인터랙션 되기 전에 로드 완료
- 전역 객체 (`window.kakao`)를 초기 렌더 시점에 보장

### 핵심 포인트

React보다 먼저 실행된다

---

### 4. Kakao Maps에서 beforeInteractive를 사용하는 이유

Kakao Maps는 다음과 같은 구조를 가진다.

```
if (!window.kakao)return;
```

즉,

`window.kakao`가 존재해야 지도 생성 가능

---

### 문제 케이스 (afterInteractive 사용 시)

1. React 먼저 렌더링됨
2. Script 나중에 로드됨
3. `window.kakao` 없음
4. 지도 초기화 실패

---

### 해결 방식

```
<Script
src="https://dapi.kakao.com/v2/maps/sdk.js?...&autoload=false"
strategy="beforeInteractive"
/>
```

Script를 먼저 로드해서 kakao 객체를 확보

---

### 5. autoload=false의 역할

```
autoload=false
```

### 의미

- SDK가 자동으로 실행되지 않음
- 개발자가 직접 초기화 타이밍을 제어

---

### 반드시 같이 사용해야 하는 코드

```
window.kakao.maps.load(() => {
// 지도 생성
});
```

### 이유

- Script는 로드되었지만
- 내부 maps 모듈은 아직 준비되지 않았을 수 있음

load 콜백에서 안전하게 실행

---

### 6. beforeInteractive + autoload=false 조합

이 조합은 다음과 같은 흐름을 만든다.

1. Script는 가장 먼저 로드됨
2. kakao 객체는 확보됨
3. maps는 자동 실행 안됨
4. 개발자가 원하는 시점에 load 실행

---

### 장점

- 지도 초기화 안정성 확보
- race condition 방지
- 예측 가능한 실행 흐름

---

### 7. 다른 전략과 비교

### afterInteractive

```
strategy="afterInteractive"
```

- React 이후 실행
- 초기 렌더는 빠름
- 하지만 kakao 객체 타이밍 불안정

지도 서비스에서는 비추천

---

### lazyOnload

```
strategy="lazyOnload"
```

- 페이지 로딩 완료 후 실행
- 가장 늦게 로드됨

지도 로딩이 늦어 UX 저하

---

### 8. 지도 서비스 기준 전략 선택

지도 서비스에서는 다음 기준이 중요하다.

- 지도 = 핵심 UI
- 첫 진입 시 바로 보여야 함
- 사용자 인터랙션과 직접 연결됨

---

### 결론

```
<Script
strategy="beforeInteractive"
src="...&autoload=false"
/>
```

가장 적합한 선택

---

### 9. 실무 트러블슈팅 포인트

### 1. kakao undefined

- Script 전략 문제
- beforeInteractive 확인 필요

### 2. 지도 안 뜸

- `kakao.maps.load` 호출 여부 확인

### 3. 마커 안 찍힘

- map 생성 이전 실행 여부 확인

### 4. 간헐적 오류

- autoload=true 사용했을 가능성 높음

---

### 10. 정리

Kakao Maps를 Next.js에서 사용할 때 핵심은

**스크립트 로딩 타이밍과 지도 초기화 타이밍을 분리하는 것**이다.

이를 위해

- Script는 beforeInteractive로 빠르게 로드하고
- autoload=false로 자동 실행을 막고
- kakao.maps.load로 초기화를 제어한다

이 구조를 사용하면 지도 관련 대부분의 비동기 문제를 해결할 수 있다.

---

### 한 줄 정리

Kakao Maps는

"언제 로드되느냐"보다

"언제 초기화하느냐"가 더 중요하다.