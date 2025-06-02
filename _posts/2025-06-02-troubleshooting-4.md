---
title: "movie web Troubleshooting (2)"
excerpt: "movie web Troubleshooting"

categories:
  - Troubleshooting
tags:
  - [movie, troubleshooting]

permalink: /categories8/Troubleshooting-4/

toc: true
toc_sticky: true

date: 2025-06-02
last_modified_at: 2025-06-02
---

## 🦥 본문

# 🔐 Supabase SNS 로그인 후 사용자 테이블에 데이터가 들어오지 않는 문제

---

## ✅ 1. 문제 상황

- Supabase로 SNS 로그인(Google, Kakao 등)을 구현한 뒤, `auth.users`에는 사용자 정보가 정상적으로 저장됨.
- 하지만 로그인한 사용자의 정보가 커스텀 테이블(`user_table`, `user_profile` 등)에 자동으로 저장되지 않아 이후 기능에서 사용자 정보가 비어 있음.
- 예: 좋아요 기록, 프로필 출력, 유저 기반 페이지가 작동하지 않음.

---

## ✅ 2. 원인 분석

- Supabase는 `auth.users`만 자동으로 관리하는 인증 전용 테이블임.
- 개발자가 생성한 `user_table` 같은 커스텀 테이블은 자동으로 insert 되지 않음.
- 따라서 SNS 로그인을 해도 커스텀 테이블에는 아무 데이터가 들어가지 않음.
- 인증과 사용자 데이터 저장은 명확히 분리되어 설계되어 있음.

---

## ✅ 3. 시도한 해결 방법

- 로그인 이후 사용자 정보를 커스텀 테이블에 저장하는 `upsert` 코드 작성
- 중복 저장 방지를 위해 `onConflict: "id"` 설정

```js
const {
  data: { user },
} = await supabase.auth.getUser();

await supabase.from("user_table").upsert(
  [
    {
      id: user.id,
      email: user.email,
      name: user.user_metadata.name,
      avatar_url: user.user_metadata.avatar_url.replace(
        /^http:\/\//,
        "https://"
      ),
    },
  ],
  { onConflict: "id" }
);
```

- 또는 `useEffect` 내에서 자동 실행되도록 훅 구성

```js
useEffect(() => {
  const syncUser = async () => {
    const {
      data: { session },
    } = await supabase.auth.getSession();
    const user = session?.user;
    if (!user) return;

    await supabase.from("user_table").upsert(
      [
        {
          id: user.id,
          email: user.email,
          name: user.user_metadata.name,
          avatar_url: user.user_metadata.avatar_url,
        },
      ],
      { onConflict: "id" }
    );
  };

  syncUser();
}, []);
```

---

## ✅ 4. 최종 해결책 및 조치

- 로그인 직후 사용자 정보를 수동으로 커스텀 테이블에 insert/upsert
- `user.id` 기준으로 중복 방지 처리 적용
- 아바타 URL의 `http` → `https` 정제
- 전역 상태에 사용자 정보를 저장하여 재사용 가능하도록 처리

---

## ✅ 5. 회고 및 기록 이유

> - Supabase에서 `auth.users`는 인증 전용 테이블이고, 커스텀 유저 테이블은 자동으로 채워지지 않는다는 점을 놓쳐 처음엔 원인을 파악하지 못했음.
> - 이 문제를 해결하면서 Supabase의 인증과 데이터 관리의 구조적 분리를 이해하게 되었고, 실무에서 커스텀 사용자 데이터를 다룰 때 필수적인 로직임을 체감함.
> - 향후 SNS 로그인 기반 프로젝트를 진행할 때도 재사용 가능한 구조이므로, 이 기록을 통해 나중에 동일한 문제를 빠르게 해결할 수 있도록 함.
