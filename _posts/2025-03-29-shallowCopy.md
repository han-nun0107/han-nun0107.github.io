---
title: "Shallow Copy란? "
excerpt: "기본형은 독립적, 참조형은 연결된다? 얕은 복사의 개념과 동작 방식을 실제 코드로 알아봅니다."

categories:
  - VanillaJS
tags:
  - [ShallowCopy, 얕은복사]

permalink: /categories3/VanillaJS-3/

toc: true
toc_sticky: true

date: 2025-03-31
last_modified_at: 2025-03-31
---

## 🦥 본문

# 🧬 자바스크립트의 얕은 복사 (Shallow Copy)

이 문서는 JavaScript에서 `Object.assign()`이나 전개 연산자(`...`)를 이용한 **얕은 복사(shallow copy)**의 개념을 설명하고 예제와 함께 정리합니다.

---

## 📦 예제 코드

```js
let original = {
  name: "홍길동",
  age: 30,
  family: ["아버지", "어머니"],
};

let shallowCopy1 = { ...original };

let shallowCopy2 = Object.assign({}, original);

shallowCopy1.family.push("나");

console.log("원본:", original);
console.log("얕은 복사본1:", shallowCopy1);

shallowCopy1.age = 31;

console.log("원본:", original);
console.log("얕은 복사본1:", shallowCopy1);
```

---

## ✅ 설명

- `Object.assign()`과 전개 연산자(`...`)는 객체를 **얕게 복사**합니다.
- **기본형(primitive)** 속성들은 값 자체가 복사됩니다.
- 하지만 **참조형(reference)** 속성들 (배열, 객체 등)은 **같은 주소를 공유**합니다.
- 그래서 복사본에서 참조형 데이터를 변경하면 원본도 영향을 받습니다.

---

## 🔍 예시 요약

- `shallowCopy1.family.push("나")` → `original.family`도 변경됨.
- `shallowCopy1.age = 31` → 원본은 변하지 않음 (기본형이라 독립적).

---

## 💡 참고

- 깊은 복사(deep copy)가 필요한 경우에는 `structuredClone()`, `lodash.cloneDeep()`, JSON 방식 등을 사용해야 합니다.

---
