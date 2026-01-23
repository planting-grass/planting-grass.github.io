---
title: 2026-01-23
author: 김소희
date: 2026-01-23
category: TIL/김소희/2026/01
layout: post
---

## React Hooks 복습 - 오답 정리

복습 퀴즈에서 틀린 문제들을 정리한다.

---

### useRef: DOM 접근이 필요하면 useRef

input에 포커스를 주려면 **DOM 요소에 직접 접근**해야 한다.
화면에 표시하는 게 아니라 DOM 메서드를 호출하는 것이므로 `useState`가 아닌 `useRef`를 사용한다.

```jsx
const inputRef = useRef(null);

// 버튼 클릭 시
inputRef.current.focus();

return <input ref={inputRef} />;
```

**판단 기준**

- 화면에 보여야 함 → `useState`
- DOM 직접 건드림 → `useRef`

---

### useRef 사용 3단계

```jsx
// 1단계: 생성
const inputRef = useRef(null);

// 2단계: JSX에 연결
<input ref={inputRef} />;

// 3단계: .current로 접근
inputRef.current.focus();
```

생성 → 연결 → `.current`

---

### CCTV 비유

| Hook     | 비유           | 의미                                        |
| -------- | -------------- | ------------------------------------------- |
| useState | CCTV 달린 방   | 값 변경 시 React가 감지하고 리렌더링        |
| useRef   | CCTV 없는 창고 | 값 변경해도 아무 일 없음, 나중에 꺼내 쓸 뿐 |

---

### dispatch = action 전달 함수

`dispatch`는 action을 reducer에게 전달하는 역할을 한다.

```jsx
dispatch({ type: "INCREMENT" });
//         ↓
//  action = { type: "INCREMENT" }
//         ↓
//  Reducer가 받아서 상태 업데이트
```

카운터에서 "주문입니다!" 하고 전달하는 것과 같다.

---

### 요약

- DOM 접근 = useRef
- useRef 3단계: 생성 → 연결 → .current
- useState는 감시됨, useRef는 감시 안 됨
- dispatch는 action을 reducer에 전달하는 함수
