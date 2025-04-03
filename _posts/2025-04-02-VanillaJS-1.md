---
title: "Hoisting과 TDZ"
excerpt: "Hoisting과 TDZ에 대해 설명"

categories:
  - VanillaJS
tags:
  - [Hoisting, TDZ]

permalink: /categories3/VanillaJS-1/

toc: true
toc_sticky: true

date: 2025-03-29
last_modified_at: 2025-03-29
---

## 🦥 본문

# 📚 호이스팅과 Temporal Dead Zone(TDZ)

이 문서는 JavaScript의 호이스팅(Hoisting)과 TDZ(Temporal Dead Zone)에 대한 개념과 예제를 설명합니다.

---

## 🧠 TDZ(Temporal Dead Zone)란?

`let`과 `const`로 선언된 변수는 **선언 전에 접근하면 오류가 발생하는 영역**, 즉 TDZ에 존재하게 됩니다.

---

## 🧪 예제 코드 및 설명

### ✅ `var`의 호이스팅 예시

```js
console.log(x);
var x = 10;
```

- 이 경우 출력 결과는 `undefined`입니다.
- 왜냐하면 JavaScript에서는 **변수 선언부가 위로 끌어올려지는 "호이스팅"**이 발생하기 때문입니다.
- 실제로는 아래와 같이 동작합니다:

```js
var x;
console.log(x); // undefined
x = 10;
```

---

### ⚠️ `let`의 TDZ 예시

```js
console.log(b);
let b = 10;
```

- 출력 결과: ❌ `ReferenceError: Cannot access 'b' before initialization`
- 이유: `let`도 내부적으로는 호이스팅되지만 **초기화 전에는 접근할 수 없도록 막는 TDZ**가 적용됩니다.

---

### ⚠️ `const`도 동일한 TDZ 발생

```js
console.log(c);
const c = 10;
```

- 출력 결과: ❌ `ReferenceError` (let과 동일)
- `const` 또한 호이스팅은 되지만 TDZ 영역에 위치해 초기화 전 접근은 금지됩니다.

---

## 🔍 `let`과 `const`의 차이점

| 항목           | `let` | `const`                     |
| -------------- | ----- | --------------------------- |
| 선언 후 초기화 | 가능  | 반드시 선언과 동시에 초기화 |
| 재할당         | 가능  | 불가능                      |
| TDZ 발생 여부  | 있음  | 있음                        |

---

## 🧾 요약

- `var`: 선언이 호이스팅되고 초기화 전 접근 시 `undefined`
- `let`, `const`: 호이스팅은 되지만 TDZ로 인해 초기화 전 접근 시 **ReferenceError** 발생
- `const`: 선언과 동시에 초기화가 필수

---

> ✅ **Tip:** `const`는 선언 즉시 초기화를 요구하므로 TDZ를 상대적으로 더 쉽게 회피할 수 있습니다.
