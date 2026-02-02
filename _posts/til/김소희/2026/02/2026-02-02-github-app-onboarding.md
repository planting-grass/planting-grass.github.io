---
title: 2026-02-02
author: 김소희
date: 2026-02-02
category: TIL/김소희/2026/02
layout: post
---

# 오늘의 학습: GitHub App 설치 후 자동 온보딩 로직 구현

### 핵심 요약

GitHub App 설치 후 리다이렉트되는 경로(`/project/org/setup`)에서 `installation_id`를 추출하여 백엔드 DB에 연동하고, 유저의 서비스 이용 상태를 자동으로 최신화하는 프론트엔드 로직을 구현했습니다.

### 주요 포인트

- **콜백 경로 처리**: GitHub이 `?installation_id=XXX`를 포함한 URL로 리다이렉트하면, 콜백 컴포넌트가 이를 파싱하여 백엔드에 전달
- **API 서비스 레이어**: `org.service.ts`에 `setupOrganization()` 함수를 추가하여 백엔드 `/project/org/setup` 엔드포인트 호출
- **상태 갱신 연동**: DB 저장 후 `authService.fetchUserInfo()`를 호출하여 `appInstalled` 상태를 `true`로 업데이트
- **React StrictMode 대응**: `useRef`를 활용해 개발 환경에서의 중복 API 호출 방지
- **UI 레이아웃 분리**: `isAuthPage` 체크에 콜백 경로를 추가하여 처리 중에는 사이드바/헤더 없이 로딩 스피너만 표시

### 워크플로우

1. GitHub App 설치 완료 → GitHub이 콜백 URL로 리다이렉트
2. `OrgSetupCallback` 컴포넌트 마운트 → 로딩 스피너 표시
3. 백엔드 API 호출 → DB에 `installation_id` 저장
4. 유저 정보 재조회 → 프론트엔드 상태 업데이트
5. 대시보드로 이동 → 설치 유도 모달 사라지고 정상 서비스 이용

### 결론

OAuth 콜백 패턴과 유사하게, 외부 서비스(GitHub)에서 돌아올 때 **쿼리 파라미터 파싱 → 백엔드 동기화 → 상태 갱신 → 리다이렉트**의 흐름을 따릅니다. 이 패턴을 익히면 다른 외부 서비스 연동에도 동일하게 적용할 수 있습니다.
