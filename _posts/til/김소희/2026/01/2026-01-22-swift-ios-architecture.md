---
title: 2026-01-22
author: 김소희
date: 2026-01-22
category: TIL/김소희/2026/01
layout: post
---

## 핵심 요약

iOS 앱 개발에서 Swift는 웹 개발처럼 프론트엔드/백엔드가 명확히 분리되지 않는다. Swift 앱 자체가 클라이언트 역할을 하고, 백엔드가 필요하면 Firebase 같은 BaaS나 별도 서버를 사용한다.

## 주요 포인트

### 웹 vs iOS 구조 차이

- **웹**: Frontend(React 등) + Backend(Node 등) 별도 프로젝트
- **iOS**: 하나의 앱 프로젝트 안에 UI + 로직 포함

### iOS 앱 내부 레이어

- **Presentation Layer**: SwiftUI/UIKit (화면 담당)
- **Business Logic Layer**: ViewModel, Services
- **Data Layer**: 네트워크, 로컬 DB 처리

### 백엔드 선택지

- **BaaS**: Firebase, Supabase (간단한 앱에 적합)
- **자체 서버**: Vapor(Swift), Node.js 등
- **서버리스**: AWS Lambda, Cloud Functions

## 결론

간단한 앱은 Firebase로 충분하고, 복잡한 서비스는 별도 백엔드 구축을 고려해야 한다.
