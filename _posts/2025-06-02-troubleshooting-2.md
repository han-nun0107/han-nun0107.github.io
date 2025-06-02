---
title: "weather web Troubleshooting (2)"
excerpt: "weather web Troubleshooting"

categories:
  - Troubleshooting
tags:
  - [weather, troubleshooting]

permalink: /categories8/Troubleshooting-2/

toc: true
toc_sticky: true

date: 2025-06-02
last_modified_at: 2025-06-02
---

## 🦥 본문

# 🌤️ 날씨 웹 프로젝트 – 트러블슈팅 기록

---

## ✅ 한글 지역 검색 문제 (ID 매핑)

### 1. 문제 상황

- **기능 목표**: OpenWeather API를 사용하면 검색 기능에서 한국어 지원이 안 되는 것을 알 수 있음. 이를 해결하고자 한국어 검색이 가능하게 수정하는 작업을 진행하게 됨.
- **데이터 방식**: city ID를 기반으로 날씨를 조회하는 방식이며 해당 ID 값은 문서에서 제공됨.
- **직면한 문제**: OpenWeather API는 한국어 검색 기능을 제공하지 않음 → 한글 → ID로 매핑이 필요

### 2. 원인 분석

- OpenWeather API는 한글 지역명을 직접 지원하지 않음.
- 도시 이름 중 중복되는 값도 있기 때문에 정확한 ID 값을 사용하는 것이 권장 방식임.
- 한글 입력을 하면 매핑되어 있는 ID 값이 들어가게 해야 함.

### 3. 시도한 방법

- 서칭을 하다보니 날씨 이름을 바꾸는 매핑 테이블을 발견. 이것도 마찬가지로 `'지역명': id`로 매핑해서 사용하면 되지 않을까 생각함.
- 따라서 제공되는 지역명과 아이디를 위와 같은 방식으로 js 파일에 매핑함.

```js
const InputName = {
  바그다드: 98182,
  메카: 5371965,
  제다: 105343,
  리야드: 108410,
  메디나: 5803092,
  ...
};
```

- 사용자가 한글로 입력한 값으로 ID 값을 찾아 API에 요청하도록 구현

### 4. 최종 해결법

입력 → ID 매핑 → API 요청 흐름

```js
export function useWeaklyWeather(city, apiKey, setWeather, setError) {
  const fetchWeather = async () => {
    const cityID = InputName[city];
    if (!cityID) {
      setError("지원하지 않는 지역입니다.");
      setWeather(null);
      return;
    }
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/forecast?id=${cityID}&appid=${apiKey}&units=metric&lang=kr`
      );
      const data = await response.json();

      if (data.error || data.cod !== "200") {
        setError(data.message || "데이터 오류");
        setWeather(null);
      } else {
        setWeather(data);
        setError(null);
      }
    } catch (error) {
      setError("날씨 데이터를 불러오는 중 오류 발생");
      setWeather(null);
      console.error(error);
    }
  };

  useEffect(() => {
    fetchWeather();
  }, [city, apiKey, setWeather, setError]);
}
```

### 5. 회고 및 기록 이유

- OpenWeather API는 다국어 지원이 제한적이라 사용자 친화적인 인터페이스 구현을 위해 중간 매핑이 필요하다는 것을 경험.
- 한글 검색을 가능하게 하기 위해 도시 이름과 ID 간 매핑을 구성하면서 실무에서의 데이터 처리 및 설계의 중요성을 체감.
- 처음에는 전체 도시 약 12만 개의 지역에 대해 한글 매핑을 시도했지만, 예상보다 데이터 양이 너무 많았고 양방향 매핑까지 하게 된다면 24만 줄에 달해 프론트 리소스를 크게 소모할 수 있음을 파악함.
- 성능과 실용성 측면을 고려해 주요 도시 위주로 일부 지역만 한국어 매핑을 진행하였으며, 웹 페이지 무게를 줄일 수 있는 현실적인 선택을 하게 되었음.
