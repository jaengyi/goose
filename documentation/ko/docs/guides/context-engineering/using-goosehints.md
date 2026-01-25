---
title: goose에게 힌트 제공
sidebar_position: 1
sidebar_label: goosehints 사용
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft } from 'lucide-react';

`.goosehints`는 프로젝트에 대한 추가 컨텍스트를 제공하고 goose와의 커뮤니케이션을 개선하는 데 사용되는 텍스트 파일입니다. `.goosehints`를 사용하면 goose가 요구 사항을 더 잘 이해하고 작업을 더 효과적으로 실행할 수 있습니다.

<details>
  <summary>goose 힌트 비디오 안내</summary>
  <iframe
  class="aspect-ratio"
  src="https://www.youtube.com/embed/kWXJC5p0608"
  title="goose Hints"
  frameBorder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  ></iframe>
</details>

`.goosehints` 파일을 추가하기 좋은 시기는 프롬프트를 반복하거나 같은 종류의 지침을 여러 번 제공하고 있을 때입니다. 파일에 더 적합한 많은 컨텍스트를 제공하는 좋은 방법이기도 합니다.

이 가이드에서는 사용자 정의 지침과 컨텍스트로 워크플로우를 간소화하기 위해 `.goosehints` 파일을 만들고 사용하는 방법을 안내합니다.

:::info Developer 확장 기능 필요
힌트 파일을 사용하려면 `Developer` 확장 기능이 [활성화](/docs/getting-started/using-extensions)되어 있어야 합니다.
:::

## 힌트 파일 만들기

goose는 두 가지 유형의 힌트 파일을 지원합니다:
- **전역 힌트 파일** - 이 힌트는 디렉토리에 관계없이 goose와의 모든 세션에 적용됩니다. 전역 힌트는 `~/.config/goose/.goosehints`에 저장됩니다.
- **로컬 힌트 파일** - 이 힌트는 특정 디렉토리 또는 디렉토리 계층에서 작업할 때만 적용됩니다.

전역 및 로컬 힌트를 동시에 사용할 수 있습니다. 둘 다 존재하면 goose는 전역 선호도와 프로젝트별 요구 사항을 모두 고려합니다. 로컬 힌트 파일의 지침이 전역 선호도와 충돌하면 goose는 로컬 힌트를 우선시합니다.

:::tip 사용자 정의 컨텍스트 파일
[`CONTEXT_FILE_NAMES` 환경 변수](#사용자-정의-컨텍스트-파일)를 사용하여 goose와 함께 다른 에이전트 규칙 파일을 사용할 수 있습니다.
:::

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>

    #### 전역 힌트 파일
    1. `~/.config/goose`에 `.goosehints` 파일을 만듭니다

    #### 로컬 힌트 파일

    1. 앱 하단의 디렉토리 경로를 클릭하고 파일을 만들려는 디렉토리를 엽니다
    2. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    3. 사이드바에서 `Settings`를 클릭합니다
    4. `Chat`을 클릭합니다
    5. `Project Hints (.goosehints)` 섹션으로 스크롤하고 `Configure`를 클릭합니다
    6. 텍스트 영역에 로컬 힌트를 입력합니다
    7. `Save`를 클릭합니다
    8. goose가 업데이트된 `.goosehints`를 읽을 수 있도록 세션을 다시 시작합니다

    주어진 디렉토리에 `.goosehints` 파일이 이미 있으면 기존 힌트를 편집할 수 있습니다.

    </TabItem>
    <TabItem value="manual" label="수동">

    - **전역 힌트 파일** - `~/.config/goose`에 `.goosehints` 파일을 만듭니다.
    - **로컬 힌트 파일** - 프로젝트 루트 및/또는 계층의 모든 디렉토리에 `.goosehints` 파일을 만듭니다.

    </TabItem>
</Tabs>

`.goosehints` 파일에는 프로젝트와 관련된 모든 지침이나 상황별 세부 정보가 포함될 수 있습니다.

## 힌트 설정

`.goosehints` 파일은 자연어를 지원합니다. goose가 쉽게 이해하고 따를 수 있는 직접적인 언어를 사용하여 명확하고 구체적인 지침을 작성하세요. 프로젝트와 워크플로우 선호도에 대한 관련 컨텍스트를 포함하고 가장 중요한 지침을 먼저 우선시하세요.

goosehints는 세션 시작 시 로드되고 모든 요청과 함께 전송되는 시스템 프롬프트의 일부가 됩니다. 이는 `.goosehints`의 내용이 토큰 사용량에 기여함을 의미하므로 간결하게 유지하면 비용과 처리 시간을 모두 절약할 수 있습니다.

### 전역 `.goosehints` 파일 예시

```
Always use TypeScript for new Next.js projects.

@coding-standards.md  # Contains our coding standards
docs/contributing.md  # Contains our pull request process

Follow the [Google Style Guide](https://google.github.io/styleguide/pyguide.html) for Python code.

Run unit tests before committing any changes.

Prefer functional programming patterns where applicable.
```

### 로컬 `.goosehints` 파일 예시

```
This is a simple example JavaScript web application that uses the Express.js framework. View [Express documentation](https://expressjs.com/) for extended guidance.

Go through the @README.md for information on how to build and test it as needed.

Make sure to confirm all changes with me before applying.

Run tests with `npm run test` ideally after each change.
```

이 예시는 다른 파일을 참조하는 두 가지 방법을 보여줍니다:
- **`@` 구문**: 파일 내용을 goose의 즉시 컨텍스트에 자동으로 포함
- **일반 참조**: 필요할 때 검토할 파일을 goose에게 지시 (선택적이거나 매우 큰 파일에 사용)

### 중첩된 `.goosehints` 파일

goose는 git 저장소에서 계층적 로컬 힌트를 지원합니다. 현재 디렉토리에서 루트 디렉토리까지의 모든 `.goosehints` 파일이 자동으로 로드되고 결합됩니다. git 저장소에서 작업하지 않는 경우 goose는 현재 디렉토리의 `.goosehints` 파일만 로드합니다.

모범 사례로 각 수준의 `.goosehints`는 해당 범위와 관련된 힌트만 포함해야 합니다:
- **루트 수준**: 프로젝트 전체 표준, 빌드 프로세스 및 일반 지침 포함
- **모듈/기능 수준**: 해당 코드베이스 영역에 대한 특정 요구 사항 추가
- **디렉토리 수준**: 로컬 테스트 절차 또는 구성요소 패턴과 같은 매우 구체적인 컨텍스트 포함

**프로젝트 구조 예시:**
```sh
my-project/
├── .git/
├── .goosehints              # 프로젝트 전체 힌트
├── frontend/
│   ├── .goosehints          # 프론트엔드 특정 힌트
│   └── components/
│       ├── .goosehints      # 구성요소 특정 힌트
│       └── Button.tsx
└── backend/
    ├── .goosehints          # 백엔드 특정 힌트
    └── api/
        └── routes.py
```

이 예시 프로젝트에서 `frontend/components/`에서 작업할 때 goose는 다음 순서로 계층 상위 디렉토리의 힌트를 로드합니다:
1. `my-project/.goosehints` (프로젝트 루트)
2. `frontend/.goosehints`
3. `frontend/components/.goosehints` (현재 디렉토리)

## 일반적인 사용 사례
다음은 goose에게 추가 컨텍스트를 제공하기 위해 힌트를 사용한 몇 가지 방법입니다:

- **의사 결정**: goose가 자율적으로 변경해야 하는지 아니면 먼저 사용자에게 작업을 확인해야 하는지 지정합니다.

- **검증 루틴**: 변경 사항이 프로젝트 사양을 충족하는지 확인하기 위해 goose가 수행해야 하는 테스트 케이스 또는 검증 방법을 제공합니다.

- **피드백 루프**: goose가 피드백을 받고 제안을 반복적으로 개선할 수 있는 단계를 포함합니다.

- **더 자세한 문서 참조**: goose가 자세한 설명을 위해 참조해야 하는 `README.md`, `docs/setup-guide.md` 또는 기타 중요한 파일을 표시합니다.

- **@-멘션으로 정리**: 자주 필요한 문서의 경우 `@filename.md` 또는 `@relative/path/testing.md`를 사용하여 참조만 하는 대신 파일 내용을 현재 컨텍스트에 자동으로 포함합니다. 이렇게 하면 goose가 중요한 정보에 즉시 액세스할 수 있습니다.
핵심 문서(API 스키마 또는 코딩 표준과 같은)는 즉시 컨텍스트를 위해 @-멘션으로 포함하지만 선택적이거나 매우 큰 파일에는 일반 참조(`@` 없이)를 사용합니다.

프롬프트처럼 이것은 `.goosehints` 파일을 형성하기 위한 광범위한 목록이 아닙니다. 필요한 만큼 많은 컨텍스트를 포함할 수 있습니다.

## 모범 사례

- **파일을 최신 상태로 유지**: 프로젝트 프로토콜이나 우선순위의 변경 사항을 반영하도록 `.goosehints` 파일을 정기적으로 업데이트합니다.
- **간결하게**: goose가 정보를 빠르게 파싱하고 조치를 취할 수 있도록 내용이 간단하고 요점에 맞는지 확인합니다.
- **작게 시작**: 명확하고 구체적인 소수의 힌트를 만들고 필요에 따라 점진적으로 확장합니다. 이렇게 하면 goose가 지침을 어떻게 해석하고 적용하는지 이해하기 쉬워집니다.
- **다른 파일 참조**: 반복을 줄이고 지침을 가볍게 유지하기 위해 /docs/style.md 또는 /scripts/validation.js와 같은 관련 파일을 goose에게 지시합니다.

## 사용자 정의 컨텍스트 파일

goose는 기본적으로 `AGENTS.md` 다음 `.goosehints` 파일을 찾지만 `CONTEXT_FILE_NAMES` 환경 변수를 사용하여 다른 파일 이름이나 여러 컨텍스트 파일을 구성할 수 있습니다. 다음에 유용합니다:

- **도구 호환성**: 다른 AI 도구의 규칙 사용 (예: `CLAUDE.md`)
- **조직**: 자주 사용하는 규칙을 자동으로 로드되는 여러 파일로 분리
- **프로젝트 규칙**: 프로젝트의 기존 도구 체인에서 컨텍스트 파일 사용 (`.cursorrules`)

작동 방식:
1. goose는 전역(~/.config/goose/) 및 로컬(현재 디렉토리) 위치 모두에서 각 구성된 파일 이름을 찾습니다
2. 찾은 모든 파일이 로드되어 컨텍스트에 결합됩니다

### 구성

`CONTEXT_FILE_NAMES` 환경 변수를 파일 이름의 JSON 배열로 설정합니다. 기본값은 `["AGENTS.md", ".goosehints"]`입니다.

```bash
# 단일 사용자 정의 파일
export CONTEXT_FILE_NAMES='["AGENTS.md"]'

# 프로젝트 도구 체인 파일
export CONTEXT_FILE_NAMES='[".cursorrules", "AGENTS.md"]'

# 여러 파일
export CONTEXT_FILE_NAMES='["CLAUDE.md", ".goosehints", "project_rules.txt"]'
```
