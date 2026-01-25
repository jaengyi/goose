---
sidebar_position: 85
title: 구성 파일
sidebar_label: 구성 파일
---

# 구성 개요

goose는 설정과 확장 기능을 관리하기 위해 YAML [구성 파일](#구성-파일들)을 사용합니다. 기본 구성 파일 위치:

* macOS/Linux: `~/.config/goose/config.yaml`
* Windows: `%APPDATA%\Block\goose\config\config.yaml`

구성 파일을 통해 기본 동작을 설정하고, 언어 모델을 구성하고, 도구 권한을 설정하고, 확장 기능을 관리할 수 있습니다. 많은 설정은 [환경 변수](/docs/guides/environment-variables)를 사용하여 설정할 수도 있지만, 구성 파일은 선호도를 유지하는 영구적인 방법을 제공합니다.

## 구성 파일들

- **config.yaml** - 공급자, 모델, 확장 기능 및 일반 설정
- **permission.yaml** - `goose configure`를 통해 구성된 도구 권한 수준
- **secrets.yaml** - API 키 및 시크릿(키링이 비활성화된 경우에만)
- **permissions/tool_permissions.json** - 런타임 권한 결정(자동 관리)

## 전역 설정

다음 설정은 config.yaml 파일의 루트 레벨에서 구성할 수 있습니다:

| 설정 | 목적 | 값 | 기본값 | 필수 |
|---------|---------|---------|---------|-----------|
| `GOOSE_PROVIDER` | 기본 [LLM 공급자](/docs/getting-started/providers) | "anthropic", "openai" 등 | 없음 | 예 |
| `GOOSE_MODEL` | 사용할 기본 모델 | 모델 이름(예: "claude-3.5-sonnet", "gpt-4") | 없음 | 예 |
| `GOOSE_TEMPERATURE` | 모델 응답 무작위성 | 0.0에서 1.0 사이의 실수 | 모델별 기본값 | 아니오 |
| `GOOSE_MAX_TOKENS` | 각 모델 응답의 최대 토큰 수(긴 응답 잘림) | 양의 정수 | 모델별 기본값 | 아니오 |
| `GOOSE_MODE` | [도구 실행 동작](/docs/guides/goose-permissions) | "auto", "approve", "chat", "smart_approve" | "auto" | 아니오 |
| `GOOSE_MAX_TURNS` | 사용자 입력 없이 허용되는 [최대 턴 수](/docs/guides/sessions/smart-context-management#maximum-turns) | 정수(예: 10, 50, 100) | 1000 | 아니오 |
| `GOOSE_LEAD_PROVIDER` | [리드/워커 모드](/docs/guides/environment-variables#leadworker-model-configuration)의 리드 모델 공급자 | `GOOSE_PROVIDER` 옵션과 동일 | `GOOSE_PROVIDER`로 폴백 | 아니오 |
| `GOOSE_LEAD_MODEL` | 리드/워커 모드의 리드 모델 | 모델 이름 | 없음 | 아니오 |
| `GOOSE_PLANNER_PROVIDER` | [계획 모드](/docs/guides/creating-plans)의 공급자 | `GOOSE_PROVIDER` 옵션과 동일 | `GOOSE_PROVIDER`로 폴백 | 아니오 |
| `GOOSE_PLANNER_MODEL` | 계획 모드의 모델 | 모델 이름 | `GOOSE_MODEL`로 폴백 | 아니오 |
| `GOOSE_TOOLSHIM` | 도구 해석 활성화 | true/false | false | 아니오 |
| `GOOSE_TOOLSHIM_OLLAMA_MODEL` | 도구 해석용 모델 | 모델 이름(예: "llama3.2") | 시스템 기본값 | 아니오 |
| `GOOSE_CLI_MIN_PRIORITY` | 도구 출력 상세 수준 | 0.0에서 1.0 사이의 실수 | 0.0 | 아니오 |
| `GOOSE_CLI_THEME` | CLI 응답 마크다운 [테마](/docs/guides/goose-cli-commands#themes) | "light", "dark", "ansi" | "dark" | 아니오 |
| `GOOSE_CLI_SHOW_COST` | CLI에서 토큰 사용에 대한 예상 비용 표시 | true/false | false | 아니오 |
| `GOOSE_ALLOWLIST` | 허용된 확장 기능 URL | 유효한 URL | 없음 | 아니오 |
| `GOOSE_RECIPE_GITHUB_REPO` | 레시피용 GitHub 저장소 | 형식: "org/repo" | 없음 | 아니오 |
| `GOOSE_AUTO_COMPACT_THRESHOLD` | goose가 [세션을 자동으로 요약](/docs/guides/sessions/smart-context-management#automatic-compaction)하는 백분율 임계값 설정 | 0.0에서 1.0 사이의 실수(0.0에서 비활성화) | 0.8 | 아니오 |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | [관측성](/docs/guides/environment-variables#opentelemetry-protocol-otlp)을 위한 OTLP 엔드포인트 URL | URL(예: `http://localhost:4318`) | 없음 | 아니오 |
| `OTEL_EXPORTER_OTLP_TIMEOUT` | [관측성](/docs/guides/environment-variables#opentelemetry-protocol-otlp)을 위한 내보내기 타임아웃(밀리초) | 정수(ms) | 10000 | 아니오 |
| `SECURITY_PROMPT_ENABLED` | 잠재적으로 유해한 명령을 식별하기 위한 [프롬프트 주입 감지](/docs/guides/security/prompt-injection-detection) 활성화 | true/false | false | 아니오 |
| `SECURITY_PROMPT_THRESHOLD` | 프롬프트 주입 감지 민감도 임계값(높을수록 엄격함) | 0.01에서 1.0 사이의 실수 | 0.8 | 아니오 |
| `SECURITY_PROMPT_CLASSIFIER_ENABLED` | 고급 위협 식별을 위한 ML 기반 프롬프트 주입 감지 활성화 | true/false | false | 아니오 |
| `SECURITY_PROMPT_CLASSIFIER_ENDPOINT` | ML 기반 프롬프트 주입 감지를 위한 분류 엔드포인트 URL | URL(예: "https://api.example.com/classify") | 없음 | 아니오 |
| `SECURITY_PROMPT_CLASSIFIER_TOKEN` | `SECURITY_PROMPT_CLASSIFIER_ENDPOINT`용 인증 토큰 | 문자열 | 없음 | 아니오 |

## 실험적 기능

이러한 설정은 활발히 개발 중인 실험적 기능을 활성화합니다. 향후 릴리스에서 변경되거나 제거될 수 있습니다.

| 설정 | 목적 | 값 | 기본값 | 필수 |
|---------|---------|---------|---------|-----------|
| `ALPHA_FEATURES` | 실험적 알파 기능에 대한 액세스 활성화&mdash;이 플래그가 필요한지 기능 문서를 확인하세요 | true/false | false | 아니오 |

추가 [환경 변수](/docs/guides/environment-variables)도 config.yaml에서 지원될 수 있습니다.

## 구성 예시

다음은 config.yaml 파일의 기본 예시입니다:

```yaml
# 모델 구성
GOOSE_PROVIDER: "anthropic"
GOOSE_MODEL: "claude-4.5-sonnet"
GOOSE_TEMPERATURE: 0.7

# 계획 구성
GOOSE_PLANNER_PROVIDER: "openai"
GOOSE_PLANNER_MODEL: "gpt-4"

# 도구 구성
GOOSE_MODE: "smart_approve"
GOOSE_TOOLSHIM: true
GOOSE_CLI_MIN_PRIORITY: 0.2

# 레시피 구성
GOOSE_RECIPE_GITHUB_REPO: "block/goose-recipes"

# 검색 경로 구성
GOOSE_SEARCH_PATHS:
  - "/usr/local/bin"
  - "~/custom/tools"
  - "/opt/homebrew/bin"

# 관측성(OpenTelemetry)
OTEL_EXPORTER_OTLP_ENDPOINT: "http://localhost:4318"
OTEL_EXPORTER_OTLP_TIMEOUT: 20000

# 보안 구성
SECURITY_PROMPT_ENABLED: true

# 확장 기능 구성
extensions:
  developer:
    bundled: true
    enabled: true
    name: developer
    timeout: 300
    type: builtin

  memory:
    bundled: true
    enabled: true
    name: memory
    timeout: 300
    type: builtin
```

## 확장 기능 구성

확장 기능은 `extensions` 키 아래에서 구성됩니다. 각 확장 기능에는 다음 설정이 있을 수 있습니다:

```yaml
extensions:
  extension_name:
    bundled: true/false       # goose에 포함되어 있는지 여부
    display_name: "Name"      # 사람이 읽을 수 있는 이름(선택 사항)
    enabled: true/false       # 확장 기능이 활성화되어 있는지 여부
    name: "extension_name"    # 내부 이름
    timeout: 300              # 작업 타임아웃(초)
    type: "builtin"/"stdio"   # 확장 기능 유형

    # stdio 확장 기능에 대한 추가 설정:
    cmd: "command"            # 실행할 명령
    args: ["arg1", "arg2"]    # 명령 인수
    description: "text"       # 확장 기능 설명
    env_keys: []              # 필수 환경 변수
    envs: {}                  # 환경 값
```

## 검색 경로 구성

확장 기능은 외부 명령이나 도구를 실행해야 할 수 있습니다. 기본적으로 goose는 시스템의 PATH 환경 변수를 사용합니다. 구성 파일에서 추가 검색 디렉토리를 추가할 수 있습니다:

```yaml
GOOSE_SEARCH_PATHS:
  - "/usr/local/bin"
  - "~/custom/tools"
  - "/opt/homebrew/bin"
```

이러한 경로는 확장 기능 명령을 실행할 때 시스템 PATH 앞에 추가되어 전역 PATH를 수정하지 않고도 사용자 정의 도구를 찾을 수 있습니다.

## 레시피 명령 구성
선택적으로 만든 레시피를 실행하기 위한 [사용자 정의 슬래시 명령어](/docs/guides/context-engineering/slash-commands)를 설정할 수 있습니다. 레시피 경로와 함께 명령(앞의 `/` 없이)을 나열하세요:

```yaml
slash_commands:
  - command: "run-tests"
    recipe_path: "/path/to/recipe.yaml"
  - command: "daily-standup"
    recipe_path: "/Users/me/.local/share/goose/recipes/standup.yaml"
```

## 구성 우선순위

설정은 다음 우선순위 순서로 적용됩니다:

1. 환경 변수(최고 우선순위)
2. 구성 파일 설정
3. 기본값(최저 우선순위)

## 보안 고려사항

- 구성 파일에 민감한 정보(API 키, 토큰)를 저장하지 마세요
- 시크릿 저장에는 시스템 키링을 사용하세요
- 키링이 비활성화된 경우 시크릿은 별도의 `secrets.yaml` 파일에 저장됩니다

## 구성 업데이트

구성 파일에 대한 변경 사항은 goose를 다시 시작해야 적용됩니다. 다음을 사용하여 현재 구성을 확인할 수 있습니다:

```bash
goose info -v
```

이렇게 하면 모든 활성 설정과 현재 값이 표시됩니다.

## 참조

- **[다중 모델 구성](/docs/guides/multi-model/)** - 여러 모델 선택 전략용
- **[환경 변수](./environment-variables.md)** - 환경 변수 구성용
- **[확장 기능 사용](/docs/getting-started/using-extensions.md)** - 확장 기능 구성에 대한 자세한 내용
