---
title: "copyByValue,copyByReference"
excerpt: "copyByValue와 copyByReference에 대해서 알아보자"

categories:
  - VanillaJS
tags:
  - [copyByValue, copyByReference, 기본형, 참조형]

permalink: /categories3/VanillaJS-2/

toc: true
toc_sticky: true

date: 2025-03-29
last_modified_at: 2025-03-29
---

## 🦥 본문

# 📦 자바스크립트의 값 복사 vs 참조 복사

이 문서는 JavaScript에서 **기본형(primitive)** 과 **참조형(reference)** 데이터가 변수에 할당될 때 어떻게 동작하는지를 설명합니다.

---

## 🧪 Copy by Value (값에 의한 복사)

```js
let original = "original";
let copy = "original";

console.log(original); // "original"
console.log(copy); // "original"
console.log(original === copy); // true
console.log("----------------------------------");

copy += "copy";

console.log(original); // "original"
console.log(copy); // "originalcopy"
console.log(original === copy); // false
```

### ✅ 설명

- `Number`, `String`, `Boolean`, `Undefined`, `Null`, `Symbol`, `BigInt` 와 같은 **기본형 데이터**는 값을 그대로 복사함.
- 그래서 한 변수를 수정해도 다른 변수에는 영향을 주지 않음.

---

## 🧬 Copy by Reference (참조에 의한 복사)

```js
let originalObj = {
  name: "name",
  value: "value",
};
let copyObj = originalObj;

console.log(originalObj);
console.log(copyObj);
console.log(originalObj === copyObj); // true

originalObj["team"] = "team";

console.log(originalObj);
console.log(copyObj);
console.log(originalObj === copyObj); // true
```

### ✅ 설명

- 객체(Object), 배열(Array), 함수(Function) 등은 **참조형 데이터**로 주소(reference)가 복사됨.
- 따라서 한 곳에서 값을 바꾸면 **모든 참조가 동시에 변경됨**.

---

## 🧾 요약

| 구분         | 값 복사 (Copy by Value) | 참조 복사 (Copy by Reference)       |
| ------------ | ----------------------- | ----------------------------------- |
| 데이터 종류  | 기본형 (primitive)      | 참조형 (object, array, function 등) |
| 변수 간 영향 | 없음                    | 있음                                |
| 비교 시      | 값 자체 비교            | 주소 비교 (같은 객체인지)           |

---
