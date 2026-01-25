---
sidebar_position: 95
title: 환경 변수
sidebar_label: 환경 변수
---

goose는 동작을 사용자 정의할 수 있는 다양한 환경 변수를 지원합니다. 이 가이드는 기능별로 그룹화된 사용 가능한 환경 변수의 포괄적인 목록을 제공합니다.

## 모델 구성

이러한 변수는 [언어 모델](/docs/getting-started/providers)과 그 동작을 제어합니다.

### 기본 공급자 구성

goose를 시작하는 데 필요한 최소 변수입니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_PROVIDER` | 사용할 LLM 공급자 지정 | [사용 가능한 공급자 참조](/docs/getting-started/providers#available-providers) | 없음([구성 필요](/docs/getting-started/providers#configure-provider-and-model)) |
| `GOOSE_MODEL` | 공급자에서 사용할 모델 지정 | 모델 이름(예: "gpt-4", "claude-sonnet-4-20250514") | 없음([구성 필요](/docs/getting-started/providers#configure-provider-and-model)) |
| `GOOSE_TEMPERATURE` | 모델 응답의 [온도](https://medium.com/@kelseyywang/a-comprehensive-guide-to-llm-temperature-%EF%B8%8F-363a40bbc91f) 설정 | 0.0에서 1.0 사이의 실수 | 모델별 기본값 |
| `GOOSE_MAX_TOKENS` | 각 모델 응답의 최대 토큰 수 설정(긴 응답 잘림) | 양의 정수(예: 4096, 8192) | 모델별 기본값 |

**예시**

```bash
# 기본 모델 구성
export GOOSE_PROVIDER="anthropic"
export GOOSE_MODEL="claude-sonnet-4-5-20250929"
export GOOSE_TEMPERATURE=0.7

# 짧은 상호작용을 위해 낮은 제한 설정
export GOOSE_MAX_TOKENS=4096

# 더 긴 출력이 필요한 작업(예: 코드 생성)을 위해 높은 제한 설정
export GOOSE_MAX_TOKENS=16000
```

### 고급 공급자 구성

사용자 정의 엔드포인트, 기업 배포 또는 특정 공급자 구현을 사용할 때 필요한 변수입니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_PROVIDER__TYPE` | 공급자의 특정 유형/구현 | [사용 가능한 공급자 참조](/docs/getting-started/providers#available-providers) | GOOSE_PROVIDER에서 파생 |
| `GOOSE_PROVIDER__HOST` | 공급자의 사용자 정의 API 엔드포인트 | URL(예: "https://api.openai.com") | 공급자별 기본값 |
| `GOOSE_PROVIDER__API_KEY` | 공급자 인증 키 | API 키 문자열 | 없음 |

**예시**

```bash
# 고급 공급자 구성
export GOOSE_PROVIDER__TYPE="anthropic"
export GOOSE_PROVIDER__HOST="https://api.anthropic.com"
export GOOSE_PROVIDER__API_KEY="your-api-key-here"
```

### 리드/워커 모델 구성

이러한 변수는 강력한 리드 모델이 초기 계획과 복잡한 추론을 처리한 다음 실행을 위해 더 빠르고/저렴한 워커 모델로 전환하는 [리드/워커 모델 패턴](/docs/tutorials/lead-worker)을 구성합니다. 전환은 설정에 따라 자동으로 이루어집니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_LEAD_MODEL` | **리드 모드를 활성화하는 데 필수.** 리드 모델 이름 | 모델 이름(예: "gpt-4o", "claude-sonnet-4-20250514") | 없음 |
| `GOOSE_LEAD_PROVIDER` | 리드 모델의 공급자 | [사용 가능한 공급자 참조](/docs/getting-started/providers#available-providers) | `GOOSE_PROVIDER`로 폴백 |
| `GOOSE_LEAD_TURNS` | 워커 모델로 전환하기 전 리드 모델을 사용하는 초기 턴 수 | 정수 | 3 |
| `GOOSE_LEAD_FAILURE_THRESHOLD` | 리드 모델로 폴백하기 전 연속 실패 횟수 | 정수 | 2 |
| `GOOSE_LEAD_FALLBACK_TURNS` | 폴백 모드에서 리드 모델을 사용하는 턴 수 | 정수 | 2 |

_턴_은 하나의 완전한 프롬프트-응답 상호작용입니다. 기본 설정에서 작동 방식:
- 처음 3턴 동안 리드 모델 사용
- 4턴째부터 워커 모델 사용
- 워커 모델이 2턴 연속으로 어려움을 겪으면 리드 모델로 폴백
- 리드 모델을 2턴 사용한 후 워커 모델로 다시 전환

리드 모델과 워커 모델 이름은 goose CLI 세션 시작 시 표시됩니다. 세션에 대해 `GOOSE_MODEL`을 내보내지 않으면 워커 모델은 [구성 파일](/docs/guides/config-files)의 `GOOSE_MODEL`로 기본 설정됩니다.

**예시**

```bash
# 기본 리드/워커 설정
export GOOSE_LEAD_MODEL="o4"

# 고급 리드/워커 구성
export GOOSE_LEAD_MODEL="claude4-opus"
export GOOSE_LEAD_PROVIDER="anthropic"
export GOOSE_LEAD_TURNS=5
export GOOSE_LEAD_FAILURE_THRESHOLD=3
export GOOSE_LEAD_FALLBACK_TURNS=2
```

### 계획 모드 구성

이러한 변수는 goose의 [계획 기능](/docs/guides/creating-plans)을 제어합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_PLANNER_PROVIDER` | 계획 모드에 사용할 공급자 지정 | [사용 가능한 공급자 참조](/docs/getting-started/providers#available-providers) | GOOSE_PROVIDER로 폴백 |
| `GOOSE_PLANNER_MODEL` | 계획 모드에 사용할 모델 지정 | 모델 이름(예: "gpt-4", "claude-sonnet-4-20250514") | GOOSE_MODEL로 폴백 |

**예시**

```bash
# 다른 모델로 계획 모드
export GOOSE_PLANNER_PROVIDER="openai"
export GOOSE_PLANNER_MODEL="gpt-4"
```

### 공급자 재시도

LLM 공급자에 대한 구성 가능한 재시도 파라미터입니다.

#### AWS Bedrock

| 변수 | 목적 | 기본값 |
|---------------------|-------------|---------|
| `BEDROCK_MAX_RETRIES` | 포기하기 전 최대 재시도 횟수 | 6 |
| `BEDROCK_INITIAL_RETRY_INTERVAL_MS` | 첫 번째 재시도 전 대기 시간(밀리초) | 2000 |
| `BEDROCK_BACKOFF_MULTIPLIER` | 각 시도 후 재시도 간격이 증가하는 배수 | 2(매번 두 배) |
| `BEDROCK_MAX_RETRY_INTERVAL_MS` | 재시도 간격의 최대값(밀리초) | 120000 |

**예시**

```bash
export BEDROCK_MAX_RETRIES=10                    # 10회 재시도 시도
export BEDROCK_INITIAL_RETRY_INTERVAL_MS=1000    # 첫 번째 재시도 전 1초로 시작
export BEDROCK_BACKOFF_MULTIPLIER=3              # 각 재시도는 이전보다 3배 더 오래 대기
export BEDROCK_MAX_RETRY_INTERVAL_MS=300000      # 최대 재시도 지연을 5분으로 제한
```

#### Databricks

| 변수 | 목적 | 기본값 |
|---------------------|-------------|---------|
| `DATABRICKS_MAX_RETRIES` | 포기하기 전 최대 재시도 횟수 | 3 |
| `DATABRICKS_INITIAL_RETRY_INTERVAL_MS` | 첫 번째 재시도 전 대기 시간(밀리초) | 1000 |
| `DATABRICKS_BACKOFF_MULTIPLIER` | 각 시도 후 재시도 간격이 증가하는 배수 | 2(매번 두 배) |
| `DATABRICKS_MAX_RETRY_INTERVAL_MS` | 재시도 간격의 최대값(밀리초) | 30000 |

**예시**

```bash
export DATABRICKS_MAX_RETRIES=5                      # 5회 재시도 시도
export DATABRICKS_INITIAL_RETRY_INTERVAL_MS=500      # 첫 번째 재시도 전 0.5초로 시작
export DATABRICKS_BACKOFF_MULTIPLIER=2               # 각 재시도는 이전보다 2배 더 오래 대기
export DATABRICKS_MAX_RETRY_INTERVAL_MS=60000        # 최대 재시도 지연을 1분으로 제한
```


## 세션 관리

이러한 변수는 goose가 대화 세션과 컨텍스트를 관리하는 방법을 제어합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_CONTEXT_STRATEGY` | goose가 컨텍스트 제한 초과 상황을 처리하는 방법 제어 | "summarize", "truncate", "clear", "prompt" | "prompt"(대화형), "summarize"(헤드리스) |
| `GOOSE_MAX_TURNS` | 사용자 입력 없이 허용되는 [최대 턴 수](/docs/guides/sessions/smart-context-management#maximum-turns) | 정수(예: 10, 50, 100) | 1000 |
| `GOOSE_SUBAGENT_MAX_TURNS` | [서브에이전트](/docs/guides/subagents)가 타임아웃 전에 완료할 수 있는 최대 턴 수 설정 | 정수(예: 25) | 25 |
| `CONTEXT_FILE_NAMES` | [힌트/컨텍스트 파일](/docs/guides/context-engineering/using-goosehints#custom-context-files)의 사용자 정의 파일 이름 지정 | JSON 문자열 배열(예: `["CLAUDE.md", ".goosehints"]`) | `[".goosehints"]` |
| `GOOSE_CLI_THEME` | CLI 응답 마크다운 [테마](/docs/guides/goose-cli-commands#themes) | "light", "dark", "ansi" | "dark" |
| `GOOSE_RANDOM_THINKING_MESSAGES` | 처리 중 재미있는 무작위 메시지 표시 여부 제어 | "true", "false" | "true" |
| `GOOSE_CLI_SHOW_COST` | CLI 출력에서 모델 비용 추정치 표시 전환 | "true", "1"(대소문자 구분 없음)로 활성화 | false |
| `GOOSE_AUTO_COMPACT_THRESHOLD` | goose가 [세션을 자동으로 요약](/docs/guides/sessions/smart-context-management#automatic-compaction)하는 백분율 임계값 설정 | 0.0에서 1.0 사이의 실수(0.0에서 비활성화) | 0.8 |

**예시**

```bash
# 컨텍스트 제한에 도달하면 자동으로 요약
export GOOSE_CONTEXT_STRATEGY=summarize

# 항상 사용자에게 선택 프롬프트(대화형 모드 기본값)
export GOOSE_CONTEXT_STRATEGY=prompt

# 단계별 제어를 위해 낮은 제한 설정
export GOOSE_MAX_TURNS=5

# 제어된 자동화를 위해 적당한 제한 설정
export GOOSE_MAX_TURNS=25

# 프로덕션을 위해 합리적인 제한 설정
export GOOSE_MAX_TURNS=100

# 서브에이전트 턴 제한 사용자 정의
export GOOSE_SUBAGENT_MAX_TURNS=50

# 여러 컨텍스트 파일 사용
export CONTEXT_FILE_NAMES='["CLAUDE.md", ".goosehints", ".cursorrules", "project_rules.txt"]'

# 세션에 ANSI 테마 설정
export GOOSE_CLI_THEME=ansi

# 방해를 줄이기 위해 무작위 생각 메시지 비활성화
export GOOSE_RANDOM_THINKING_MESSAGES=false

# CLI에서 모델 비용 표시 활성화
export GOOSE_CLI_SHOW_COST=true

# 사용 가능한 토큰의 60%가 사용되면 세션 자동 압축
export GOOSE_AUTO_COMPACT_THRESHOLD=0.6
```

### 모델 컨텍스트 제한 재정의

이러한 변수를 사용하면 모델의 기본 컨텍스트 윈도우 크기(토큰 제한)를 재정의할 수 있습니다. [LiteLLM 프록시](https://docs.litellm.ai/docs/providers/litellm_proxy)를 사용하거나 goose의 사전 정의된 모델 패턴과 일치하지 않는 사용자 정의 모델을 사용할 때 특히 유용합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_CONTEXT_LIMIT` | 메인 모델의 컨텍스트 제한 재정의 | 정수(토큰 수) | 모델별 기본값 또는 128,000 |
| `GOOSE_LEAD_CONTEXT_LIMIT` | [리드/워커 모드](/docs/tutorials/lead-worker)에서 리드 모델의 컨텍스트 제한 재정의 | 정수(토큰 수) | `GOOSE_CONTEXT_LIMIT` 또는 모델 기본값으로 폴백 |
| `GOOSE_WORKER_CONTEXT_LIMIT` | 리드/워커 모드에서 워커 모델의 컨텍스트 제한 재정의 | 정수(토큰 수) | `GOOSE_CONTEXT_LIMIT` 또는 모델 기본값으로 폴백 |
| `GOOSE_PLANNER_CONTEXT_LIMIT` | [플래너 모델](/docs/guides/creating-plans)의 컨텍스트 제한 재정의 | 정수(토큰 수) | `GOOSE_CONTEXT_LIMIT` 또는 모델 기본값으로 폴백 |

**예시**

```bash
# 메인 모델의 컨텍스트 제한 설정(LiteLLM 프록시에 유용)
export GOOSE_CONTEXT_LIMIT=200000

# 리드/워커 모델에 다른 컨텍스트 제한 설정
export GOOSE_LEAD_CONTEXT_LIMIT=500000   # 계획을 위한 큰 컨텍스트
export GOOSE_WORKER_CONTEXT_LIMIT=128000 # 실행을 위한 작은 컨텍스트

# 플래너의 컨텍스트 제한 설정
export GOOSE_PLANNER_CONTEXT_LIMIT=1000000
```

자세한 내용과 예시는 [모델 컨텍스트 제한 재정의](/docs/guides/sessions/smart-context-management#model-context-limit-overrides)를 참조하세요.

## 도구 구성

이러한 변수는 goose가 [도구 실행](/docs/guides/goose-permissions)과 [도구 관리](/docs/guides/managing-tools/)를 처리하는 방법을 제어합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_MODE` | goose가 도구 실행을 처리하는 방법 제어 | "auto", "approve", "chat", "smart_approve" | "smart_approve" |
| `GOOSE_TOOLSHIM` | 도구 호출 해석 활성화/비활성화 | "1", "true"(대소문자 구분 없음)로 활성화 | false |
| `GOOSE_TOOLSHIM_OLLAMA_MODEL` | [도구 호출 해석](/docs/experimental/ollama)용 모델 지정 | 모델 이름(예: llama3.2, qwen2.5) | 시스템 기본값 |
| `GOOSE_CLI_MIN_PRIORITY` | [도구 출력](/docs/guides/managing-tools/adjust-tool-output)의 상세 수준 제어 | 0.0에서 1.0 사이의 실수 | 0.0 |
| `GOOSE_CLI_TOOL_PARAMS_TRUNCATION_MAX_LENGTH` | CLI 출력에서 도구 파라미터 값의 최대 길이(디버그 모드가 아닐 때) | 정수 | 40 |
| `GOOSE_DEBUG` | 잘림 없이 전체 도구 파라미터를 표시하는 디버그 모드 활성화 | "1", "true"(대소문자 구분 없음)로 활성화 | false |
| `GOOSE_SEARCH_PATHS` | 확장 기능 실행 시 실행 파일을 검색할 추가 디렉토리 | JSON 경로 배열(예: `["/usr/local/bin", "~/custom/bin"]`) | 시스템 PATH만 | 아니오 |

**예시**

```bash
# 도구 해석 활성화
export GOOSE_TOOLSHIM=true
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
export GOOSE_MODE="auto"
export GOOSE_CLI_MIN_PRIORITY=0.2  # 중간 및 높은 중요도 출력만 표시
export GOOSE_CLI_TOOL_PARAMS_MAX_LENGTH=100  # CLI 출력에서 도구 파라미터에 최대 100자 표시

# 확장 기능을 위한 사용자 정의 도구 디렉토리 추가
export GOOSE_SEARCH_PATHS='["/usr/local/bin", "~/custom/tools", "/opt/homebrew/bin"]'
```

이러한 경로는 확장 기능이 명령을 실행할 때 시스템 PATH 앞에 추가되어 전역 PATH를 수정하지 않고도 사용자 정의 도구를 찾을 수 있습니다.

### 향상된 코드 편집

이러한 변수는 Developer 확장 기능의 `str_replace` 도구에 대한 [AI 기반 코드 편집](/docs/guides/enhanced-code-editing)을 구성합니다. 기능이 활성화되려면 세 변수 모두 설정되어 있고 비어 있지 않아야 합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_EDITOR_API_KEY` | 코드 편집 모델용 API 키 | API 키 문자열 | 없음 |
| `GOOSE_EDITOR_HOST` | 코드 편집 모델용 API 엔드포인트 | URL(예: "https://api.openai.com/v1") | 없음 |
| `GOOSE_EDITOR_MODEL` | 코드 편집에 사용할 모델 | 모델 이름(예: "gpt-4o", "claude-sonnet-4") | 없음 |

**예시**

이 기능은 모든 OpenAI 호환 API 엔드포인트에서 작동합니다. 예:

```bash
# OpenAI 구성
export GOOSE_EDITOR_API_KEY="sk-..."
export GOOSE_EDITOR_HOST="https://api.openai.com/v1"
export GOOSE_EDITOR_MODEL="gpt-4o"

# Anthropic 구성(OpenAI 호환 프록시를 통해)
export GOOSE_EDITOR_API_KEY="sk-ant-..."
export GOOSE_EDITOR_HOST="https://api.anthropic.com/v1"
export GOOSE_EDITOR_MODEL="claude-sonnet-4-20250514"

# 로컬 모델 구성
export GOOSE_EDITOR_API_KEY="your-key"
export GOOSE_EDITOR_HOST="http://localhost:8000/v1"
export GOOSE_EDITOR_MODEL="your-model"
```

## 보안 구성

이러한 변수는 보안 관련 기능을 제어합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_ALLOWLIST` | 로드할 수 있는 확장 기능 제어 | [허용된 확장 기능](/docs/guides/allowlist) 목록 URL | 미설정 |
| `GOOSE_DISABLE_KEYRING` | 시크릿 저장을 위한 시스템 키링 비활성화 | 아무 값이나 설정(예: "1", "true", "yes")하여 비활성화. 실제 값은 중요하지 않으며 변수 설정 여부만 중요 | 미설정(키링 활성화) |
| `SECURITY_PROMPT_ENABLED` | 잠재적으로 유해한 명령을 식별하기 위한 [프롬프트 주입 감지](/docs/guides/security/prompt-injection-detection) 활성화 | true/false | false |
| `SECURITY_PROMPT_THRESHOLD` | 프롬프트 주입 감지 민감도 임계값(높을수록 엄격함) | 0.01에서 1.0 사이의 실수 | 0.8 |
| `SECURITY_PROMPT_CLASSIFIER_ENABLED` | 고급 위협 식별을 위한 ML 기반 프롬프트 주입 감지 활성화 | true/false | false |
| `SECURITY_PROMPT_CLASSIFIER_ENDPOINT` | ML 기반 프롬프트 주입 감지를 위한 분류 엔드포인트 URL | URL(예: "https://api.example.com/classify") | 미설정 |
| `SECURITY_PROMPT_CLASSIFIER_TOKEN` | `SECURITY_PROMPT_CLASSIFIER_ENDPOINT`용 인증 토큰 | 문자열 | 미설정 |

**예시**

```bash
# 기본 임계값으로 프롬프트 주입 감지 활성화
export SECURITY_PROMPT_ENABLED=true

# 사용자 정의 임계값으로 활성화(더 엄격함)
export SECURITY_PROMPT_ENABLED=true
export SECURITY_PROMPT_THRESHOLD=0.9

# 외부 엔드포인트로 ML 기반 감지 활성화
export SECURITY_PROMPT_ENABLED=true
export SECURITY_PROMPT_CLASSIFIER_ENABLED=true
export SECURITY_PROMPT_CLASSIFIER_ENDPOINT="https://your-endpoint.com/classify"
export SECURITY_PROMPT_CLASSIFIER_TOKEN="your-auth-token"
```

:::tip
키링이 비활성화되면 시크릿은 다음 위치에 저장됩니다:

* macOS/Linux: `~/.config/goose/secrets.yaml`
* Windows: `%APPDATA%\Block\goose\config\secrets.yaml`
:::

## 네트워크 구성

이러한 변수는 goose의 네트워크 프록시 설정을 구성합니다.

### HTTP 프록시

goose는 기업 방화벽이나 프록시 서버 뒤에 있는 사용자를 위한 표준 HTTP 프록시 환경 변수를 지원합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `HTTP_PROXY` | HTTP 연결용 프록시 URL | URL(예: `http://proxy.company.com:8080`) | 없음 |
| `HTTPS_PROXY` | HTTPS 연결용 프록시 URL(둘 다 설정된 경우 `HTTP_PROXY`보다 우선) | URL(예: `http://proxy.company.com:8080`) | 없음 |
| `NO_PROXY` | 프록시를 우회할 호스트 | 쉼표로 구분된 목록(예: `localhost,127.0.0.1,.internal.com`) | 없음 |

**예시**

```bash
# 모든 연결에 프록시 구성
export HTTPS_PROXY="http://proxy.company.com:8080"
export NO_PROXY="localhost,127.0.0.1,.internal,.local,10.0.0.0/8"

# 또는 인증과 함께
export HTTPS_PROXY="http://username:password@proxy.company.com:8080"
export NO_PROXY="localhost,127.0.0.1,.internal"
```

## 관측성

goose의 내장 [로깅 시스템](/docs/guides/logs) 외에도 고급 모니터링, 성능 분석 및 프로덕션 인사이트를 위해 외부 관측성 플랫폼으로 텔레메트리를 내보낼 수 있습니다.

### OpenTelemetry 프로토콜(OTLP)

OTLP 호환 관측성 플랫폼으로 트레이스와 메트릭을 내보내도록 goose를 구성합니다.
OTLP는 [OpenTelemetry](https://opentelemetry.io/docs/)에서 수집한 텔레메트리를 전송하기 위한 표준 프로토콜입니다. 구성되면 goose는 텔레메트리를 비동기적으로 내보내고 종료 시 플러시합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|--------|---------|
| `OTEL_EXPORTER_OTLP_ENDPOINT` | OTLP 엔드포인트 URL | URL(예: `http://localhost:4318`) | 없음 |
| `OTEL_EXPORTER_OTLP_TIMEOUT` | 내보내기 타임아웃(밀리초) | 정수(ms) | `10000` |

**OTLP를 사용할 때:**
- 느린 도구 실행 또는 LLM 응답 시간 진단
- 여러 세션에서 간헐적 실패 이해
- 프로덕션 또는 CI/CD 환경에서 goose 성능 모니터링
- 시간 경과에 따른 사용 패턴, 비용 및 리소스 소비 추적
- 성능 저하 또는 높은 오류율에 대한 알림 설정

**예시:**
```bash
export OTEL_EXPORTER_OTLP_ENDPOINT="http://localhost:4318"
export OTEL_EXPORTER_OTLP_TIMEOUT=10000
```

### Langfuse 통합

이러한 변수는 [관측성을 위한 Langfuse 통합](/docs/tutorials/langfuse)을 구성합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `LANGFUSE_PUBLIC_KEY` | Langfuse 통합용 공개 키 | 문자열 | 없음 |
| `LANGFUSE_SECRET_KEY` | Langfuse 통합용 비밀 키 | 문자열 | 없음 |
| `LANGFUSE_URL` | Langfuse 서비스용 사용자 정의 URL | URL 문자열 | 기본 Langfuse URL |
| `LANGFUSE_INIT_PROJECT_PUBLIC_KEY` | Langfuse용 대체 공개 키 | 문자열 | 없음 |
| `LANGFUSE_INIT_PROJECT_SECRET_KEY` | Langfuse용 대체 비밀 키 | 문자열 | 없음 |

## 레시피 구성

이러한 변수는 레시피 검색 및 관리를 제어합니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_RECIPE_PATH` | 레시피를 검색할 추가 디렉토리 | Unix에서 콜론으로 구분, Windows에서 세미콜론으로 구분된 경로 | 없음 |
| `GOOSE_RECIPE_GITHUB_REPO` | 레시피를 검색할 GitHub 저장소 | 형식: "owner/repo"(예: "block/goose-recipes") | 없음 |
| `GOOSE_RECIPE_RETRY_TIMEOUT_SECONDS` | 레시피 성공 확인 명령의 전역 타임아웃 | 정수(초) | 레시피별 기본값 |
| `GOOSE_RECIPE_ON_FAILURE_TIMEOUT_SECONDS` | 레시피 on_failure 명령의 전역 타임아웃 | 정수(초) | 레시피별 기본값 |

**예시**

```bash
# 사용자 정의 레시피 디렉토리 추가
export GOOSE_RECIPE_PATH="/path/to/my/recipes:/path/to/team/recipes"

# GitHub 레시피 저장소 구성
export GOOSE_RECIPE_GITHUB_REPO="myorg/goose-recipes"

# 전역 레시피 타임아웃 설정
export GOOSE_RECIPE_RETRY_TIMEOUT_SECONDS=300
export GOOSE_RECIPE_ON_FAILURE_TIMEOUT_SECONDS=60
```

## 실험적 기능

이러한 변수는 활발히 개발 중인 실험적 기능을 활성화합니다. 향후 릴리스에서 변경되거나 제거될 수 있습니다. 프로덕션 환경에서는 주의해서 사용하세요.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `ALPHA_FEATURES` | 실험적 알파 기능 활성화&mdash;이 플래그가 필요한지 기능 문서를 확인하세요 | "true", "1"(대소문자 구분 없음)로 활성화 | false |

**예시**

```bash
# 알파 기능 활성화
export ALPHA_FEATURES=true

# 또는 단일 세션에 활성화
ALPHA_FEATURES=true goose session
```

## 개발 및 테스트

이러한 변수는 주로 goose 자체의 개발, 테스트 및 디버깅에 사용됩니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_PATH_ROOT` | 모든 goose 데이터, 구성 및 상태 파일의 루트 디렉토리 재정의 | 디렉토리 절대 경로 | 플랫폼별 기본값 |

**기본 위치:**
- macOS: `~/Library/Application Support/Block/goose/`
- Linux: `~/.local/share/goose/`
- Windows: `%APPDATA%\Block\goose\`

설정되면 goose는 지정된 경로 아래에 `config/`, `data/` 및 `state/` 하위 디렉토리를 생성합니다. 테스트 환경 격리, 여러 구성 실행 또는 CI/CD 파이프라인에 유용합니다.

**예시**

```bash
# 임시 테스트 환경
export GOOSE_PATH_ROOT="/tmp/goose-test"

# 단일 명령에 격리된 환경
GOOSE_PATH_ROOT="/tmp/goose-isolated" goose run --recipe my-recipe.yaml

# CI/CD 사용
GOOSE_PATH_ROOT="$(mktemp -d)" goose run --recipe integration-test.yaml

# 개발자 도구와 함께 사용
GOOSE_PATH_ROOT="/tmp/goose-test" ./scripts/goose-db-helper.sh status
```

## goose가 제어하는 변수

이러한 변수는 명령 실행 중 goose에 의해 자동으로 설정됩니다.

| 변수 | 목적 | 값 | 기본값 |
|----------|---------|---------|---------|
| `GOOSE_TERMINAL` | 명령이 goose에 의해 실행되고 있음을 나타내며, 셸 동작 사용자 정의 가능 | 설정 시 "1" | 미설정 |

### 셸 동작 사용자 정의

때로는 일반 터미널 사용과 다른 명령이나 다른 셸 동작을 goose가 사용하길 원할 수 있습니다. 예를 들어 goose가 다른 도구를 사용하게 하거나, goose가 `git commit`을 실행하지 못하게 하거나, AI 에이전트를 중단시킬 수 있는 장시간 실행되는 개발 서버를 차단하고 싶을 수 있습니다. 이는 셸 명령이 터미널 환경에서 직접 실행되는 goose CLI를 사용할 때 가장 유용합니다.

**작동 방식:**
1. goose가 명령을 실행할 때 `GOOSE_TERMINAL`이 자동으로 "1"로 설정됩니다
2. 셸 구성이 이를 감지하고 일반 터미널 사용은 변경하지 않으면서 동작을 변경할 수 있습니다

**예시:**

```bash
# ~/.zshenv(zsh 사용자용) 또는 ~/.bashrc(bash 사용자용)에서

# goose가 실행할 때 git commit 차단
if [[ -n "$GOOSE_TERMINAL" ]]; then
  git() {
    if [[ "$1" == "commit" ]]; then
      echo "❌ BLOCKED: git commit is not allowed when run by goose"
      return 1
    fi
    command git "$@"
  }
fi
```

```bash
# goose를 더 나은 도구 선택으로 안내
if [[ -n "$GOOSE_TERMINAL" ]]; then
  alias find="echo 'Use rg instead: rg --files | rg <pattern> for filenames, or rg <pattern> for content search'"
fi
```

## 기업 환경

기업 환경에서 goose를 배포할 때 관리자는 동작과 인프라를 제어하거나 팀 전체에서 일관된 설정을 적용해야 할 수 있습니다. 다음 환경 변수가 일반적으로 사용됩니다:

**네트워크 및 인프라** - goose가 외부 서비스 및 내부 인프라에 연결하는 방법 제어:
- [네트워크 구성](#네트워크-구성) - 프록시 구성 및 네트워크 설정
- [고급 공급자 구성](#고급-공급자-구성) - 내부 LLM 엔드포인트 지정(예: Databricks, 사용자 정의 배포)
- [모델 컨텍스트 제한 재정의](#모델-컨텍스트-제한-재정의) - LiteLLM 프록시 및 사용자 정의 모델의 컨텍스트 제한 구성

**보안 및 액세스 제어** - 실행할 수 있는 확장 기능과 시크릿 저장 방법 관리:

- [보안 구성](#보안-구성) - 확장 기능 로딩(`GOOSE_ALLOWLIST`) 및 시크릿 관리(`GOOSE_DISABLE_KEYRING`) 제어

**규정 준수 및 모니터링** - 감사를 위해 사용량 추적 및 텔레메트리 내보내기:

- [관측성](#관측성) - 모니터링 플랫폼(OTLP, Langfuse)으로 텔레메트리 내보내기

## 참고사항

- 환경 변수는 구성 파일보다 우선합니다.
- 보안에 민감한 변수(API 키 등)의 경우 환경 변수 대신 시스템 키링 사용을 고려하세요.
- 일부 변수는 goose를 다시 시작해야 적용됩니다.
- 계획 모드를 사용할 때 플래너별 변수가 설정되지 않으면 goose는 메인 모델 구성으로 폴백합니다.
