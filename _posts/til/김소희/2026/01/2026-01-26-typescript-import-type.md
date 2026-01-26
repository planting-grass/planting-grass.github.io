---
title: 2026-01-26
author: 김소희
date: 2026-01-26
category: TIL/김소희/2026/01
layout: post
---

# 📝 TypeScript import type 학습 회고

> **Date**: 2026-01-26
> **Tags**: #TypeScript #Vite #JavaScript #ErrorHandling

## 💡 오답 노트 및 성찰

오늘 TypeScript 개발 과정에서 발생한 `Uncaught SyntaxError` 트러블슈팅 내역을 정리합니다.

### 1. "Uncaught SyntaxError: ... does not provide an export named..."

- **기존 생각**: TypeScript의 `interface`나 `type`을 `import`할 때 일반 변수나 함수와 동일하게 가져와도 빌드 도구가 알아서 처리해줄 것이라고 생각했습니다.
- **바로잡은 내용**: TypeScript의 `interface`나 `type`은 컴파일 후 사라지는 '타입 전용' 요소입니다. `import { Item }`과 같이 가져오면 런타임 환경(브라우저)은 이를 실제 값이 있는 변수로 오해하고 찾으려다 에러를 발생시킵니다.
- **핵심**: 컴파일 타임에만 존재하는 타입 요소는 `import type` 혹은 `import { type ... }` 문법을 사용하여 타입임을 명시해야 합니다.

## 🔑 주요 학습 내용 (Key Concepts)

### 1. 원인 분석 (Why?)

- **자바스크립트의 특성**: 브라우저에서 실행되는 자바스크립트는 타입을 알지 못합니다.
- **TypeScript의 빌드 과정**: 빌드 과정에서 `interface`와 `type` 정의는 제거됩니다.
- **에러의 원인**: 빌드 후 삭제된 요소를 일반 변수처럼 참조하려고 하니, 런타임에서 "해당 이름의 export가 없다"는 에러가 발생한 것입니다.

### 2. 해결 방법 (Solution)

`import` 시 `type` 키워드를 추가하여 명시해줍니다.

**수정 전 (에러 발생)**

```typescript
// 브라우저는 NotificationItem이 실제 값인 줄 알고 찾으려 함
import { NotificationPopover, NotificationItem } from "./...";
```

**수정 후 (해결)**

```typescript
// 브라우저에게 "이건 타입이니 런타임에서 찾지 마라"라고 명시
import { NotificationPopover, type NotificationItem } from "./...";
```

## 📌 향후 학습 계획

- TypeScript의 컴파일 결과물(Transpiled JavaScript)이 어떤 모습인지 지속적으로 확인하며 런타임 에러를 방지하겠습니다.
- ESLint 규칙(e.g., `@typescript-eslint/consistent-type-imports`)을 활용하여 타입 전용 import를 자동화하는 방법을 도입해 보겠습니다.
