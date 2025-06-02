---
title: "movie web Troubleshooting (1)"
excerpt: "movie web Troubleshooting"

categories:
  - Troubleshooting
tags:
  - [movie, troubleshooting]

permalink: /categories8/Troubleshooting-3/

toc: true
toc_sticky: true

date: 2025-06-02
last_modified_at: 2025-06-02
---

## 🦥 본문

# ❤️ Like 기능 추가 (Supabase Table Editor 기반)

---

## ✅ 1. 문제 상황

- TMDB에서 받아온 데이터에 하트(좋아요) 버튼을 추가하고, 해당 버튼을 눌렀을 때 map으로 다시 풀어 좋아요 누른 영화만 따로 보여주고자 함.
- 각 영화 데이터는 고유한 `id`와 `title`, `overview`, `poster_path` 등의 정보를 포함하고 있음.
- 사용자가 좋아요 누른 영화만 필터링하여 출력되게 만들고자 했지만, 처음엔 어떻게 구현할지 감이 잡히지 않음.

---

## ✅ 2. 원인 분석

- 초기에는 하트를 누르면 로컬 상태 배열에 해당 영화의 ID를 추가하는 방식으로 처리함.
- 그러나 이 방식은 브라우저를 새로고침하면 상태가 초기화되기 때문에 유지되지 않음.
- 유저별로 좋아요 데이터를 저장하려면 **서버에 저장**해야 함.
- Supabase에서는 로그인 시 유저마다 **고유한 ID** (`user.id`)가 존재한다는 점을 떠올림.

---

## ✅ 3. 시도한 해결 방법

- Supabase에 `likes` 테이블 생성
  - 컬럼: `user_id`, `movie_id`, `movie_title`, `poster_path`, `vote_average`, `created_at`
- 사용자가 하트 버튼을 클릭하면 해당 영화 데이터를 `insert`로 `likes` 테이블에 저장
- 페이지 로드 시 또는 영화 ID가 변경될 때 현재 사용자가 해당 영화를 좋아요했는지 여부를 Supabase에서 확인

---

## ✅ 4. 최종 해결책 및 조치

### 🎯 1. 좋아요 상태 확인 (렌더링 시점)

```js
useEffect(() => {
  const checkLiked = async () => {
    if (!userInfo) return;

    const { data } = await supabase
      .from("likes")
      .select("id")
      .eq("user_id", userInfo.id)
      .eq("movie_id", id)
      .maybeSingle();

    if (data) setIsLiked(true);
  };
  checkLiked();
}, [userInfo, id]);
```

### 🎯 2. 좋아요 토글 기능 (하트 버튼 클릭 시)

```js
const handleLikeToggle = async () => {
  if (isLiked) {
    await supabase
      .from("likes")
      .delete()
      .eq("user_id", userInfo.id)
      .eq("movie_id", id);
    setIsLiked(false);
    toast.info("좋아요 취소");
  } else {
    await supabase.from("likes").insert({
      user_id: userInfo.id,
      movie_id: id,
      movie_title: title,
      poster_path: poster_path,
      vote_average: vote_average,
      created_at: koreaTime(),
    });
    setIsLiked(true);
    toast.success("좋아요 추가");
  }
};
```

---

## ✅ 5. 회고 및 기록 이유

- 초기에는 단순히 좋아요 버튼을 프론트 상태 관리로만 구현하려고 했지만, 새로고침을 하면 유지되지 않는다는 것을 직접 겪으면서 **서버에 저장해야 하는 이유**를 명확히 이해하게 되었음.
- Supabase의 테이블 설계를 활용하여 **사용자별 데이터 분리와 저장 구조**를 직접 구현해보았고, 실무에서도 실제로 사용될 수 있는 구조라는 점에서 의미가 있었음.
- 이 기능을 구현하면서, 단순한 버튼 하나에도 결국 **백엔드와 연결되는 구조적 사고**가 필요하다는 것을 느꼈고, 이 기록을 남김으로써 나중에 유사 기능을 다시 구현할 때 참고할 수 있기를 바람.
