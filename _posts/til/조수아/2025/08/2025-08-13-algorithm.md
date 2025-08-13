---
title: 2025-08-12
author: 조수아
date: 2025-08-12
category: TIL/조수아/2025/08
layout: post
---

# 오늘 배운 것

## 크롬 익스텐션 배포

### 자동 배포 적용 방법

- 크롬 익스텐션 자동 배포 과정은 먼저 확장을 Chrome Web Store에 수동 등록해 확장 ID를 확보하고, Google Cloud Console에서 Chrome Web Store API를 활성화한 뒤 웹 애플리케이션 유형의 OAuth 클라이언트를 생성해 Client ID와 Client Secret을 발급받는다
- OAuth Playground를 이용해 https://www.googleapis.com/auth/chromewebstore 스코프로 인증하고 Refresh Token을 얻어, 이 네 가지 값(CWS_EXTENSION_ID, CWS_CLIENT_ID, CWS_CLIENT_SECRET, CWS_REFRESH_TOKEN)을 GitHub Secrets에 저장한다.
- 이후 .github/workflows 폴더에 mnao305/chrome-extension-upload@v5.0.0 액션을 사용하는 워크플로 파일을 작성해, deploy 브랜치에 푸시될 때 manifest.json이 루트에 포함된 ZIP을 생성하고 자동 업로드·게시되도록 설정한다.

### 적용 이유

- 매번 식단표 수정할 때마다 배포를 진행해야했는데 해당과정이 너무 복잡해서 자동 배포를 진행하게되었다.
- 근데 이 배포도 한번 하면 검증되는데 하루정도 걸려서 깃허브 레포지토리를 새로 파서 해당 레포를 페이지로 열고 json파일을 읽어오는 방식으로 해서 크롬익스텐션 배포 없이도 식단표 업그레이드가 신속하게 되도록 수정했다.

# 내일 할 것
