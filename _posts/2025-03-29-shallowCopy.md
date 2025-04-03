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

> JavaScript에서 `Object.assign()`이나 전개 연산자(`...`)를 이용한 **얕은 복사(shallow copy)**는 객체를 복사하지만, 참조형 데이터의 경우 동일한 주소를 공유하게 됩니다. 이 문서는 얕은 복사의 개념과 예제를 출력 결과와 함께 정리합니다.

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

## 🖨️ 출력 결과

```bash
원본: { name: '홍길동', age: 30, family: [ '아버지', '어머니', '나' ] }
얕은 복사본1: { name: '홍길동', age: 30, family: [ '아버지', '어머니', '나' ] }

원본: { name: '홍길동', age: 30, family: [ '아버지', '어머니', '나' ] }
얕은 복사본1: { name: '홍길동', age: 31, family: [ '아버지', '어머니', '나' ] }
```

---

## 🧾 요약

| 구분      | 얕은 복사 (Shallow Copy)        |
| --------- | ------------------------------- |
| 기본형    | 값 자체 복사 (독립적)           |
| 참조형    | 주소 공유 (연결됨)              |
| 복사 방식 | 전개 연산자(...), Object.assign |
| 주의사항  | 참조형은 변경 시 원본에도 영향  |

---
