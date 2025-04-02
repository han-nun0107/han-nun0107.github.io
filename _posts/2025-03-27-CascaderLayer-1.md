---
title: "Cascade-Layer"
excerpt: "협업 시 사용하는 Cascade-Layer"

categories:
  - CSS
tags:
  - [디자인, 협업, 디자인 우선순위]

permalink: /categories2/CSS-1/

toc: true
toc_sticky: true

date: 2025-03-27
last_modified_at: 2025-03-27
---

## 🦥 본문

# 🎨 CSS Cascade Layer 정리

[HTML 파일 보기](/assets/files/Cascade-Layers.html)

## 📌 개요

기존에는 CSS 우선순위가 다음 세 가지 기준에 따라 결정되었어요:

1. 선언 순서
2. 명시도 (specificity)
3. 중요도 (`!important`)

하지만 **Cascade Layer**는 이와 다르게, 개발자가 **명시적으로 스타일 그룹의 우선순위**를 지정할 수 있도록 도와주는 새로운 기능입니다.

---

## 🧩 어떻게 작동하나요?

### ▶️ `@layer`를 사용한 예시

```css
@layer base {
  h1 {
    color: black;
  }
}

@layer theme {
  h1 {
    color: green;
  }
}
```

→ 결과: **green**  
`theme` 레이어가 `base`보다 나중에 정의되었기 때문에, 같은 명시도일 경우 **theme 레이어가 우선 적용**됩니다.

---

## ⚖️ 기존 명시도 규칙과 비교

### 가정:

- HTML: `<div id="blue" class="blue">Text</div>`

- CSS:

```css
@layer one {
  #blue {
    color: blue;
  }
}

@layer two {
  .blue {
    color: green;
  }
}
```

- 기존 CSS 우선순위라면 `#blue`(ID 선택자)가 더 강해서 **blue**가 적용되어야 해요.
- 하지만 **Cascade Layer**를 사용하면 **`two` 레이어가 더 우선**이라 **green**이 적용됩니다.

✅ 즉, **레이어 순서가 명시도보다 우선**합니다!

---

## 🤝 협업에서의 장점

- **스타일 충돌 방지**에 효과적
- 팀플 시 base, component, override 등 **레이어를 나눠 관리**하면 체계적인 스타일링 가능
- 코드 가독성과 유지보수에 도움

---

## 💭 개인적인 생각

> 사실 이걸 정리하고 있는 지금도 “굳이 Cascade Layer를 꼭 써야 하나?” 하는 생각이 든다.  
> 아직 팀 프로젝트나 협업을 본격적으로 해본 건 아니라서…  
> 아직은 그 중요도를 잘 느끼지 못하는 것 같고,  
> 그래서 당장은 **후순위로 미뤄야겠다.**

---

📚 **결론:**  
Cascade Layer는 지금 당장 필요하지 않더라도,  
앞으로 협업하거나 규모 있는 프로젝트를 하게 된다면 큰 도움이 될 수 있는 기능입니다!
