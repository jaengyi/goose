---
sidebar_position: 15
title: 프로젝트 관리
sidebar_label: 프로젝트 관리
---

goose 프로젝트는 작업 디렉토리와 관련 세션을 자동으로 추적하여 전체 컨텍스트 보존과 함께 여러 코드베이스에서 작업을 쉽게 재개할 수 있게 합니다.

goose에서 **프로젝트**는 goose를 사용한 작업 디렉토리의 기록입니다. goose를 실행할 때마다 현재 디렉토리를 프로젝트로 자동 추적하며 다음을 저장합니다:

- **경로**: 프로젝트 디렉토리의 절대 경로
- **마지막 액세스**: 이 프로젝트에서 마지막으로 작업한 시간
- **마지막 지시**: goose에게 제공한 가장 최근 명령
- **세션 ID**: 컨텍스트 연속성을 위한 관련 세션

프로젝트는 `~/.local/share/goose/projects.json`에 저장됩니다.

:::info CLI 전용 기능
프로젝트는 현재 goose CLI를 통해서만 사용할 수 있습니다. 데스크톱 지원은 향후 릴리스에 계획되어 있습니다.
:::

## 기본 사용법

**가장 최근 프로젝트 재개:**

```bash
goose project
```

**모든 프로젝트 찾아보기:**

```bash
goose projects
```
:::tip
프로젝트를 재개할 때 이전 세션을 계속하거나 해당 디렉토리에서 새로 시작할 수 있습니다.
:::

전체 명령 구문과 옵션은 [CLI 명령 가이드](/docs/guides/goose-cli-commands#project)를 참조하세요.

## 워크플로우 예시

하루 종일 여러 프로젝트를 작업하는 개발자 Sarah를 따라가 봅시다:

### 아침: API 개발
```bash
cd ~/projects/ecommerce-api
goose session --name "api-auth-work"
```
*Sarah는 goose에게 JWT 토큰 갱신 로직 구현을 도와달라고 요청합니다*

### 오전 중반: 모바일 앱 버그 수정
```bash
cd ~/projects/mobile-app
goose session
```
*Sarah는 로그인 화면에서 iOS 충돌을 디버깅하는 데 도움을 받습니다*

### 오후: 관리자 대시보드
```bash
cd ~/projects/admin-dashboard
goose session --name "dashboard-ui"
```
*Sarah는 사용자 관리 인터페이스 컴포넌트를 만드는 작업을 합니다*

### 다음 날: 빠른 재개
```bash
# 어느 디렉토리에서든 가장 최근 프로젝트를 빠르게 재개
goose project
```

goose가 표시합니다:
```
┌ goose Project Manager
│
◆ Choose an option:
│  ○ Resume project with session: .../admin-dashboard
│    Continue with the previous session
│  ○ Resume project with fresh session: .../admin-dashboard
│    Change to the project directory but start a new session
│  ○ Start new project in current directory: /Users/sarah
│    Stay in the current directory and start a new session
└
```

### 나중에: 모든 프로젝트 찾아보기
```bash
goose projects
```

goose가 표시합니다:
```
┌ goose Project Manager
│
◆ Select a project:
│  ○ 1  .../admin-dashboard (2025-01-07 09:15:30) [create user management interface]
│  ○ 2  .../mobile-app (2025-01-06 11:45:20) [login screen crashing on iOS]
│  ○ 3  .../ecommerce-api (2025-01-06 09:30:15) [JWT token refresh logic]
│  ○ Cancel
└
```

Sarah는 타임스탬프와 컨텍스트와 함께 최근 프로젝트를 볼 수 있어 작업을 계속할 위치를 쉽게 선택할 수 있습니다.

## 이점

:::tip 시간 절약
프로젝트는 코드베이스 간 전환 시 일반적으로 손실되는 2-5분을 없애주며, 특히 매일 여러 프로젝트를 작업하는 개발자에게 유용합니다.
:::

- **컨텍스트 전환 마찰 제거** - 수동 탐색 없이 프로젝트 간 즉시 이동
- **작업 컨텍스트 보존** - 전체 대화 기록과 함께 중단한 곳에서 정확히 재개
- **원활한 세션 통합** - 다양한 코드베이스에서 연속성 유지
