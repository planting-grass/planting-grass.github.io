---
title: 타입스크립트 타입
author: 유주성
date: 2026-03-11
category: TIL/유주성/2026/03
layout: post
---
오랫만에 타입 스크립트 보니 어지럽다.. 타입 설정을 어떻게 하는지 한번 더 보자.

## 1. 기본 타입 설정

가장 기본은 변수 선언 시 타입을 직접 지정하는 것이다.

```tsx
let userName: string = "jin";
let age: number = 20;
let isAdmin: boolean = false;
```

TypeScript의 대표적인 기본 타입은 `string`, `number`, `boolean` 이다.

타입을 지정하지 않아도 값으로부터 추론하는 경우가 많다.

```tsx
let city = "Seoul"; // string으로 추론됨
```

하지만 협업하거나 의도를 분명히 하고 싶을 때는 명시적으로 타입을 적는 것이 좋다.

---

## 2. 배열 타입 설정

배열은 `타입[]` 형태로 많이 쓴다.

```tsx
let numbers: number[] = [1, 2, 3];
let names: string[] = ["Tom", "Jane"];
```

제네릭 문법으로도 작성할 수 있다.

```tsx
let scores: Array<number> = [90, 80, 100];
```

---

## 3. 객체 타입 설정

객체는 속성별 타입을 지정할 수 있다.

```tsx
let user: { name: string; age: number } = {
  name: "Alice",
  age: 25,
};
```

속성이 많아지면 `type` 또는 `interface`로 분리해서 관리하는 게 좋다. TypeScript 문서에서도 객체 타입을 별도의 이름으로 관리하는 방식을 권장한다.

```
type User = {
  name: string;
  age: number;
};

const user1: User = {
  name: "Bob",
  age: 30,
};
```

또는

```
interface User {
  name: string;
  age: number;
}

const user2: User = {
  name: "Chris",
  age: 28,
};
```

---

## 4. 함수 타입 설정

함수는 **매개변수 타입**과 **반환값 타입**을 지정할 수 있다.

```
function add(a: number, b: number): number {
  return a + b;
}
```

화살표 함수도 동일하다.

```
const greet = (name: string): string => {
  return `Hello, ${name}`;
};
```

이렇게 하면 잘못된 인자를 넣었을 때 컴파일 단계에서 에러를 확인할 수 있다.

---

## 5. 선택적 속성 설정

객체 속성이 꼭 필요하지 않은 경우 `?`를 붙인다.

```
type Product = {
  name: string;
  price: number;
  description?: string;
};
```

`description`은 있어도 되고 없어도 된다.

---

## 6. Union 타입 설정

하나의 값이 여러 타입 중 하나일 수 있을 때 `|`를 사용한다.

```
let id: string | number;

id = "abc123";
id = 1001;
```

이런 타입은 조건문과 함께 많이 쓴다. TypeScript는 코드 흐름에 따라 타입을 좁혀가는 **narrowing**을 지원한다.

```
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id);
  }
}
```

---

## 7. type alias와 interface 차이

둘 다 객체 구조를 정의할 수 있지만, `type`은 union, tuple 같은 다양한 타입 조합에도 쓸 수 있고, `interface`는 객체 구조 확장에 강점이 있다. 공식 문서도 대부분의 경우 둘 다 가능하지만, `type`은 더 넓은 타입 표현에 사용할 수 있다고 설명한다.

```
type Status = "success" | "error" | "loading";
```

```
interface Animal {
  name: string;
}

interface Dog extends Animal {
  breed: string;
}
```

실무에서는 보통

- 객체 구조 중심이면 `interface`
- 유니온이나 복잡한 조합 타입이면 `type`
    
    처럼 많이 나눈다.
    

---

## 8. 제네릭으로 타입 재사용하기

같은 로직을 여러 타입에 재사용하고 싶을 때 제네릭을 사용한다. TypeScript 공식 문서에서도 재사용 가능한 타입 설계의 핵심으로 제네릭을 소개한다.

```
function getValue<T>(value: T): T {
  return value;
}

const num = getValue<number>(10);
const str = getValue<string>("hello");
```

이 방식은 함수, 객체, API 응답 타입 설계에서 자주 사용된다.

---

## 9. satisfies로 객체 검증하기

최근 TypeScript 문서에서는 `satisfies` 연산자를 통해 객체가 특정 타입 조건을 만족하는지 검증하면서도, 원래 값의 더 구체적인 타입 정보는 유지하는 방식을 소개한다.

```
type Colors = "red" | "green" | "blue";

const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
  blue: [0, 0, 255],
} satisfies Record<Colors, string | number[]>;
```

이 방법은 객체 키 누락이나 오타를 잡을 때 유용하다.