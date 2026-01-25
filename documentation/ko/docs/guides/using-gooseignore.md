---
title: goose의 파일 액세스 방지
sidebar_label: gooseignore 사용
sidebar_position: 80
---


`.gooseignore`는 goose가 액세스하지 않을 파일과 디렉토리의 패턴을 정의하는 텍스트 파일입니다. 이는 goose가 Developer 확장 기능의 도구를 사용할 때 이러한 파일을 읽거나, 수정하거나, 삭제하거나, 셸 명령을 실행할 수 없음을 의미합니다.

:::info Developer 확장 기능만 해당
.gooseignore 기능은 현재 [Developer](/docs/mcp/developer-mcp) 확장 기능의 도구에만 영향을 미칩니다. 다른 확장 기능은 이러한 규칙에 의해 제한되지 않습니다.
:::

이 가이드는 `.gooseignore` 파일을 사용하여 goose가 특정 파일과 디렉토리를 변경하지 못하게 하는 방법을 보여줍니다.

## `.gooseignore` 파일 만들기

goose는 두 가지 유형의 `.gooseignore` 파일을 지원합니다:
- **전역 무시 파일** - `~/.config/goose`에 `.gooseignore` 파일을 만듭니다. 이러한 제한은 디렉토리에 관계없이 goose와의 모든 세션에 적용됩니다.
- **로컬 무시 파일** - 적용하려는 디렉토리의 루트에 `.gooseignore` 파일을 만듭니다. 이러한 제한은 특정 디렉토리에서 작업할 때만 적용됩니다.

:::tip
전역 및 로컬 `.gooseignore` 파일을 동시에 사용할 수 있습니다. 둘 다 존재하면 goose는 두 파일의 제한을 결합하여 제한된 경로를 결정합니다.
:::

## `.gooseignore` 파일 예시

`.gooseignore` 파일에서 goose가 무시할 파일과 일치하는 패턴을 작성할 수 있습니다. 다음은 몇 가지 일반적인 패턴입니다:

```plaintext
# 이름으로 특정 파일 무시
settings.json         # "settings.json"이라는 파일만 무시

# 확장자로 파일 무시
*.pdf                # 모든 PDF 파일 무시
*.config             # .config로 끝나는 모든 파일 무시

# 디렉토리와 그 내용 무시
backup/              # "backup" 디렉토리의 모든 것 무시
downloads/           # "downloads" 디렉토리의 모든 것 무시

# 모든 디렉토리에서 이 이름을 가진 모든 파일 무시
**/credentials.json  # 모든 디렉토리에서 "credentials.json"이라는 모든 파일 무시

# 복잡한 패턴
*.log                # 모든 .log 파일 무시
!error.log           # error.log 파일은 예외
```

## 무시 파일 유형 및 우선순위
goose는 전역 `.gooseignore` 및 로컬 `.gooseignore` 파일의 무시 규칙을 준수합니다. 무시해야 할 파일을 결정하기 위해 우선순위 시스템을 사용합니다.

### 1. 전역 `.gooseignore`
- 가장 높은 우선순위로 항상 먼저 적용
- `~/.config/goose/.gooseignore`에 위치
- 컴퓨터의 모든 프로젝트에 영향

```
~/.config/goose/
└── .gooseignore      ← 모든 프로젝트에 적용
```

### 2. 로컬 `.gooseignore`
- 프로젝트별 규칙
- 프로젝트 루트 디렉토리에 위치

```
~/.config/goose/
└── .gooseignore      ← 전역 규칙이 먼저 적용

Project/
├── .gooseignore      ← 로컬 규칙이 두 번째로 적용
└── src/
```

### 3. 기본 패턴
기본적으로 .gooseignore 파일을 만들지 않은 경우 goose는 다음 패턴과 일치하는 파일을 수정하지 않습니다:
```plaintext
**/.env
**/.env.*
**/secrets.*
```

## 일반적인 사용 사례

다음은 `.gooseignore`가 유용한 몇 가지 일반적인 시나리오입니다:

- **생성된 파일**: goose가 자동 생성된 코드나 빌드 출력을 수정하지 못하게 방지
- **타사 코드**: goose가 외부 라이브러리나 종속성을 변경하지 못하게 유지
- **중요한 구성**: 중요한 구성 파일을 실수로 수정하지 못하게 보호
- **버전 관리**: `.git` 디렉토리와 같은 버전 관리 파일의 변경 방지
- **사용자 정의 제한**: goose가 액세스하지 않아야 할 파일을 정의하기 위해 `.gooseignore` 파일 생성
