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

> JavaScript에서 `Object.assign()`이나 전개 연산자(`...`)를 이용한 **얕은 복사(shallow copy)**는 객체를 복사하지만, 참조형 데이터의 경우 동일한 주소를 공유하게 됩니다. 이 문서는 얕은 복사의 개념과 예제, 기본형(primitive)과 참조형(reference)의 차이를 함께 설명합니다.

---

## 🧠 기본 개념

### ✅ 기본형(Primitive)

- 값 자체가 저장됨
- 복사 시 **완전히 독립적인 값**이 생성됨
- 종류: `string`, `number`, `boolean`, `undefined`, `null`, `symbol`, `bigint`

### ✅ 참조형(Reference)

- 값이 아닌 **주소(참조값)** 가 저장됨
- 복사 시 같은 주소를 가리키게 되어 **한쪽을 바꾸면 다른 쪽도 바뀜**
- 예: `object`, `array`, `function`

---

## 📦 예제 코드

```js
let original = {
  name: "홍길동", // 기본형 (string)
  age: 30, // 기본형 (number)
  family: ["아버지", "어머니"], // 참조형 (array)
};

let shallowCopy1 = { ...original };

let shallowCopy2 = Object.assign({}, original);

// 참조형 값 변경
shallowCopy1.family.push("나");

// 기본형 값 변경
shallowCopy1.age = 31;

console.log("원본:", original);
console.log("얕은 복사본1:", shallowCopy1);
```

---

## ✅ 설명

- `name`과 `age`는 **기본형**이라서 값이 그대로 복사되고 독립적으로 존재합니다.
- `family`는 **참조형(배열)**이라서 같은 주소를 공유하게 됩니다.
- 따라서 복사본에서 `family`에 값을 추가하면 원본도 함께 변경됩니다.

---

## 🖨️ 출력 결과

```bash
원본: { name: '홍길동', age: 30, family: [ '아버지', '어머니', '나' ] }
얕은 복사본1: { name: '홍길동', age: 31, family: [ '아버지', '어머니', '나' ] }
```

- `age`는 복사본에서 바꿨지만 원본은 그대로 (✅ 기본형이기 때문)
- `family`는 복사본에서 바꾸자 원본도 같이 바뀜 (⚠️ 참조형이기 때문)

---

## 🧾 요약

| 타입 구분          | 설명                    | 복사 시 특징         |
| ------------------ | ----------------------- | -------------------- |
| 기본형 (Primitive) | 숫자, 문자열 등 단순 값 | 완전 복사 (독립)     |
| 참조형 (Reference) | 객체, 배열 등           | 주소만 복사 (연결됨) |

---

## 🧠 기억할 점

- 기본형은 `값 자체`를 복사하므로 **안전하게 복사 가능**
- 참조형은 `주소(참조)`만 복사되므로 **원본과 복사본이 연결됨**
- 얕은 복사는 **1단계까지만 복사**되므로 중첩된 객체에는 영향을 줌

---
