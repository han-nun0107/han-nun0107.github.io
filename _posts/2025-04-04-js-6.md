---
title: "고차 함수 콜백의 개념 설명 "
excerpt: "고차 함수는 함수를 인자로 받고, 콜백 함수는 그 인자로 전달되어 실행되는 함수. map과 repeat를 예로 들어 헷갈릴 수 있는 호출 구조를 상세하게 정리했습니다."

categories:
  - VanillaJS
tags:
  - [callback, Higher-order function, VanillaJS, 고차 함수 콜백]

permalink: /categories3/VanillaJS-6/

toc: true
toc_sticky: true

date: 2025-04-04
last_modified_at: 2025-04-04
---

## 🦥 본문

# 🧠 고차 함수와 콜백 함수의 관계

자바스크립트에서는 **함수를 인자로 받는 함수**를 **고차 함수(Higher-Order Function)**라고 하고,  
그 안에서 실행되는 **전달된 함수**를 **콜백 함수(Callback Function)**라고 부릅니다.

---

## 🔁 고차 함수의 대표 예: `map`

```js
const numbers = [1, 2, 3];
const doubled = numbers.map(function (item) {
  return item * 2;
});

console.log(doubled);
```

### ✅ 콘솔 출력:

```
[2, 4, 6]
```

### ✅ 이 코드에서 일어나는 일

1. `numbers.map(...)`을 내가 직접 호출함
2. `map`은 내부에서 배열 `[1, 2, 3]`을 순회하며, 전달받은 함수를 요소마다 호출함
3. `function(item) { return item * 2; }` 이 함수는 내가 직접 호출한 것이 아님
4. **→ 그래서 이 함수는 콜백 함수가 됨**

📌 다시 말해:

> 내가 호출한 건 `map`이고, **콜백 함수는 `map`이 호출했기 때문에**  
> `function(item) { return item * 2; }`는 콜백 함수입니다.

---

## ❓ 왜 콜백 함수인가?

콜백 함수는 **“내가 실행하는 게 아니라, 다른 함수가 대신 실행하는 함수”**입니다.

함수를 변수처럼 다른 함수에 넘겨줄 수 있는 자바스크립트의 특징 덕분에,  
이런 **고차 함수 ↔ 콜백 함수** 관계가 가능합니다.

---

## ✅ 사용자 정의 함수와 콜백의 구조

```js
function repeat(n, f) {
  for (let i = 0; i < n; i++) {
    f(i); // 전달받은 함수 f를 반복마다 실행
  }
}

const logAll = function (i) {
  console.log(i);
};

repeat(5, logAll);
```

### ✅ 콘솔 출력:

```
0
1
2
3
4
```

### 🧩 이 코드 구조 설명

- `repeat`는 내가 만든 **사용자 정의 함수(custom function)**입니다.
- `repeat`는 인자로 받은 `f`를 내부에서 실행합니다.
- 이때 넘긴 `logAll` 함수는 직접 실행하지 않았고, `repeat` 내부에서 실행되므로 **콜백 함수**가 됩니다.
- `repeat`는 콜백을 인자로 받아 사용하는 **고차 함수**입니다.

---

## 🔍 고차 함수 콜백 vs 사용자 정의 함수 콜백

| 항목      | map 사용 예                       | 사용자 정의 함수 예                  |
| --------- | --------------------------------- | ------------------------------------ |
| 고차 함수 | 내장 함수 `map`                   | 직접 만든 `repeat`                   |
| 콜백 함수 | 익명 함수 `(item) => item * 2`    | `logAll`, `logOdds`                  |
| 호출 주체 | `map()` 호출 → 내부에서 콜백 호출 | `repeat()` 호출 → 내부에서 콜백 호출 |
| 콜백 실행 | `map`이 실행함                    | `repeat`가 실행함                    |

---

## 🧠 핵심 개념 요약

- **고차 함수(Higher-Order Function)**  
  함수를 인자로 받거나, 함수를 반환하는 함수  
  예: `map`, `filter`, `setTimeout`, `repeat` 등

- **콜백 함수(Callback Function)**  
  고차 함수에 인자로 전달되어, **내가 아닌 다른 함수가 나중에 호출**하는 함수

- **내가 호출한 건 map/repeat이지만, 콜백 함수는 내가 아닌 map/repeat이 대신 호출한다!**

---

## ✅ 결론

- `map`을 호출하면 내부에서 전달된 함수를 대신 실행합니다.
- 그렇기 때문에 직접 넘긴 함수는 **콜백 함수**가 됩니다.
- `repeat`도 같은 원리로, 커스텀 고차 함수로 동작합니다.

> 💬 **“내가 직접 호출한 함수가 아니라, 다른 함수가 대신 호출한 함수 = 콜백 함수”**  
> 이 개념을 기억하면 됩니다.

---
