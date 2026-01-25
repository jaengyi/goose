---
title: 격리된 개발 환경
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

이 가이드에서는 **[Container Use MCP](https://github.com/dagger/container-use)**와 goose를 사용하여 격리된 개발 환경을 설정하는 방법을 보여줍니다. 이 설정을 사용하면 개발 작업이 git 브랜치와 컨테이너 모두에 격리되어 메인 시스템 상태에 영향을 주지 않고 자유롭게 실험할 수 있습니다.
Container-use MCP는 매우 새롭고 부상하고 있지만 Docker, copy on write 파일 시스템 등의 도구를 기반으로 에이전트 친화적인 격리된 개발을 수행하는 강력한 방법을 제공합니다.

## 개요

**[Container Use MCP](https://github.com/dagger/container-use)** 서버는 goose와 원활하게 통합되는 컨테이너화된 개발 환경을 제공합니다. 이를 통해 다음을 수행할 수 있습니다:

- git 브랜치에 격리된 변경 사항 작업
- 로컬 머신에 영향을 주지 않고 컨테이너에서 코드 실행
- 필요할 때 쉽게 리셋하고 새로 시작
- 다른 프로젝트와 실험 간에 깔끔한 분리 유지
- 병렬로 작업 수행

## 사전 요구 사항

- Docker ([Podman](https://docs.dagger.io/ci/integrations/podman), [NerdCtl](https://docs.dagger.io/ci/integrations/nerdctl/) 또는 [Container](https://docs.dagger.io/ci/integrations/apple-container/))가 시스템에 설치되어 실행 중
- Git이 설치되고 구성됨
- goose가 설치되고 구성됨

## 설치

### Container Use 설치

이 빠르게 발전하는 프로젝트의 최신 설치 지침은 [Container Use README](https://github.com/dagger/container-use/blob/main/README.md)를 참조하세요.

## goose에 추가

### 방법 1: 빠른 설정 링크

이 링크를 클릭하여 goose에 확장 프로그램을 자동으로 추가:

**[goose에 Container-Use 추가](goose://extension?cmd=cu&arg=stdio&id=container-use&name=container%20use&description=use%20containers%20with%20dagger%20and%20git%20for%20isolated%20environments)**

### 방법 2: 수동 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>

  1. goose Desktop 오른쪽 상단의 `...`를 클릭합니다.
  2. 메뉴에서 `Advanced Settings`를 선택합니다.
  3. `Extensions` 아래에서 `Add custom extension`을 클릭합니다.
  4. 세부 정보 입력:
     - **Type**: `Standard IO`
     - **ID**: `container-use`
     - **Name**: `Container Use`
     - **Description**: `Use containers with dagger and git for isolated environments`
     - **Command**: `cu`
     - **Arguments**: `stdio`
  5. `Add` 버튼 클릭

  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. 구성 명령 실행:
  ```bash
  goose configure
  ```

  2. 메뉴에서 `Add Extension` 선택.

  3. `Command-line Extension` 선택.

  4. 프롬프트 따르기:
     - **Extension name**: `Container Use`
     - **Command**: `cu stdio`
     - **Timeout**: `300` (또는 원하는 타임아웃)
     - **Environment variables**: 필요 없음

  </TabItem>
  <TabItem value="config" label="Config File">

`~/.config/goose/config.yaml` 파일에 다음 구성 추가:

```yaml
extensions:
  container-use:
    name: container-use
    type: stdio
    enabled: true
    cmd: cu
    args:
    - stdio
    envs: {}
    timeout: 300
```

  </TabItem>
</Tabs>

## 사용법

goose에서 확장 프로그램이 활성화되면 다음을 수행할 수 있습니다:

### 격리된 개발 시작

goose와의 대화에서 격리된 환경에서 작업하고 싶다고 말하면 됩니다:

```
"새로운 기능을 추가하는 실험을 하고 싶은데, 메인 코드베이스에 영향을 주지 않도록 격리된 환경에서 하고 싶어요."
```

goose가 자동으로:
1. 작업을 위한 새 git 브랜치 생성
2. 컨테이너화된 환경 설정
3. 모든 변경 사항이 호스트 시스템과 격리되도록 보장

### 실험 작업

```
"이 알고리즘에 완전히 다른 접근 방식을 시도해 보고 싶어요. 실험할 수 있는 격리된 환경을 설정해 줄 수 있나요?"
```

### 새로운 기술 학습

```
"이 새로운 프레임워크를 사용해 보고 싶지만 메인 시스템에 모든 종속성을 설치하고 싶지 않아요."
```

## 이점

- **안전성**: 메인 개발 환경을 손상시킬 걱정 없이 실험
- **재현성**: 다른 머신과 팀 구성원 간에 일관된 환경
- **격리**: 충돌 없이 여러 프로젝트를 동시에 실행 가능
- **쉬운 정리**: 완료되면 컨테이너와 브랜치 제거
- **버전 관리**: 모든 변경 사항이 격리된 git 브랜치에서 추적됨
- **롤백 기능**: 실패한 실험을 쉽게 폐기

## 일반적인 워크플로우

### 기능 개발

1. goose와 새로운 기능에 대해 대화 시작
2. 격리된 개발 환경 요청
3. goose가 브랜치와 컨테이너 생성
4. 기능 개발 및 테스트
5. 성공하면 브랜치 병합, 아니면 폐기

### 종속성 탐색

1. goose에게 새 라이브러리나 도구 탐색 요청
2. 종속성이 있는 격리된 컨테이너에서 작업
3. 호환성 및 기능 테스트
4. 메인 프로젝트에 통합 여부 결정

### 리팩토링

1. 주요 리팩토링을 위한 격리된 환경 요청
2. 컨테이너와 브랜치의 안전한 환경에서 변경
3. 병합 전 철저히 테스트
4. 문제 발생 시 쉬운 롤백

## 문제 해결

### 일반적인 문제

**Docker가 실행되지 않음:**
- Docker Desktop이 설치되어 실행 중인지 확인
- Docker 데몬 상태 확인: `docker info`

**권한 문제:**
- 사용자가 Docker 명령을 실행할 권한이 있는지 확인
- Linux에서는 docker 그룹에 사용자 추가: `sudo usermod -aG docker $USER`

**Git 문제:**
- Git이 사용자 이름과 이메일로 올바르게 구성되었는지 확인
- 격리된 작업을 시작할 때 Git 저장소에 있는지 확인

### 도움 받기

문제가 발생하면:

1. 문서는 **[Container Use GitHub 저장소](https://github.com/dagger/container-use)** 확인
2. 모든 사전 요구 사항이 설치되어 작동하는지 확인
3. 지원을 위해 [Discord 커뮤니티](https://discord.gg/goose-oss)에 참여

## 다음 단계

goose에서 container-use가 활성화되면 자신있게 개발할 준비가 된 것입니다. 실험하기 주저했던 프로젝트에 대해 대화를 시작하고 goose가 탐색을 위한 안전하고 격리된 환경을 설정하도록 하세요.

기억하세요: 격리된 환경에서는 실패한 실험이란 없습니다 - 메인 코드베이스에 영향을 주지 않는 학습 기회만 있을 뿐입니다.
