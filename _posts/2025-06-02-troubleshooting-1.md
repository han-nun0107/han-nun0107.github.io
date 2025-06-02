---
title: "Weather web Troubleshooting (1)"
excerpt: "Weather web Troubleshooting"

categories:
  - Troubleshooting
tags:
  - [weather, troubleshooting]

permalink: /categories8/Troubleshooting-1/

toc: true
toc_sticky: true

date: 2025-06-02
last_modified_at: 2025-06-02
---

## 🦥 본문

# 🌤️ 날씨 웹 프로젝트 – 트러블슈팅 기록

---

## ✅ 오늘 날씨 필터링 문제

### 1. 문제 상황

- **기능 목표**: OpenWeather API에서 받아온 5일치 예보 데이터를 활용해 오늘 하루의 날씨만 시각적으로 보여주고자 함.
- **데이터 형식**: 3시간 간격 5일치 api를 가져옴
- **마주한 문제**: 시간이 지나면 배열 안에 있는 데이터들이 자동으로 삭제되기 때문에 정확한 데이터 수집을 못하였음.

### 2. 원인 분석

- **예상**: 안에 있는 0번 배열의 시간을 봤을 때 시간이 우리나라랑 다른 것을 확인하였기 때문에 UTC 기준 시간 때문이라 예상
- **실제 원인**: API 기준은 UTC로 되어 있기 때문에 단순히 slice(0,8)이나 filter()를 사용해서는 내가 원하는 데이터를 못 가져옴

### 3. 시도한 방법

1. `slice(0,8)`을 사용하여 0번부터 7번까지 잘라서 확인해봄. 이 또한 UTC 기준이라 우리나라 시간과 달라 실패
2. `filter()`를 기존과 같이 사용하지만 filter 안에 한국 시간을 넣어주고 비교를 하는 방식으로 변경

### 4. 최종 해결책 및 조치

KST 기준으로 시간을 바꾼 js 함수와, API에서 받은 `dt_txt`를 한국 시간으로 변경한 후 엄격한 동등 연산자 `===`을 사용하여 비교

```js
.filter((item) => {
  const utcDate = new Date(item.dt_txt);
  const koreaDate = new Date(
    utcDate.getTime() + 9 * 60 * 60 * 1000
  );

  const year = koreaDate.getFullYear();
  const month = String(koreaDate.getMonth() + 1).padStart(2, "0");
  const day = String(koreaDate.getDate()).padStart(2, "0");

  const koreaDateString = `${year}-${month}-${day}`;

  return koreaDateString === today();
})
```

```js
export function today() {
  const nowDate = new Date();
  const year = nowDate.getFullYear();
  const month = String(nowDate.getMonth() + 1).padStart(2, "0");
  const date = String(nowDate.getDate()).padStart(2, "0");
  return `${year}-${month}-${date}`;
}
```

### 5. 회고 및 기록 이유

- 처음 UTC 기준인 것을 정확하게 파악하지 못하고 그냥 만들었다가 시간을 소비하게 됨.
- 날씨 웹을 만들면서 시간대(UTC, KST) 개념을 이해하는 데 도움이 되었음.
- 단순하게 데이터만 가져오는 것이 아니라 프론트에서 시간 데이터를 처리하는 방법에 대해 배우게 되었음.
- 실무에서도 충분히 이런 상황이 발생할 수 있기 때문에 문서화하여 나중에 참고하기 위해 기록하게 되었음.

---
