---
title: "map 함수의 사용자 정의 구현 "
excerpt: "고차 함수는 함수를 인자로 받고, 콜백 함수는 그 인자로 전달되어 실행되는 함수. map과 repeat를 예로 들어 헷갈릴 수 있는 호출 구조를 상세하게 정리했습니다."

categories:
  - VanillaJS
tags:
  - [callback, map 함수의 사용자 정의 구현, VanillaJS]

permalink: /categories3/VanillaJS-5/

toc: true
toc_sticky: true

date: 2025-04-05
last_modified_at: 2025-04-05
---

## 🦥 본문

# 🧸 README: 쉽게 배우는 map 함수 만들기!

---

## 🎯 목표

- `map()`이라는 도구가 **무슨 일을 하는지** 알아보기
- 이 도구를 **직접 써보고**, **직접 만들어 보기**!

---

## 1️⃣ `map()`이 뭐예요?

```javascript
let newArr = [10, 20, 30].map(function (value, index) {
  console.log(value, index);
  return value * 2;
});

console.log(newArr);
```

### 📝 이건 무슨 뜻일까요?

- `[10, 20, 30]` → 숫자들이 들어 있는 상자예요.
- `map()` → 이 상자 안에 있는 숫자들 하나하나에게 **2를 곱하라고 시키는 도구**예요.
- 그래서 새 상자 `newArr`에는 `[20, 40, 60]`이 들어가요!

💡 중간에 `console.log(value, index)`는 지금 어떤 숫자와 몇 번째 숫자인지 **출력하는 거예요**.

---

## 2️⃣ map 도구를 내 손으로 만들어보자!

```javascript
Array.prototype.map = function (callback, thisArg) {
  var mappedArr = [];

  for (var i = 0; i < this.length; i++) {
    if (i in this) {
      mappedArr[i] = callback.call(thisArg || globalThis, this[i], i, this);
    }
  }

  return mappedArr;
};
```

---

## 🧠 아주 쉽게 설명해줄게요!

### 1. `Array.prototype.map = function(callback, thisArg)`

- "map"이라는 새로운 도구를 만들어요.
- `callback`은 숫자를 어떻게 바꿀지 알려주는 **작업 지시서**예요.

### 2. `var mappedArr = [];`

- 새 상자를 하나 만들어요! (바뀐 숫자들을 넣을 거예요)

### 3. `for (var i = 0; i < this.length; i++)` → 반복!

- 상자 안에 있는 숫자들을 하나씩 꺼낼 거예요.

### 4. `if (i in this)`

- 진짜 값이 있는 자리만 골라서 작업해요. (비어 있는 칸은 건너뛰어요!)

### 5. `mappedArr[i] = callback.call(...)`

- 꺼낸 숫자에게 일을 시키고 → 그 결과를 새 상자에 넣어요!

### 6. `return mappedArr;`

- 바뀐 숫자들이 들어있는 새 상자를 돌려줘요 🎁

---

## 🧪 예시 결과 보기

```javascript
let newArr = [10, 20, 30].map(function (value, index) {
  console.log(value, index);
  return value * 2;
});

console.log(newArr);
```

### 출력:

```
10 0
20 1
30 2
[20, 40, 60]
```

---

## 🎉 정리하면!

- `map()`은 숫자들을 하나씩 바꿔주는 마법 도구예요 ✨
- 우리도 직접 만들 수 있어요!
- 재미있게 코딩해보자! 💻💙

필요하면 다른 도구(`filter`, `forEach`)도 함께 배울 수 있어요!
