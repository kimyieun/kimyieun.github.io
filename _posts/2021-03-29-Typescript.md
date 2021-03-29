---
title: "Typescript"

categories:
  - typescript

tags:
  - typescript
---

## Typescript

## Type Assertion

- 두가지의 Type Assertion 방법이 있다.

```typescript
export interface Person {
  name: string;
  age: number;
  occupation: string;
}

// #1
const john = {
  name: "John",
  age: 25,
} as Person;

// #2
const william = <Person>{
  name: "William",
  age: 25,
};
```

- **It weakens Type Safety**

```typescript
export interface Person {
  name: string;
  age: number;
  occupation: string;
}
//here we are casting the object literal to Person using the `as` syntax for type assertion
const john = {
  name: "John",
  age: 25,
} as Person;

//here we are casting the object literal to Person using the `ang-bracket` syntax of type assertion
const william = <Person>{
  name: "William",
  age: 25,
};
```

- type assertion 을 사용하면 occupation property 가 필수로 포함되어야함에도 불구하고, type error 가 발생하지 않는다.
- 그 이유는?
  - It is essentially telling typescript to **“stop type checking and trust me, I know what I’m doing”**.
  - 그래서 아래와 같이 type annotation 방식을 권장한다.(occupation property 가 없으면 type error 발생한다.)

```typescript
const william: Person = {
  name: "William",
  age: 25,
  occupation: "artist",
};
```

- type casting 과는 다르다.
  - type casting : compile time / runtime 에서 모두 타입을 변경시킨다.
  - type assertion : compile time 에서만 타입을 변경시킨다. runtime 에서 에러가 발생할 수 있다.
