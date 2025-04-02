---
title: "CascadeLayer 사용"
excerpt: "협업 시의 스타일 그룹의 우선 순위 지정"

categories:
  - CSS
tags:
  - [CascadeLayer, 협업, 스타일 우선 순위]

permalink: /categories2/post-name-here-2/

toc: true
toc_sticky: true

date: 2025-03-27
last_modified_at: 2025-03-27
---

## 🦥 본문

[📄 PDF 파일 다운로드](../_posts/asset/files/Cascade-Layers.html)

우선 Cascade-Layer란 기존에는 선언 순서, 명시도, 중요도 등으로만 우선 순위를 조절했지만 Cascade Layer의 경우 개발자가 명시적으로 스타일 그룹의 우선 순위를 지정할 수 있게 해주는 기술.

예를 들어 @layer를 사용하여 정렬 순서를 지정해 주는데 layer base, theme의 경우

base에 h1 black을 주고 theme에 h1 green을 주게 되면 theme에 있는 스타일로 적용이 된다.

여기서 중요한 점은 같은 명시도, 같은 중요도일 경우 나중에 정의된 레이어가 우선으로 적용이 된다.

** 만약 div #blue .blue가 있다는 가정이다. 1번 레이어에 #blue color blue를 주고 2번 레이어에 .blue color green을 주게 된다면 어떻게 될까? 기존 css 우선 순위에 따르면 blue로 적용이 될 것이다. 하지만 케스케이드 레이어를 사용하게 되면 레이어 순서가 높은 쪽으로 규칙이 우선 적용 되기 때문에 green으로 적용이 되게 된다. **
** 이러한 우선 적용 규칙 덕분에 팀플이나 협업을 할 때 스타일 충돌을 방지 할 수 있다. **

근데 사실 이걸 적고있는 지금도 굳이 캐스케이드 레이어를 사용을 해야 하나? 하는 생각이 든다.
아직 팀플이나 협업을 안 해봐서 그런가... 아직 중요도를 못 느끼겠으니 후 순위로 미뤄야겠다.
