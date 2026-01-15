---
title: 2026-01-15
author: 김소희
date: 2026-01-15
category: TIL/김소희/2026/01
layout: post
---

## 📝 Git: WSL에서 clone 직후 modified 폭탄 해결하기

> **Tags**: #Git #WSL #LineEnding #CRLF #LF

---

## 💡 핵심 요약

`git clone` 직후 아무것도 안 했는데 수십~수백 개 파일이 modified로 뜨는 현상의 원인과 해결법을 정리했습니다. 대부분 **Windows(CRLF)와 Linux(LF)의 줄바꿈 차이** 때문입니다.

---

## 🔑 주요 포인트

### 1. 줄바꿈 차이

| OS            | 줄바꿈 문자   |
| ------------- | ------------- |
| Windows       | CRLF (`\r\n`) |
| Linux / macOS | LF (`\n`)     |

Git은 줄바꿈도 파일 내용으로 보기 때문에, 체크아웃 시 자동 변환되면 전체가 수정된 것으로 인식됩니다.

### 2. 진단 방법

```bash
# 줄바꿈 무시 diff (출력 없으면 줄바꿈 문제)
git diff --ignore-space-at-eol --ignore-cr-at-eol -- README.md

# 실제 줄바꿈 확인
file README.md
```

### 3. 해결 방법 (WSL 기준)

```bash
# 저장소 단위 설정
git config core.autocrlf input
git config core.eol lf

# 원격 브랜치로 복구
git fetch origin
git reset --hard origin/develop
```

---

## 📌 재발 방지 팁

- WSL 홈 디렉토리(`~/`)에서 작업
- VSCode 우측 하단 줄바꿈을 `LF`로 유지
- 팀 프로젝트는 `.gitattributes` 도입 고려
