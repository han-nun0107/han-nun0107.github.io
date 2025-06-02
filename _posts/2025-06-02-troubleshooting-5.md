---
title: "movie web Troubleshooting (3)"
excerpt: "movie web Troubleshooting"

categories:
  - Troubleshooting
tags:
  - [movie, troubleshooting]

permalink: /categories8/Troubleshooting-5/

toc: true
toc_sticky: true

date: 2025-06-02
last_modified_at: 2025-06-02
---

## 🦥 본문

# 🖼️ Supabase 프로필 이미지 변경 – 로컬 파일 적용 불가 문제

---

## ✅ 1. 문제 상황

- 사용자의 이름을 바꿀 때 `updateUser()`를 사용하여 성공적으로 구현함.
- 같은 방식으로 프로필 사진도 `updateUser()`를 사용하여 바꾸려고 했음.
- 웹 이미지 URL을 넣으면 정상 작동하지만, `input type="file"`을 통해 로컬 파일을 넣으면 적용되지 않음.

---

## ✅ 2. 원인 분석

- `updateUser()`는 Supabase `auth.users`의 메타데이터를 수정하는 함수이며, `avatar_url`에는 문자열(URL)만 허용됨.
- `input type="file"`로 선택한 값은 File 객체로, 실제 경로는 로컬 시스템 경로(`C://\...`)이거나 브라우저에서 생성된 임시 URL임.

---

## ✅ 3. 시도한 방법

- `input file`로 이미지 파일을 선택하고 `URL.createObjectURL(file)`을 통해 임시 URL을 생성하여 `updateUser()`에 전달.
- 처음에는 화면상에서 이미지가 변경된 것처럼 보였으나, 강력 새로고침(ctrl + shift + F5) 또는 시간이 지나면 이미지가 사라짐.
- 브라우저 임시 URL은 캐시 기반이며, 지속적인 저장에는 적합하지 않음.

---

## ✅ 4. 최종 해결법 및 조치

- Supabase Storage에 이미지를 업로드하고, 해당 이미지의 public URL을 추출하여 `updateUser()`의 `avatar_url`에 반영.

```js
export const handleChangeImage = async (
  uploadImg,
  setUserInfo,
  userInfo,
  setChangeImg
) => {
  if (!uploadImg) {
    toast.warn("사진을 선택 해주세요", { toastId: "ChangeImg" });
    return;
  }

  const fileExt = uploadImg.name.split(".").pop();
  const fileName = `${Date.now()}.${fileExt}`;
  const filePath = `avatars/${fileName}`;

  const { error: storageImg } = await supabase.storage
    .from("avatars")
    .upload(filePath, uploadImg, {
      cacheControl: "3600",
      upsert: false,
    });
  if (storageImg) {
    toast.error("이미지 업로드 실패");
    return;
  }
  toast.success("이미지 업로드 성공");

  const { data: urlData } = supabase.storage
    .from("avatars")
    .getPublicUrl(filePath);
  const publicUrl = urlData?.publicUrl;
  if (!publicUrl) {
    toast.error("스토리지에서 이미지 가져오기 실패");
    return;
  }

  const { error: ChangeImgErr } = await supabase.auth.updateUser({
    data: { avatar_url: publicUrl },
  });
  if (ChangeImgErr) {
    toast.error("이미지 변경 실패");
  }
  setUserInfo({ ...userInfo, avatar_url: publicUrl });
  setChangeImg("");
};

export const handleUserProfileChange = (e, { setUploadImg }) => {
  const { files } = e.target;
  const uploadFile = files[0];
  setUploadImg(uploadFile);
};
```

---

## ✅ 5. 회고 및 기록 이유

- 처음에는 `updateUser()`로 이미지도 닉네임과 같이 바로 변경될 줄 알았지만, 로컬 파일은 업로드 없이 사용할 수 없다는 점을 알게 되었음.
- `createObjectURL()`은 임시 URL(시간이 지나거나 캐시 삭제 시 소멸)이라 사용자 프로필처럼 **영속적으로 유지돼야 할 값에는 부적합**함.
- Supabase에서는 Storage 업로드 → Public URL 생성 → metadata 업데이트라는 **단계적인 이미지 처리 흐름**이 필요하다는 것을 이번에 정확히 이해했고, 이후에도 이 구조를 재활용할 수 있도록 기록해 둠.
