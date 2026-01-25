---
sidebar_position: 2
title: 레시피 참조 가이드
sidebar_label: 레시피 참조
description: goose에서 레시피를 만들고 사용자 정의하기 위한 완전한 기술 참조
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

레시피는 지침과 설정을 패키징하여 설정을 쉽게 공유하고 다른 사람이 실행할 수 있도록 하는 재사용 가능한 goose 구성입니다.

## 레시피 파일 형식

레시피는 다음으로 정의할 수 있습니다:
- `.yaml` (권장) 및 `.yml` 파일
- `.json` 파일

:::info
`.yml` 파일은 goose CLI에서 지원되지 않습니다.
:::

레시피를 만들고 사용하고 관리하는 방법은 [공유 가능한 레시피](/docs/guides/recipes/session-recipes)를 참조하세요.

## 레시피 위치

레시피는 다음에서 로드할 수 있습니다:

1. 로컬 파일 시스템:
   - 현재 디렉토리
   - [`GOOSE_RECIPE_PATH`](/docs/guides/environment-variables#recipe-configuration) 환경 변수에 지정된 디렉토리

2. GitHub 리포지토리:
   - [`GOOSE_RECIPE_GITHUB_REPO`](/docs/guides/environment-variables#recipe-configuration) 구성 키를 사용하여 구성
   - GitHub CLI (`gh`)가 설치되고 인증되어야 함

## 핵심 레시피 스키마

레시피는 이 스키마 구조를 따릅니다:

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `description` | String | ✅ | 레시피가 하는 일에 대한 자세한 설명 |
| `instructions` | String | ✅*  | 매개변수 대체를 포함할 수 있는 템플릿 지침 |
| `prompt` | String| ✅*   | 매개변수 대체를 포함할 수 있는 템플릿 프롬프트. [헤드리스](/docs/tutorials/headless-goose) (비대화형) 모드에서 필수. |
| `title` | String | ✅ | 레시피를 설명하는 짧은 제목 |
| [`activities`](#activities) | Array | - | 매개변수 대체를 포함할 수 있는 예시 프롬프트 목록. 활동은 goose 데스크톱에서 클릭 가능한 버블로 나타납니다. |
| [`extensions`](#extensions) | Array | - | 확장 기능 구성 목록 |
| [`parameters`](#parameters) | Array | - | 동적 레시피를 위한 매개변수 정의 목록 |
| [`response`](#response) | Object | - | 자동화 워크플로우를 위한 구조화된 출력 스키마 |
| [`retry`](#retry) | Object | - | 성공 검증을 통한 자동화된 재시도 로직 구성 |
| [`settings`](#settings) | Object | - | 모델 프로바이더, 모델 이름 및 기타 설정 구성 |
| [`sub_recipes`](#subrecipes) | Array | - | 서브레시피 목록 |
| `version` | String | - | 레시피 형식 버전, 생략 시 기본값 "1.0.0" |

*`instructions` 또는 `prompt` 중 하나 이상을 제공해야 합니다.

## 필드 사양

### Activities

`activities` 필드는 goose 데스크톱에서 레시피를 열 때 나타나는 선택적 메시지와 클릭 가능한 활동 버블(버튼)을 정의합니다.

:::info 데스크톱 전용
활동은 데스크톱 전용 기능입니다. 활동이 있는 레시피가 CLI를 통해 또는 예약된 작업으로 실행되면 `activities` 필드는 무시되고 레시피 실행에 영향을 미치지 않습니다.
:::

#### 활동 유형

활동은 두 가지 방법으로 정의할 수 있습니다:

1. **메시지 활동**: 활동 버블 위의 정보 상자에 마크다운 형식의 활동 텍스트를 표시합니다. 예:

   ```
   activities:
     - "message: **환영합니다!** 제가 도울 수 있는 내용은 다음과 같습니다:\n\n• 📊 데이터 분석\n• 🔍 코드 리뷰\n• 📝 문서화\n\n아래에서 옵션을 선택하여 시작하세요."
   ```

   `message:` 접두사가 있는 활동은 하나만 포함하세요. 추가 `message:` 접두사 활동은 일반 클릭 가능한 버블이 됩니다 (리터럴 "message:" 텍스트를 표시).

2. **버튼 활동**: 활동 버블에 표시할 텍스트로, 클릭 시 활동 텍스트를 프롬프트로 전송합니다

#### 매개변수 대체

활동은 [매개변수 대체](#parameters)를 지원하여 동적이고 개인화된 활동 버블을 만들 수 있습니다. 사용자가 **Recipe Parameters** 대화 상자에서 매개변수 값을 제공하면 버블이 표시되기 전에 값이 활동 텍스트에 대체됩니다.

#### 구성 예시

<Tabs groupId="format">
  <TabItem value="yaml" label="YAML" default>
    ```yaml
    version: "1.0.0"
    title: "Code Review Assistant"
    description: "Review code with customizable focus areas"
    parameters:
      - key: language
        input_type: string
        requirement: required
        description: "Programming language to review"
      - key: focus
        input_type: string
        requirement: optional
        default: "best practices"
        description: "Review focus area"

    activities:
      - "message: Click an option below to start reviewing {{ language }} code with a focus on {{ focus }}."
      - "Review the current file for {{ focus }}"
      - "Suggest improvements for {{ language }} code quality"
      - "Check for security vulnerabilities"
      - "Generate unit tests"
    ```
  </TabItem>
  <TabItem value="json" label="JSON">
    ```json
    {
      "version": "1.0.0",
      "title": "Code Review Assistant",
      "description": "Review code with customizable focus areas",
      "parameters": [
        {
          "key": "language",
          "input_type": "string",
          "requirement": "required",
          "description": "Programming language to review"
        },
        {
          "key": "focus",
          "input_type": "string",
          "requirement": "optional",
          "default": "best practices",
          "description": "Review focus area"
        }
      ],
      "activities": [
        "message: Click an option below to start reviewing {{ language }} code with a focus on {{ focus }}.",
        "Review the current file for {{ focus }}",
        "Suggest improvements for {{ language }} code quality",
        "Check for security vulnerabilities",
        "Generate unit tests"
      ]
    }
    ```
  </TabItem>
</Tabs>

이 예시에서:
- 메시지 활동은 대체된 매개변수 값과 함께 지침을 표시합니다. 예: "Click an option below to start reviewing rust code with a focus on best practices."
- 처음 두 활동 버블은 매개변수 대체를 사용합니다. 예: "Review the current file for best practices"
- 마지막 두 활동 버블은 매개변수에 관계없이 작동하는 정적 프롬프트입니다

### Extensions

`extensions` 필드를 사용하면 레시피가 작동하는 데 필요한 Model Context Protocol (MCP) 서버 및 기타 확장 기능을 지정할 수 있습니다. `extensions` 배열의 각 확장 기능에는 다음 스키마가 있습니다:

#### 확장 기능 스키마

| 필드 | 타입 | 설명 |
|-------|------|-------------|
| `type` | String | 확장 기능 유형 (예: "stdio") |
| `name` | String | 확장 기능의 고유 이름 |
| `cmd` | String | 확장 기능을 실행하는 명령 |
| `args` | Array | 명령에 대한 인수 목록 |
| `env_keys` | Array | (선택 사항) 확장 기능에 필요한 환경 변수 이름 |
| `timeout` | Number | 타임아웃 (초) |
| `bundled` | Boolean | (선택 사항) 확장 기능이 goose와 번들로 제공되는지 여부 |
| `description` | String | 확장 기능이 하는 일에 대한 설명 |
| `available_tools` | Array | 사용 가능한 확장 기능 내 도구 이름 목록. 지정하지 않으면 모두 사용 가능 |

#### 확장 기능 유형

- **`stdio`**: 명령과 인수가 있는 표준 I/O 클라이언트
- **`builtin`**: 번들된 goose MCP 서버의 일부인 내장 확장 기능
- **`platform`**: 에이전트 프로세스에서 실행되는 플랫폼 확장 기능
- **`streamable_http`**: URI 엔드포인트가 있는 스트리밍 가능한 HTTP 클라이언트
- **`frontend`**: 프론트엔드를 통해 호출되는 프론트엔드 제공 도구
- **`inline_python`**: uvx를 사용하여 실행되는 인라인 Python 코드. `code` 필드 필수; 패키지용 선택적 `dependencies`.

#### 확장 기능 구성 예시

<Tabs groupId="format">
  <TabItem value="yaml" label="YAML" default>

```yaml
extensions:
  - type: stdio
    name: codesearch
    cmd: uvx
    args:
      - mcp_codesearch@latest
    timeout: 300
    bundled: true
    description: "Query https://codesearch.sqprod.co/ directly from goose"

  - type: stdio
    name: presidio
    timeout: 300
    cmd: uvx
    args:
      - 'mcp_presidio@latest'
    available_tools:
      - query_logs

  - type: stdio
    name: github-mcp
    cmd: github-mcp-server
    args: []
    env_keys:
      - GITHUB_PERSONAL_ACCESS_TOKEN
    timeout: 60
    description: "GitHub MCP extension for repository operations"

  - type: inline_python
    name: data_processor
    code: |
      import pandas as pd
      print("Processing data...")
    dependencies:
      - pandas
      - numpy
    timeout: 120
    description: "Process data using pandas"
```

  </TabItem>
  <TabItem value="json" label="JSON">

```json
{
  "extensions": [
    {
      "type": "stdio",
      "name": "codesearch",
      "cmd": "uvx",
      "args": ["mcp_codesearch@latest"],
      "timeout": 300,
      "bundled": true,
      "description": "Query https://codesearch.sqprod.co/ directly from goose"
    },
    {
      "type": "stdio",
      "name": "presidio",
      "timeout": 300,
      "cmd": "uvx",
      "args": ["mcp_presidio@latest"],
      "available_tools": ["query_logs"]
    },
    {
      "type": "stdio",
      "name": "github-mcp",
      "cmd": "github-mcp-server",
      "args": [],
      "env_keys": ["GITHUB_PERSONAL_ACCESS_TOKEN"],
      "timeout": 60,
      "description": "GitHub MCP extension for repository operations"
    },
    {
      "type": "inline_python",
      "name": "data_processor",
      "code": "import pandas as pd\nprint(\"Processing data...\")",
      "dependencies": ["pandas", "numpy"],
      "timeout": 120,
      "description": "Process data using pandas"
    }
  ]
}
```

  </TabItem>
</Tabs>

#### 확장 기능 비밀

이 기능은 CLI를 통해서만 사용할 수 있습니다.

레시피가 비밀이 필요한 확장 기능을 사용하는 경우 goose는 레시피를 실행할 때 사용자에게 비밀을 제공하라는 메시지를 표시할 수 있습니다:

1. 레시피가 로드되면 goose는 모든 확장 기능(서브레시피 포함)에서 `env_keys` 필드를 스캔합니다
2. 필요한 환경 변수가 보안 키링에 없으면 goose는 사용자에게 입력하라는 메시지를 표시합니다
3. 값은 시스템 키링에 안전하게 저장되고 이후 실행에서 재사용됩니다

저장된 비밀을 업데이트하려면 시스템 키링에서 제거하고 레시피를 다시 실행하여 다시 프롬프트를 받으세요.

:::info
이 기능은 비밀(예: API 키)을 프롬프트하고 안전하게 저장하도록 설계되었지만 `env_keys`에는 확장 기능에 필요한 모든 환경 변수(예: API 엔드포인트, 구성 값 등)가 포함될 수 있습니다.

사용자는 확장 기능에서 선택 사항인 경우 `ESC`를 눌러 변수 입력을 건너뛸 수 있습니다.
:::

### Parameters

`parameters` 필드를 사용하면 다양한 컨텍스트에 맞게 사용자 정의할 수 있는 동적이고 재사용 가능한 레시피를 만들 수 있습니다. 매개변수는 사용자가 레시피를 실행할 때 입력하는 플레이스홀더를 정의하여 레시피를 더 유연하고 적응 가능하게 만듭니다.

매개변수 대체는 `{{ parameter_name }}` 플레이스홀더와 함께 Jinja 스타일 템플릿 구문을 사용합니다. `parameters` 배열의 각 매개변수에는 다음 스키마가 있습니다:

#### 매개변수 스키마

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `key` | String | ✅ | 매개변수의 고유 식별자 |
| `input_type` | String | ✅ | 입력 유형: `"string"` (기본값), `"number"`, `"boolean"`, `"date"`, `"file"` 또는 `"select"` |
| `requirement` | String | ✅ | "required", "optional" 또는 "user_prompt" 중 하나 |
| `description` | String | ✅ | 매개변수에 대한 사람이 읽을 수 있는 설명 |
| `default` | String | - | 선택적 매개변수의 기본값 |
| `options` | Array | - | 사용 가능한 선택 항목 목록 (`select` 입력 유형에 필수) |

#### 매개변수 요구 사항

- `required`: 레시피 사용 시 매개변수를 반드시 제공해야 함
- `optional`: 기본값이 지정된 경우 생략 가능
- `user_prompt`: 제공되지 않으면 대화형으로 사용자에게 입력을 요청

`required` 및 `optional` 매개변수는 goose 데스크톱에서 열리는 레시피에 가장 적합합니다. `user_prompt` 매개변수에 값이 제공되지 않으면 매개변수가 대체되지 않고 레시피 출력에 리터럴 `{{ parameter_name }}` 텍스트로 나타날 수 있습니다.

#### 입력 유형

- `string`: 기본 유형. 매개변수 값은 템플릿 대체에서 그대로 사용됩니다
- `number`: 숫자 값. 데스크톱 UI는 숫자 입력 검증을 제공합니다
- `boolean`: True/false 값. 데스크톱 UI는 "True"/"False" 옵션이 있는 드롭다운을 표시합니다
- `date`: 날짜 값. 현재 텍스트 입력으로 렌더링됩니다
- `file`: 매개변수 값은 파일 경로여야 합니다. goose는 파일 내용을 읽고 템플릿에 경로가 아닌 실제 내용을 대체합니다
- `select`: 미리 정의된 옵션이 있는 드롭다운 선택. `options` 필드 필수

**예시:**
```yaml
parameters:
  - key: max_files
    input_type: number
    requirement: optional
    default: "10"
    description: "Maximum files to process"

  - key: output_format
    input_type: select
    requirement: required
    description: "Choose output format"
    options:
      - json
      - markdown
      - csv

  - key: enable_debug
    input_type: boolean
    requirement: optional
    default: "false"
    description: "Enable debug mode"

  - key: source_code
    input_type: file
    requirement: required
    description: "Path to the source code file to analyze"

prompt: "Process {{ max_files }} files in {{ output_format }} format. Debug: {{ enable_debug }}. Code:\n\n{{ source_code }}"
```

:::important
- 선택적 매개변수는 반드시 기본값을 지정해야 합니다
- 필수 매개변수에는 기본값을 지정할 수 없습니다
- 파일 매개변수는 민감한 파일의 의도하지 않은 가져오기를 방지하기 위해 요구 사항 유형에 관계없이 기본값을 가질 수 없습니다
- Select 매개변수는 사용 가능한 선택 항목이 있는 `options` 필드가 있어야 합니다
- 매개변수 키는 지침, 프롬프트 또는 활동에서 사용되는 모든 템플릿 변수와 일치해야 합니다
:::

#### 데스크톱에서의 매개변수 대체

매개변수가 있는 레시피가 goose 데스크톱에서 열리면 사용자에게 **Recipe Parameters** 대화 상자가 표시되어 다음을 수행할 수 있습니다:
- 필수 매개변수에 값 제공
- 선택적 매개변수의 기본값 수정 또는 수락
- `user_prompt` 매개변수에 값 입력

매개변수 값이 제출되면 레시피가 시작되기 전에 레시피의 `instructions`, `prompt` 및 `activities` 필드에 대체됩니다.

### Response

`response` 필드를 사용하면 레시피가 최종 구조화된 JSON 출력을 강제할 수 있습니다. `json_schema`를 지정하면 goose는:

1. **출력 검증**: 기본 JSON 스키마 검증으로 출력 JSON을 JSON 스키마에 대해 검증합니다
2. **최종 구조화된 출력**: 에이전트의 최종 출력이 JSON 구조와 일치하는 응답인지 확인합니다

이 기능은 일관되고 파싱 가능한 출력을 보장하기 위한 **비대화형 자동화**용으로 설계되었습니다. 레시피는 goose CLI 또는 goose 데스크톱에서 실행할 때 구조화된 출력을 생성할 수 있습니다. [자동화 워크플로우를 위한 사용 사례 및 아이디어](/docs/guides/recipes/session-recipes#structured-output-for-automation)를 참조하세요.

#### Response 스키마

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `json_schema` | Object | ✅ | 출력 검증을 위한 [JSON 스키마](https://json-schema.org/) |

#### 기본 구조

```yaml
response:
  json_schema:
    type: object
    properties:
      # 여기에 타입과 설명과 함께 필드를 정의합니다
    required:
      # 필수 필드 이름 나열
```

#### 간단한 예시

```yaml
version: "1.0.0"
title: "Task Summary"
description: "Summarize completed tasks"
prompt: "Summarize the tasks you completed"
response:
  json_schema:
    type: object
    properties:
      summary:
        type: string
        description: "Brief summary of work done"
      tasks_completed:
        type: number
        description: "Number of tasks finished"
      next_steps:
        type: array
        items:
          type: string
        description: "Recommended next actions"
    required:
      - summary
      - tasks_completed
```

### Retry

`retry` 필드를 사용하면 성공 기준이 충족되지 않는 경우 레시피가 자동으로 실행을 재시도할 수 있습니다. 이는 목표를 달성하기 위해 여러 번 시도가 필요할 수 있는 레시피나 자동화된 검증 및 복구 워크플로우를 구현하는 데 유용합니다.

#### Retry 스키마

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `max_retries` | Number | ✅ | 최대 재시도 횟수 |
| `checks` | Array | ✅ | 성공 검사 구성 목록 |
| `timeout_seconds` | Number | - | 성공 검사 명령의 타임아웃 (기본값: 300초) |
| `on_failure_timeout_seconds` | Number | - | on_failure 명령의 타임아웃 (기본값: 600초) |
| `on_failure` | String | - | 재시도 시도가 실패할 때 실행할 셸 명령 |

#### 성공 검사 구성

`checks` 배열의 각 성공 검사에는 다음 스키마가 있습니다:

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `type` | String | ✅ | 검사 유형 - 현재 "shell"만 지원됨 |
| `command` | String | ✅ | 검증을 위해 실행할 셸 명령 (성공 시 코드 0으로 종료해야 함) |

#### 재시도 로직 작동 방식

1. **레시피 실행**: 제공된 지침으로 레시피가 정상적으로 실행됩니다
2. **성공 검증**: 완료 후 모든 성공 검사가 순서대로 실행됩니다
3. **재시도 결정**: 성공 검사가 실패하고 재시도 횟수가 남아 있으면:
   - on_failure 명령 실행 (구성된 경우)
   - 에이전트의 메시지 기록을 초기 상태로 재설정
   - 재시도 카운터 증가 및 실행 재시작
4. **완료**: 다음 중 하나가 발생하면 프로세스가 중지됩니다:
   - 모든 성공 검사 통과 (성공)
   - 최대 재시도 횟수 도달 (실패)

#### 기본 재시도 예시

```yaml
version: "1.0.0"
title: "Counter Increment Task"
description: "Increment a counter until it reaches target value"
prompt: "Increment the counter value in /tmp/counter.txt by 1."

retry:
  max_retries: 5
  timeout_seconds: 10
  checks:
    - type: shell
      command: "test $(cat /tmp/counter.txt 2>/dev/null || echo 0) -ge 3"
  on_failure: "echo 'Counter is at:' $(cat /tmp/counter.txt 2>/dev/null || echo 0) '(need 3 to succeed)'"
```

#### 고급 재시도 예시

```yaml
version: "1.0.0"
title: "Service Health Check"
description: "Start service and verify it's running properly"
prompt: "Start the web service and verify it responds to health checks"

retry:
  max_retries: 3
  timeout_seconds: 30
  on_failure_timeout_seconds: 60
  checks:
    - type: shell
      command: "curl -f http://localhost:8080/health"
    - type: shell
      command: "pgrep -f 'web-service' > /dev/null"
  on_failure: "systemctl stop web-service || killall web-service"
```

#### 환경 변수

환경 변수를 사용하여 재시도 동작을 전역으로 구성할 수 있습니다:

- `GOOSE_RECIPE_RETRY_TIMEOUT_SECONDS`: 성공 검사 명령의 전역 타임아웃
- `GOOSE_RECIPE_ON_FAILURE_TIMEOUT_SECONDS`: on_failure 명령의 전역 타임아웃

이러한 환경 변수는 레시피별 타임아웃 구성에 의해 재정의됩니다.

### Settings

`settings` 필드를 사용하면 레시피에 대한 AI 모델 및 프로바이더 설정을 구성할 수 있습니다. 이는 레시피가 실행될 때 기본 구성을 재정의합니다.

#### Settings 스키마

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `goose_provider` | String | - | 사용할 AI 프로바이더 (예: "anthropic", "openai") |
| `goose_model` | String | - | 사용할 특정 모델 이름 |
| `temperature` | Number | - | 모델의 온도 설정 (일반적으로 0.0-1.0) |

#### 설정 구성 예시

```yaml
settings:
  goose_provider: "anthropic"
  goose_model: "claude-sonnet-4-20250514"
  temperature: 0.7
```

```yaml
settings:
  goose_provider: "openai"
  goose_model: "gpt-4o"
  temperature: 0.3
```

:::note
레시피에 지정된 설정은 해당 레시피가 실행될 때 기본 goose 구성을 재정의합니다. 설정이 지정되지 않으면 goose는 구성된 기본값을 사용합니다.
:::

### Subrecipes

`sub_recipes` 필드는 메인 레시피가 특정 작업을 수행하기 위해 호출하는 [서브레시피](/docs/guides/recipes/subrecipes)를 지정합니다. `sub_recipes` 배열의 각 서브레시피에는 다음 스키마가 있습니다:

#### 서브레시피 스키마

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `name` | String | ✅ | 서브레시피의 고유 식별자 |
| `path` | String | ✅ | 서브레시피 파일의 상대 또는 절대 경로 |
| `values` | Object | - | 서브레시피에 전달되는 미리 구성된 매개변수 값 |
| `sequential_when_repeated` | Boolean | - | 여러 서브레시피 인스턴스의 순차 실행을 강제합니다. 자세한 내용은 [서브레시피 병렬 실행](/docs/tutorials/subrecipes-in-parallel)을 참조하세요 |
| `description` | String | - | 서브레시피의 선택적 설명 |

#### 서브레시피 구성 예시

```yaml
sub_recipes:
  - name: "security_scan"
    path: "./subrecipes/security-analysis.yaml"
    values:  # 키-값 형식: {parameter_name}: {parameter_value}
      scan_level: "comprehensive"
      include_dependencies: "true"

  - name: "quality_check"
    path: "./subrecipes/quality-analysis.yaml"
    description: "Performs code quality analysis"
```

## 데스크톱 메타데이터 필드

goose 데스크톱에서 저장된 레시피에는 추가 메타데이터 필드가 포함됩니다. 이러한 필드는 데스크톱 앱에서 조직 및 관리에 사용되지만 CLI 작업에서는 무시됩니다.

| 필드 | 타입 | 필수 | 설명 |
|-------|------|----------|-------------|
| `recipe` | Object | ✅ |  모든 레시피 필드(`title`, `description`, `instructions` 등)를 포함합니다 |
| `name` | String | ✅ |  레시피 라이브러리에서 사용되는 표시 이름 |
| `isGlobal` | Boolean | ✅ |  레시피가 전역적으로 또는 프로젝트에 로컬로 사용 가능한지 여부 |
| `lastModified` | String | ✅ |  레시피가 마지막으로 수정된 ISO 타임스탬프 |
| `isArchived` | Boolean | ✅ |  레시피가 데스크톱 인터페이스에서 보관되었는지 여부 |

<details>
<summary>CLI 및 데스크톱 형식 예시</summary>

**CLI 형식**

<Tabs groupId="format">
  <TabItem value="yaml" label="YAML" default>

```yaml
version: "1.0.0"
title: "Code Review Assistant"
description: "Automated code review with best practices"
instructions: "You are a code reviewer..."
prompt: "Review the code in this repository"
extensions: []
```

  </TabItem>
  <TabItem value="json" label="JSON">

```json
{
  "version": "1.0.0",
  "title": "Code Review Assistant",
  "description": "Automated code review with best practices",
  "instructions": "You are a code reviewer...",
  "prompt": "Review the code in this repository",
  "extensions": []
}
```

  </TabItem>
</Tabs>

**데스크톱 형식**

<Tabs groupId="format">
  <TabItem value="yaml" label="YAML" default>

```yaml
name: "Code Review Assistant"
recipe:
  version: "1.0.0"
  title: "Code Review Assistant"
  description: "Automated code review with best practices"
  instructions: "You are a code reviewer..."
  prompt: "Review the code in this repository"
  extensions: []
isGlobal: true
lastModified: 2025-07-02T03:46:46.778Z
isArchived: false
```

  </TabItem>
  <TabItem value="json" label="JSON">

```json
{
  "name": "Code Review Assistant",
  "recipe": {
    "version": "1.0.0",
    "title": "Code Review Assistant",
    "description": "Automated code review with best practices",
    "instructions": "You are a code reviewer...",
    "prompt": "Review the code in this repository",
    "extensions": []
  },
  "isGlobal": true,
  "lastModified": "2025-07-02T03:46:46.778Z",
  "isArchived": false
}
```

  </TabItem>
</Tabs>

</details>

## 템플릿 지원

레시피는 매개변수 대체를 위해 `instructions`, `prompt` 및 `activities` 필드에서 Jinja 스타일 템플릿 구문을 지원합니다:

```yaml
instructions: "Follow these steps with {{ parameter_name }}"
prompt: "Your task is to {{ action }}"
activities:
  - "Process {{ parameter_name }} with {{ action }}"
```

고급 템플릿 기능은 다음을 포함합니다:
- `{% extends "parent.yaml" %}`를 사용한 [템플릿 상속](#템플릿-상속)
- 정의하고 재정의할 수 있는 블록:
  ```yaml
  {% block content %}
  Default content
  {% endblock %}
  ```
- [`indent()` 템플릿 필터](#멀티라인-값을-위한-indent-필터)

### 템플릿 상속

템플릿 상속에 `{% extends "parent.yaml" %}`를 사용합니다:

**부모 레시피 (`parent.yaml`):**
```yaml
version: "1.0.0"
title: "Parent Recipe"
description: "Base recipe template"
prompt: |
  {% block prompt %}
  Default prompt text
  {% endblock %}
```

**자식 레시피:**
```yaml
{% extends "parent.yaml" %}
{% block prompt %}
Modified prompt text
{% endblock %}
```

### 멀티라인 값을 위한 indent() 필터

`indent()` 필터를 사용하면 멀티라인 매개변수 값이 올바르게 들여쓰기되어 유효한 JSON 또는 YAML 형식으로 해석될 수 있습니다. 이 예시는 서브레시피에 데이터를 전달할 때 두 칸 들여쓰기를 지정하기 위해 `{{ raw_data | indent(2) }}`를 사용합니다:

```yaml
sub_recipes:
  - name: "analyze"
    path: "./analyze.yaml"
    values:
      content: |
        {{ raw_data | indent(2) }}
```

### 내장 매개변수

내장 템플릿 매개변수는 자동으로 지원되며 `parameters` 배열에 정의할 필요가 없습니다.

| 매개변수 | 설명 |
|-----------|-------------|
| `recipe_dir` | 레시피 파일이 포함된 디렉토리로 자동 설정됩니다. 동반 파일을 참조하는 데 사용합니다. 예: `{{ recipe_dir }}/style-guide.md` |

## 검증 규칙

[`validate_recipe.rs`](https://github.com/block/goose/blob/main/crates/goose/src/recipe/validate_recipe.rs)의 검증 규칙은 레시피를 로드할 때 적용되고 [`goose recipe validate`](/docs/guides/goose-cli-commands#recipe) 하위 명령에서 사용됩니다:

### 레시피 수준 검증

- `validate_prompt_or_instructions` - `instructions` 또는 `prompt` 중 하나 이상이 있어야 합니다
- `validate_json_schema` - `response.json_schema`가 지정된 경우 JSON 응답 스키마가 유효해야 합니다

### 매개변수 검증

- `validate_parameters_in_template` - 모든 템플릿 변수에 해당하는 매개변수 정의가 있어야 하고, 정의된 모든 매개변수가 사용되어야 합니다 (사용되지 않는 매개변수 없음)
- `validate_optional_parameters` - 선택적 매개변수에는 기본값이 있어야 합니다
- `validate_optional_parameters` - 파일 매개변수는 민감한 파일 가져오기를 방지하기 위해 기본값을 가질 수 없습니다

:::info
기본 필드 요구 사항(필수 필드, 유형, 문자 제한)은 [핵심 레시피 스키마](#핵심-레시피-스키마) 테이블에 문서화되어 있습니다.
:::

## 완전한 레시피 예시

<Tabs groupId="format">
  <TabItem value="yaml" label="YAML" default>

```yaml
version: "1.0.0"
title: "Example Recipe"
description: "A sample recipe demonstrating the format"
instructions: "Process {{ file_count }} files using {{ required_param }} and output in {{ output_format }} format. Configuration: {{ config_file }}"
prompt: "Start processing with the provided parameters"
parameters:
  - key: required_param
    input_type: string
    requirement: required
    description: "A required text parameter"

  - key: file_count
    input_type: number
    requirement: optional
    default: 10
    description: "Maximum number of files to process"

  - key: output_format
    input_type: select
    requirement: required
    description: "Choose the output format"
    options:
      - json
      - markdown
      - csv

  - key: config_file
    input_type: file
    requirement: required
    description: "Path to configuration file"

extensions:
  - type: stdio
    name: codesearch
    cmd: uvx
    args:
      - mcp_codesearch@latest
    timeout: 300
    bundled: true
    description: "Query codesearch directly from goose"

settings:
  goose_provider: "anthropic"
  goose_model: "claude-sonnet-4-20250514"
  temperature: 0.7

retry:
  max_retries: 3
  timeout_seconds: 30
  checks:
    - type: shell
      command: "echo 'Task validation check passed'"
  on_failure: "echo 'Retry attempt failed, cleaning up...'"

response:
  json_schema:
    type: object
    properties:
      result:
        type: string
        description: "The main result of the task"
      details:
        type: array
        items:
          type: string
        description: "Additional details of steps taken"
    required:
      - result
      - details
```

  </TabItem>
  <TabItem value="json" label="JSON">

```json
{
  "version": "1.0.0",
  "title": "Example Recipe",
  "description": "A sample recipe demonstrating the format",
  "instructions": "Process {{ file_count }} files using {{ required_param }} and output in {{ output_format }} format. Configuration: {{ config_file }}",
  "prompt": "Start processing with the provided parameters",
  "parameters": [
    {
      "key": "required_param",
      "input_type": "string",
      "requirement": "required",
      "description": "A required text parameter"
    },
    {
      "key": "file_count",
      "input_type": "number",
      "requirement": "optional",
      "default": "10",
      "description": "Maximum number of files to process"
    },
    {
      "key": "output_format",
      "input_type": "select",
      "requirement": "required",
      "description": "Choose the output format",
      "options": ["json", "markdown", "csv"]
    },
    {
      "key": "config_file",
      "input_type": "file",
      "requirement": "required",
      "description": "Path to configuration file"
    }
  ],
  "extensions": [
    {
      "type": "stdio",
      "name": "codesearch",
      "cmd": "uvx",
      "args": ["mcp_codesearch@latest"],
      "timeout": 300,
      "bundled": true,
      "description": "Query codesearch directly from goose"
    }
  ],
  "settings": {
    "goose_provider": "anthropic",
    "goose_model": "claude-sonnet-4-20250514",
    "temperature": 0.7
  },
  "retry": {
    "max_retries": 3,
    "timeout_seconds": 30,
    "checks": [
      {
        "type": "shell",
        "command": "echo 'Task validation check passed'"
      }
    ],
    "on_failure": "echo 'Retry attempt failed, cleaning up...'"
  },
  "response": {
    "json_schema": {
      "type": "object",
      "properties": {
        "result": {
          "type": "string",
          "description": "The main result of the task"
        },
        "details": {
          "type": "array",
          "items": {
            "type": "string"
          },
          "description": "Additional details of steps taken"
        }
      },
      "required": ["result", "details"]
    }
  }
}
```

  </TabItem>
</Tabs>

## 오류 처리

주의해야 할 일반적인 오류:

- 누락된 필수 매개변수
- 기본값이 없는 선택적 매개변수
- 매개변수 정의가 없는 템플릿 변수
- 잘못된 YAML/JSON 구문
- 누락된 필수 필드
- 잘못된 확장 기능 구성
- 잘못된 재시도 구성 (필수 필드 누락, 잘못된 셸 명령)

이러한 오류가 발생하면 goose는 수정해야 할 사항을 나타내는 유용한 오류 메시지를 제공합니다.

### 재시도 관련 오류

- **잘못된 성공 검사**: 실행할 수 없거나 구문 오류가 있는 셸 명령
- **타임아웃 오류**: 타임아웃 제한을 초과하는 성공 검사 또는 on_failure 명령
- **최대 재시도 초과**: 성공 없이 모든 재시도 횟수가 소진된 경우
- **누락된 필수 재시도 필드**: `max_retries` 또는 `checks`가 지정되지 않은 경우

## 더 알아보기
goose 레시피를 마스터하는 데 도움이 되는 더 많은 문서, 도구 및 리소스는 [레시피](/docs/guides/recipes) 가이드를 확인하세요.
