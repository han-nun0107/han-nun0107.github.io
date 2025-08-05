---
title: "Aws Routing Error (403,404)"
excerpt: "Aws Routing Error"

categories:
  - Troubleshooting
tags:
  - [Aws, troubleshooting]

permalink: /categories8/Troubleshooting-6/

toc: true
toc_sticky: true

date: 2025-08-05
last_modified_at: 2025-08-05
---

## 🦥 본문

# 🖥️ AWS S3 + CloudFront 환경에서 React SPA 배포 시 `/test` 경로 접근 403/404 오류 해결

---

## ✅ 1. 문제 상황

- React SPA를 AWS S3 + CloudFront로 배포함.
- 루트 경로(`/`) 접근은 잘 되지만, 주소창에 `/test`, `/search` 등 내부 라우팅 경로를 직접 입력하면 `AccessDenied` 혹은 `NotFound` 오류 발생.
- 실제 에러 메시지:

```xml
<Error>
  <Code>AccessDenied</Code>
  <Message>Access Denied</Message>
</Error>
```

```bash
GET https://moeun.kro.kr/test 403 (Forbidden)
GET https://moeun.kro.kr/favicon.ico 403 (Forbidden)
```

---

## ✅ 2. 원인 분석

- S3는 정적 파일만 제공하고, 서버 사이드 라우팅이 없어 직접 경로로 접근 시 `index.html`을 반환하지 않음.
- React SPA는 `react-router` 등의 클라이언트 라우팅을 사용하므로, 모든 경로 요청이 `index.html`로 리디렉션되어야 정상 작동.
- S3는 이 처리를 하지 않기 때문에 CloudFront + 리디렉션 설정이 필요함.

---

## ✅ 3. 시도한 방법

### 🔹 1) S3 버킷 리디렉션 규칙 추가

S3 버킷 → 속성 → 정적 웹 사이트 호스팅 → 리디렉션 규칙:

```json
[
  {
    "Condition": {
      "HttpErrorCodeReturnedEquals": "404"
    },
    "Redirect": {
      "HostName": "bucketName.s3-website-ap-northeast-2.amazonaws.com",
      "ReplaceKeyPrefixWith": ""
    }
  }
]
```

- 404 에러가 발생하면 S3 호스팅 주소로 리디렉션
- `ReplaceKeyPrefixWith: ""` 설정을 통해 모든 요청을 `index.html`로 보냄

---

### 🔹 2) CloudFront 사용자 지정 오류 응답 설정

CloudFront → 배포 선택 → 오류 페이지 탭 → 오류 응답 생성:

| 항목                  | 값            |
| --------------------- | ------------- |
| HTTP 오류 코드        | 403, 404      |
| 사용자 지정 오류 응답 | 예            |
| 응답 페이지 경로      | `/index.html` |
| HTTP 응답 코드        | `200 OK`      |

- CloudFront에서 경로를 못 찾는 경우에도 `index.html`을 반환하도록 설정

---

## ✅ 4. 최종 조치 및 결과

- GitHub Actions로 배포 자동화가 되어 있으므로, 설정 적용 후 `Re-run all jobs`로 CI/CD 재실행
- `/test`, `/search` 등 SPA 내부 경로도 정상 진입 가능 확인

---

## ✅ 5. 회고 및 기록 이유

- S3는 정적 파일 호스팅이므로 직접 경로 접근 시 `index.html`을 리턴하지 않음 → 클라이언트 라우팅이 안 됨.
- 이 문제는 CloudFront + 사용자 지정 오류 응답을 통해 해결해야 한다는 점을 확실히 이해함.
- 리디렉션 규칙과 CloudFront 오류 페이지 설정을 정확히 구성하면 SPA 라우팅 문제 없이 완벽하게 대응 가능.
- 추후 다른 SPA 프로젝트에서도 동일한 구조를 재활용할 수 있도록 이 흐름을 기록함.
