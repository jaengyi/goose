---
sidebar_position: 2
title: LLM 공급자 구성
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft } from 'lucide-react';
import { ModelSelectionTip } from '@site/src/components/ModelSelectionTip';

# 지원되는 LLM 공급자

goose는 다양한 LLM 공급자와 호환되어 원하는 모델을 선택하고 통합할 수 있습니다.

:::tip 모델 선택
<ModelSelectionTip/>
[Berkeley Function-Calling Leaderboard][function-calling-leaderboard]가 모델 선택에 좋은 가이드가 될 수 있습니다.
:::

## 사용 가능한 공급자

| 공급자                                                                    | 설명                                                                                                                                                                                                               | 파라미터                                                                                                                                                                          |
|-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Amazon Bedrock](https://aws.amazon.com/bedrock/)                           | Claude, Jurassic-2 등 다양한 기반 모델을 제공합니다. **AWS 환경 변수는 `goose configure`가 아닌 사전에 설정해야 합니다**                                           | `AWS_PROFILE` 또는 `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`                                                                                                 |
| [Amazon SageMaker TGI](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html) | Amazon SageMaker 엔드포인트를 통해 Text Generation Inference 모델을 실행합니다. **AWS 자격 증명을 사전에 구성해야 합니다.** | `SAGEMAKER_ENDPOINT_NAME`, `AWS_REGION` (선택), `AWS_PROFILE` (선택)  |
| [Anthropic](https://www.anthropic.com/)                                     | 자연어 작업을 위한 고급 AI 모델인 Claude를 제공합니다.                                                                                                                           | `ANTHROPIC_API_KEY`, `ANTHROPIC_HOST` (선택)                                                                                                                                                                 |
| [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/) | GPT-4 및 GPT-3.5를 포함한 Azure 호스팅 OpenAI 모델에 액세스합니다. API 키 및 Azure 자격 증명 체인 인증을 모두 지원합니다.                                                                                          | `AZURE_OPENAI_ENDPOINT`, `AZURE_OPENAI_DEPLOYMENT_NAME`, `AZURE_OPENAI_API_KEY` (선택)                                                                                           |
| [Databricks](https://www.databricks.com/)                                   | 모델 구축 및 배포를 위한 통합 데이터 분석 및 AI 플랫폼입니다.                                                                                                                                                 | `DATABRICKS_HOST`, `DATABRICKS_TOKEN` |
| [Docker Model Runner](https://docs.docker.com/ai/model-runner/)                             | Docker Desktop 또는 Docker CE에서 OpenAI 호환 API 엔드포인트로 실행되는 로컬 모델입니다. **이 공급자는 로컬에서 실행되므로 먼저 [모델을 다운로드](#로컬-llm)해야 합니다.**                     | `OPENAI_HOST`, `OPENAI_BASE_PATH`   |
| [Gemini](https://ai.google.dev/gemini-api/docs)                             | 멀티모달 기능(텍스트, 이미지)을 갖춘 Google의 고급 LLM입니다.                                                                                                                                                      | `GOOGLE_API_KEY`                                                                                                                                                                    |
| [GCP Vertex AI](https://cloud.google.com/vertex-ai)                         | Gemini 및 Claude 모델을 지원하는 Google Cloud의 Vertex AI 플랫폼입니다. **자격 증명을 [사전에 구성](https://cloud.google.com/vertex-ai/docs/authentication)해야 합니다.**                 | `GCP_PROJECT_ID`, `GCP_LOCATION` 및 선택적으로 `GCP_MAX_RATE_LIMIT_RETRIES` (5), `GCP_MAX_OVERLOADED_RETRIES` (5), `GCP_INITIAL_RETRY_INTERVAL_MS` (5000), `GCP_BACKOFF_MULTIPLIER` (2.0), `GCP_MAX_RETRY_INTERVAL_MS` (320_000). |
| [GitHub Copilot](https://docs.github.com/en/copilot/using-github-copilot/ai-models) | GitHub의 Copilot 인프라를 통해 OpenAI, Anthropic, Google 등의 AI 모델에 액세스합니다. **Copilot 액세스가 있는 GitHub 계정이 필요합니다.** | 수동 키 불필요. CLI와 데스크톱 모두에서 [디바이스 플로우 인증](#github-copilot-인증)을 사용합니다. |
| [Groq](https://groq.com/)                                                   | LLM을 위한 고성능 추론 하드웨어 및 도구입니다.                                                                                                                                                                   | `GROQ_API_KEY`                                                                                                                                                                      |
| [LiteLLM](https://docs.litellm.ai/docs/) | 자동 프롬프트 캐싱 및 통합 API 액세스로 여러 모델을 지원하는 LiteLLM 프록시입니다. | `LITELLM_HOST`, `LITELLM_BASE_PATH` (선택), `LITELLM_API_KEY` (선택), `LITELLM_CUSTOM_HEADERS` (선택), `LITELLM_TIMEOUT` (선택) |
| [Mistral AI](https://mistral.ai/)                                           | 범용 모델, 특수 코딩 모델(Codestral), 멀티모달 모델(Pixtral)을 포함한 Mistral 모델에 대한 액세스를 제공합니다.                                                                   | `MISTRAL_API_KEY`                                                                                                 |
| [Ollama](https://ollama.com/)                                               | Qwen, Llama, DeepSeek 및 기타 오픈 소스 모델을 지원하는 로컬 모델 러너입니다. **이 공급자는 로컬에서 실행되므로 먼저 [모델을 다운로드하고 실행](#로컬-llm)해야 합니다.**  | `OLLAMA_HOST`                                                                                                                                                                       |
| [Ramalama](https://ramalama.ai/)                                            | 네이티브 [OCI](https://opencontainers.org/) 컨테이너 런타임, [CNCF](https://www.cncf.io/) 도구를 사용하고 OCI 아티팩트로 모델을 지원하는 로컬 모델입니다. Ramalama API는 Ollama와 호환되는 대안이며 goose Ollama 공급자와 함께 사용할 수 있습니다. Qwen, Llama, DeepSeek 및 기타 오픈 소스 모델을 지원합니다. **이 공급자는 로컬에서 실행되므로 먼저 [모델을 다운로드하고 실행](#로컬-llm)해야 합니다.**  | `OLLAMA_HOST`                                                                                                                                                                       |
| [OpenAI](https://platform.openai.com/api-keys)                              | gpt-4o, o1 및 기타 고급 언어 모델을 제공합니다. OpenAI 호환 엔드포인트(예: 자체 호스팅 LLaMA, vLLM, KServe)도 지원합니다. **goose가 도구 호출을 사용하기 때문에 o1-mini 및 o1-preview는 지원되지 않습니다.** | `OPENAI_API_KEY`, `OPENAI_HOST` (선택), `OPENAI_ORGANIZATION` (선택), `OPENAI_PROJECT` (선택), `OPENAI_CUSTOM_HEADERS` (선택)                                       |
| [OpenRouter](https://openrouter.ai/)                                        | 속도 제한 관리와 같은 기능으로 다양한 모델에 대한 통합 액세스를 제공하는 API 게이트웨이입니다.                                                                                                                             | `OPENROUTER_API_KEY`                                                                                                                                                                |
| [Snowflake](https://docs.snowflake.com/user-guide/snowflake-cortex/aisql#choosing-a-model) | Claude 모델을 포함한 최신 모델에 Snowflake Cortex 서비스를 사용하여 액세스합니다. **Snowflake 계정과 프로그래밍 방식 액세스 토큰(PAT)이 필요합니다**.                                                     | `SNOWFLAKE_HOST`, `SNOWFLAKE_TOKEN`                                                                                                                                                                 |
| [Tetrate Agent Router Service](https://router.tetrate.ai)                   | Claude, Gemini, GPT, 오픈 가중치 모델 등을 포함한 AI 모델을 위한 통합 API 게이트웨이입니다. 안전한 API 키 생성을 위한 PKCE 인증 플로우를 지원합니다.                                                                                | `TETRATE_API_KEY`, `TETRATE_HOST` (선택)                                                                                                                                        |
| [Venice AI](https://venice.ai/home)                                         | 사용자 개인 정보 보호를 우선시하면서 Llama, Mistral, Qwen과 같은 오픈 소스 모델에 대한 액세스를 제공합니다. **계정과 [API 키](https://docs.venice.ai/overview/guides/generating-api-key)가 필요합니다**.                 | `VENICE_API_KEY`, `VENICE_HOST` (선택), `VENICE_BASE_PATH` (선택), `VENICE_MODELS_PATH` (선택)                                                                          |
| [xAI](https://x.ai/)                                                        | 131,072 토큰 컨텍스트 윈도우를 갖춘 grok-3, grok-3-mini, grok-3-fast를 포함한 xAI의 Grok 모델에 액세스합니다.                                                                                                            | `XAI_API_KEY`, `XAI_HOST` (선택)                                                                                                                                                |

:::tip Claude 모델용 프롬프트 캐싱
goose는 Anthropic, Databricks, OpenRouter 및 LiteLLM 공급자를 통해 Claude 모델을 사용할 때 Anthropic의 [프롬프트 캐싱](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)을 자동으로 활성화합니다. 이는 요청에 `cache_control` 마커를 추가하여 자주 사용되는 컨텍스트를 캐싱함으로써 긴 대화의 비용을 줄일 수 있습니다. 기술적 세부 사항은 [공급자 구현](https://github.com/block/goose/tree/main/crates/goose/src/providers)을 참조하세요.
:::

### CLI 공급자

goose는 기존 CLI 도구와 함께 작동하는 특별한 "패스스루" 공급자도 지원하여 토큰당 비용을 지불하는 대신 기존 구독을 사용할 수 있습니다:

| 공급자                                                                    | 설명                                                                                                                                                                                                               | 요구사항                                                                                                                                                                          |
|-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Claude Code](https://www.anthropic.com/claude-code) (`claude-code`)                       | Claude Code 구독과 함께 Anthropic의 Claude CLI 도구를 사용합니다. 200K 컨텍스트 제한의 Claude에 대한 액세스를 제공합니다.                                                                                      | Claude CLI 설치 및 인증, 활성 Claude Code 구독                                                                                                              |
| [OpenAI Codex](https://developers.openai.com/codex/cli) (`codex`)          | ChatGPT Plus/Pro 구독과 함께 OpenAI의 Codex CLI 도구를 사용합니다. 최대 400K 컨텍스트 제한의 GPT-5 모델에 대한 액세스를 제공합니다.                                                                         | Codex CLI 설치 및 인증, 활성 ChatGPT Plus/Pro 구독                                                                                                          |
| [Cursor Agent](https://docs.cursor.com/en/cli/overview) (`cursor-agent`)   | Cursor 구독과 함께 Cursor의 AI CLI 도구를 사용합니다. cursor-agent 명령줄 인터페이스를 통해 GPT-5, Claude 4 및 기타 모델에 대한 액세스를 제공합니다.                                              | cursor-agent CLI 설치 및 인증                                                                                                         |
| [Gemini CLI](https://ai.google.dev/gemini-api/docs) (`gemini-cli`)         | Google AI 구독과 함께 Google의 Gemini CLI 도구를 사용합니다. 1M 컨텍스트 제한의 Gemini에 대한 액세스를 제공합니다.                                                                                               | Gemini CLI 설치 및 인증                                                                                                                |

:::tip CLI 공급자
CLI 공급자는 기존 구독을 사용하는 비용 효율적인 대안입니다. CLI 명령을 실행하고 도구의 기본 기능과 통합되므로 API 공급자와 다르게 작동합니다. 자세한 설정 지침은 [CLI 공급자 가이드](/docs/guides/cli-providers)를 참조하세요.
:::

## 공급자 및 모델 구성

선택한 공급자를 구성하거나, 사용 가능한 옵션을 확인하거나, 모델을 선택하려면 goose 데스크톱의 `Models` 탭을 방문하거나 CLI에서 `goose configure`를 실행하세요.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  **처음 사용하는 경우:**

  goose를 처음 열면 환영 화면에서 세 가지 옵션이 있습니다:
  - **[Tetrate Agent Router](https://tetrate.io/products/tetrate-agent-router-service)로 자동 설정**
  - **[OpenRouter](https://openrouter.ai/)로 자동 설정**
  - **기타 공급자**
  <Tabs groupId="setup">
    <TabItem value="tetrate" label="Tetrate Agent Router" default>
    Tetrate Agent Router로 시작하는 것을 권장합니다. Tetrate는 내장된 속도 제한 및 자동 장애 조치 기능과 함께 여러 AI 모델에 대한 액세스를 제공합니다.

    :::info 무료 크레딧 제공
    goose를 통해 Tetrate로 처음 자동 인증하면 $10의 무료 크레딧을 받게 됩니다. 이 혜택은 신규 및 기존 Tetrate 사용자 모두에게 제공됩니다.
    :::
    1. `Tetrate Agent Router로 자동 설정`을 선택하세요.
    2. goose가 브라우저 창을 열어 Tetrate로 인증하거나, 계정이 없는 경우 새 계정을 만들 수 있습니다.
    3. goose 데스크톱 앱으로 돌아오면 첫 번째 세션을 시작할 준비가 된 것입니다.
    </TabItem>

    <TabItem value="openrouter" label="OpenRouter">
    1. `OpenRouter로 자동 설정`을 선택하세요.
    2. goose가 브라우저 창을 열어 OpenRouter로 인증하거나, 계정이 없는 경우 새 계정을 만들 수 있습니다.
    3. goose 데스크톱 앱으로 돌아오면 첫 번째 세션을 시작할 준비가 된 것입니다.
    </TabItem>

    <TabItem value="others" label="기타 공급자">
    1. goose와 함께 사용하려는 특정 공급자와 해당 공급자의 API 키가 있는 경우 `기타 공급자`를 선택하세요.
    2. 원하는 공급자를 찾아 `Configure` 버튼을 클릭하세요. 목록에 공급자가 보이지 않으면 창 하단의 `Add Custom Provider`를 클릭하여 [사용자 정의 공급자를 구성](#사용자-정의-공급자-구성)하세요.
    3. 공급자에 따라 API 키, API 호스트 또는 기타 선택적 [파라미터](#사용-가능한-공급자)를 입력해야 합니다. `Submit` 버튼을 클릭하여 인증하고 첫 번째 세션을 시작하세요.

    :::info Ollama 모델 감지
    Ollama 사용자의 경우 로컬에 설치된 모든 모델이 모델 선택 드롭다운에 자동으로 표시됩니다.
    :::

    </TabItem>
  </Tabs>
  **LLM 공급자 및 API 키를 업데이트하려면:**
  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
  2. 사이드바에서 `Settings` 버튼을 클릭합니다
  3. `Models` 탭을 클릭합니다
  4. `Configure providers`를 클릭합니다
  5. 목록에서 공급자를 클릭합니다
  6. API 키 및 기타 필수 구성을 추가한 다음 `Submit`을 클릭합니다

  **현재 모델을 변경하려면:**
  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
  2. 사이드바에서 `Settings` 버튼을 클릭합니다
  3. `Models` 탭을 클릭합니다
  4. `Switch models`를 클릭합니다
  5. 드롭다운에서 구성된 공급자 중 선택하거나 `Use other provider`를 선택하여 새 공급자를 구성합니다
  6. 사용 가능한 옵션에서 모델을 선택하거나 `Use custom model`을 선택하여 특정 모델 이름을 입력합니다
  7. `Select model`을 클릭하여 선택을 확인합니다

  :::tip 단축키
  더 빠른 액세스를 위해 앱 하단의 현재 모델 이름을 클릭하고 `Change Model`을 선택하세요.
  :::

  **공급자 및 모델 구성을 처음부터 다시 시작하려면:**
  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
  2. 사이드바에서 `Settings` 버튼을 클릭합니다
  3. `Models` 탭을 클릭합니다
  4. `Reset Provider and Model`을 클릭하여 현재 설정을 지우고 환영 화면으로 돌아갑니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    1. 터미널에서 다음 명령어를 실행하세요:

       ```sh
       goose configure
       ```

    2. 메뉴에서 `Configure Providers`를 선택하고 `Enter`를 누르세요.

       ```
       ┌   goose-configure
       │
       ◆  What would you like to configure?
       // highlight-start
       │  ● Configure Providers (Change provider or update credentials)
       // highlight-end
       │  ○ Custom Providers
       │  ○ Add Extension
       │  ○ Toggle Extensions
       │  ○ Remove Extension
       │  ○ goose Settings
       └
       ```
    3. 모델 공급자를 선택하고 `Enter`를 누르세요. 화살표 키(↑/↓)를 사용하여 옵션을 이동합니다.

       ```
       ┌   goose-configure
       │
       ◇  What would you like to configure?
       │  Configure Providers
       │
       ◆  Which model provider should we use?
       │  ○ Amazon Bedrock
       │  ○ Amazon SageMaker TGI
       // highlight-start
       │  ● Anthropic (Claude and other models from Anthropic)
       // highlight-end
       │  ○ Azure OpenAI
       │  ○ Claude Code CLI
       │  ○ ...
       └
       ```
    4. 메시지가 표시되면 API 키(및 기타 구성 세부 정보)를 입력하세요.

       ```
       ┌   goose-configure
       │
       ◇  What would you like to configure?
       │  Configure Providers
       │
       ◇  Which model provider should we use?
       │  Anthropic
       │
       ◆  Provider Anthropic requires ANTHROPIC_API_KEY, please enter a value
       // highlight-start
       │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪
       // highlight-end
       └
       ```

       모델만 변경하는 경우 공급자 구성 업데이트 프롬프트를 건너뛰세요.

    5. 원하는 `ANTHROPIC_HOST`를 입력하거나 `Enter`를 눌러 기본값을 사용하세요.

       ```
       ◆  Provider Anthropic requires ANTHROPIC_HOST, please enter a value
       // highlight-start
       │  https://api.anthropic.com (default)
       // highlight-end
       ```
    6. 사용할 모델을 선택하세요. 공급자에 따라 다음을 수행할 수 있습니다:
       - 목록에서 모델 선택
       - 이름으로 모델 검색
       - 모델 이름 직접 입력

       ```
       │
       ◇  Model fetch complete
       │
       ◇  Select a model:
       // highlight-start
       │  claude-sonnet-4-5 (default)
       // highlight-end
       │
       ◒  Checking your configuration...
       └  Configuration saved successfully
       ```

       이 변경 사항은 다음 세션을 시작할 때 적용됩니다.

  :::note
  `goose configure`는 사용자 정의 모델 이름 입력을 지원하지 않습니다. 공급자 목록에 없는 모델을 사용하려면 goose 데스크톱을 사용하거나 [`config.yaml`](/docs/guides/config-files)의 `GOOSE_MODEL` 변수를 직접 편집하세요.
  :::

  :::tip
  [`run` 명령어](/docs/guides/goose-cli-commands#run-options)를 사용하여 개별 세션의 모델을 설정하세요:

  ```bash
  goose run --model claude-sonnet-4-0 -t "initial prompt"
  ```
  :::

  </TabItem>
</Tabs>

### 사용자 정의 OpenAI 엔드포인트 사용

내장 OpenAI 공급자는 OpenAI의 공식 API(`api.openai.com`) 또는 다음과 같은 OpenAI 호환 엔드포인트에 연결할 수 있습니다:
- vLLM 또는 KServe를 사용하는 자체 호스팅 LLM(예: LLaMA, Mistral)
- 개인 OpenAI 호환 API 서버
- 데이터 거버넌스 및 보안 준수가 필요한 기업 배포
- OpenAI API 프록시 또는 게이트웨이

:::tip 사용자 정의 공급자 옵션
여러 OpenAI 호환 엔드포인트에 연결해야 하나요? 더 쉬운 전환과 더 나은 구성, 사용자 정의 이름 지정 및 공유 가능한 구성을 위해 대신 [사용자 정의 공급자를 구성](#사용자-정의-공급자-구성)하세요.
:::

#### 구성 파라미터

| 파라미터 | 필수 | 설명 |
|-----------|----------|-------------|
| `OPENAI_API_KEY` | 예 | API 인증 키 |
| `OPENAI_HOST` | 아니오 | 사용자 정의 엔드포인트 URL(기본값: api.openai.com) |
| `OPENAI_ORGANIZATION` | 아니오 | 사용량 추적 및 거버넌스를 위한 조직 ID |
| `OPENAI_PROJECT` | 아니오 | 리소스 관리를 위한 프로젝트 식별자 |
| `OPENAI_CUSTOM_HEADERS` | 아니오 | 요청에 포함할 추가 헤더. 환경 변수, 구성 파일 또는 CLI를 통해 `HEADER_A=VALUE_A,HEADER_B=VALUE_B` 형식으로 설정할 수 있습니다. |

#### 구성 예시

<Tabs groupId="deployment">
  <TabItem value="vllm" label="vLLM 자체 호스팅" default>
    OpenAI 호환성으로 vLLM을 사용하여 LLaMA 또는 기타 모델을 실행하는 경우:
    ```sh
    OPENAI_HOST=https://your-vllm-endpoint.internal
    OPENAI_API_KEY=your-internal-api-key
    ```
  </TabItem>
  <TabItem value="kserve" label="KServe 배포">
    KServe를 사용하여 Kubernetes에 배포된 모델의 경우:
    ```sh
    OPENAI_HOST=https://kserve-gateway.your-cluster
    OPENAI_API_KEY=your-kserve-api-key
    OPENAI_ORGANIZATION=your-org-id
    OPENAI_PROJECT=ml-serving
    ```
  </TabItem>
  <TabItem value="enterprise" label="기업용 OpenAI">
    거버넌스가 있는 기업 OpenAI 배포의 경우:
    ```sh
    OPENAI_API_KEY=your-api-key
    OPENAI_ORGANIZATION=org-id123
    OPENAI_PROJECT=compliance-approved
    ```
  </TabItem>
  <TabItem value="custom-headers" label="사용자 정의 헤더">
    사용자 정의 헤더가 필요한 OpenAI 호환 엔드포인트의 경우:
    ```sh
    OPENAI_API_KEY=your-api-key
    OPENAI_ORGANIZATION=org-id123
    OPENAI_PROJECT=compliance-approved
    OPENAI_CUSTOM_HEADERS="X-Header-A=abc,X-Header-B=def"
    ```
  </TabItem>
</Tabs>

#### 설정 지침

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Settings` 버튼을 클릭합니다
    3. `Models` 탭을 클릭합니다
    4. `Configure providers`를 클릭합니다
    5. 공급자 목록에서 `OpenAI`를 클릭합니다
    6. 구성 세부 정보를 입력합니다:
       - API 키(필수)
       - 호스트 URL(사용자 정의 엔드포인트용)
       - 조직 ID(사용량 추적용)
       - 프로젝트(리소스 관리용)
    7. `Submit`을 클릭합니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    1. `goose configure`를 실행합니다
    2. `Configure Providers`를 선택합니다
    3. `OpenAI`를 공급자로 선택합니다
    4. 메시지가 표시되면 구성을 입력합니다:
       - API 키
       - 호스트 URL(사용자 정의 엔드포인트 사용 시)
       - 조직 ID(조직 추적 사용 시)
       - 프로젝트 식별자(프로젝트 관리 사용 시)
  </TabItem>
</Tabs>

:::tip 기업 배포
기업 배포의 경우 환경 변수 또는 구성 파일을 사용하여 이러한 값을 사전 구성하여 조직 전체에서 일관된 거버넌스를 보장할 수 있습니다.
:::

## 사용자 정의 공급자 구성

사용자 정의 공급자를 사용하면 [사용 가능한 공급자](#사용-가능한-공급자) 목록에 없는 서비스에 연결할 수 있습니다. goose의 공급자 목록에 표시되며 다른 공급자처럼 선택할 수 있습니다.

**장점:**
- **다중 엔드포인트**: 다른 서비스 간 전환(예: vLLM, 기업 프록시, OpenAI)
- **사전 구성된 모델**: 선호하는 모델 목록 저장
- **공유 가능한 구성**: JSON 파일을 팀 간에 공유하거나 저장소에 체크인 가능
- **사용자 정의 이름 지정**: UI에서 "OpenAI" 대신 "Corporate API" 표시
- **별도 자격 증명**: 각 공급자에 자체 API 키 할당

사용자 정의 공급자는 OpenAI, Anthropic 또는 Ollama 호환 API 형식을 사용해야 합니다. OpenAI 호환 공급자는 추가 인증, API 키, 토큰 또는 테넌트 식별자를 위한 사용자 정의 헤더를 포함할 수 있습니다. 각 사용자 정의 공급자는 JSON 구성 파일에 매핑됩니다.

**사용자 정의 공급자를 추가하려면:**
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Settings` 버튼을 클릭합니다
    3. `Models` 탭을 클릭합니다
    4. `Configure providers`를 클릭합니다
    5. 창 하단의 `Add Custom Provider`를 클릭합니다
    6. 공급자 세부 정보를 입력합니다:
       - **Provider Type**:
         - `OpenAI Compatible`(가장 일반적)
         - `Anthropic Compatible`
         - `Ollama Compatible`
       - **Display Name**: 공급자의 친숙한 이름
       - **API URL**: API 엔드포인트의 기본 URL
       - **API Key**: 사용자 정의 환경 변수를 사용하여 액세스하고 키체인(또는 키링이 비활성화된 경우 `secrets.yaml`)에 저장되는 API 키
         - `Ollama Compatible` 공급자의 경우 `This is a local model (no auth required)`를 클릭합니다
       - **Available Models**: 사용 가능한 모델 이름의 쉼표로 구분된 목록
       - **Streaming Support**: API가 스트리밍 응답을 지원하는지 여부(토글하여 클릭)
    7. `Create Provider`를 클릭합니다

    :::info 사용자 정의 헤더
    현재 OpenAI 호환 공급자의 사용자 정의 헤더는 goose 데스크톱에서 정의할 수 없습니다. 해결 방법으로 goose CLI를 사용하여 공급자를 구성하거나 공급자 구성 파일을 직접 편집하세요.
    :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    1. 터미널에서 다음 명령어를 실행하세요:

       ```sh
       goose configure
       ```

    2. `Custom Providers`를 선택합니다. 화살표 키(↑/↓)를 사용하여 옵션을 이동합니다.

       ```sh
       ┌   goose-configure
       │
       ◆  What would you like to configure?
       │  ○ Configure Providers
       // highlight-start
       │  ● Custom Providers (Add custom provider with compatible API)
       // highlight-end
       │  ○ Add Extension
       │  ○ Toggle Extensions
       │  ○ Remove Extension
       │  ○ goose Settings
       └
       ```

    3. `Add A Custom Provider`를 선택합니다

       ```sh
       ┌   goose-configure
       │
       ◇  What would you like to configure?
       │  Custom Providers
       │
       ◆  What would you like to do?
       // highlight-start
       │  ● Add A Custom Provider (Add a new OpenAI/Anthropic/Ollama compatible Provider)
       // highlight-end
       │  ○ Remove Custom Provider
       └
       ```

    4. 프롬프트를 따라 공급자 세부 정보를 입력합니다:
       - **API Type**:
         - `OpenAI Compatible`(가장 일반적)
         - `Anthropic Compatible`
         - `Ollama Compatible`
       - **Name**: 공급자의 친숙한 이름
       - **API URL**: API 엔드포인트의 기본 URL
       - **API Key**: 사용자 정의 환경 변수를 사용하여 액세스하고 키체인(또는 키링이 비활성화된 경우 `secrets.yaml`)에 저장되는 API 키
         - `Ollama Compatible` 공급자의 경우 `Enter`를 눌러 건너뛰기(또는 goose 데스크톱에서 공급자를 사용할 수 있도록 아무 값이나 입력)
       - **Available Models**: 사용 가능한 모델 이름의 쉼표로 구분된 목록
       - **Streaming Support**: API가 스트리밍 응답을 지원하는지 여부
       - **Custom Headers**: 필수 헤더 이름 및 값(`OpenAI Compatible` 공급자만 해당)

  </TabItem>
  <TabItem value="config" label="구성 파일">

    먼저 `custom_providers` 디렉토리에 JSON 파일을 만드세요:
    - macOS/Linux: `~/.config/goose/custom_providers/`
    - Windows: `%APPDATA%\Block\goose\config\custom_providers\`

    예시 `custom_corp_api.json` 구성 파일:
    ```json
    {
      "name": "custom_corp_api",
      "engine": "openai",
      "display_name": "Corporate API",
      "description": "Custom Corporate API provider",
      "api_key_env": "CUSTOM_CORP_API_API_KEY",
      "base_url": "https://api.company.com/v1/chat/completions",
      "models": [
        {
          "name": "gpt-4o",
          "context_limit": 128000
        },
        {
          "name": "gpt-3.5-turbo",
          "context_limit": 16385
        }
      ],
      "headers": {
        "x-origin-client-id": "YOUR_CLIENT_ID",
        "x-origin-secret": "YOUR_SECRET_VALUE"
      },
      "supports_streaming": true
    }
    ```

    그런 다음 `api_key_env`를 사용하여 세션의 키를 설정합니다. 예:
    ```bash
    export CUSTOM_CORP_API_API_KEY="your-api-key"
    goose session start --provider custom_corp_api
    ```

    :::tip 키체인 키 저장
    API 키를 `goose` 키체인에 저장하려면 goose 데스크톱에서 공급자를 업데이트하고 키를 입력하세요. 이렇게 하면 안전하고 영구적인 저장소가 제공되며 goose가 공급자에 기본적으로 연결할 수 있습니다.
    :::

  </TabItem>
</Tabs>

**사용자 정의 공급자를 업데이트하려면:**

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Settings` 버튼을 클릭합니다
    3. `Models` 탭을 클릭합니다
    4. `Configure providers`를 클릭합니다
    5. 목록에서 사용자 정의 공급자를 클릭합니다
    6. 변경하려는 필드를 업데이트합니다
       <br/>**중요:** 저장하기 전에 `Provider Type`이 올바른 값을 표시하는지 확인하세요. 그렇지 않으면 원래 설정과 관계없이 기본적으로 `OpenAI Compatible`로 설정될 수 있습니다.
    7. `Update Provider`를 클릭합니다

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    1. 터미널에서 다음 명령어를 실행하세요:

       ```sh
       goose configure
       ```

    2. 메뉴에서 `Configure Providers`를 선택하고 `Enter`를 누르세요.

       ```sh
       ┌   goose-configure
       │
       ◆  What would you like to configure?
       // highlight-start
       │  ● Configure Providers (Change provider or update credentials)
       // highlight-end
       │  ○ Custom Providers
       │  ○ Add Extension
       │  ○ Toggle Extensions
       │  ○ Remove Extension
       │  ○ goose Settings
       └
       ```

    3. 업데이트하려는 사용자 정의 공급자를 선택하고 `Enter`를 누르세요. 화살표 키(↑/↓)를 사용하여 옵션을 이동합니다.

       ```sh
       ┌   goose-configure
       │
       ◇  What would you like to configure?
       │  Configure Providers
       │
       ◆  Which model provider should we use?
       │  ○ Amazon Bedrock
       │  ○ Amazon SageMaker TGI
       │  ○ Anthropic
       │  ○ Azure OpenAI
       │  ○ Claude Code CLI
       // highlight-start
       │  ● Corporate API (Custom Corporate API provider)
       // highlight-end
       │  ○ Cursor Agent
       │  ○ ...
       └
       ```

    4. 프롬프트를 따라 필드를 업데이트합니다.

  </TabItem>
  <TabItem value="config" label="구성 파일">

    `custom_providers` 디렉토리에서 사용자 정의 공급자 구성 파일을 엽니다:
    - macOS/Linux: `~/.config/goose/custom_providers/`
    - Windows: `%APPDATA%\Block\goose\config\custom_providers\`

    변경하려는 필드를 업데이트하고 변경 사항을 저장합니다.
  </TabItem>
</Tabs>

변경 사항은 다음 goose 세션에서 사용할 수 있습니다.

**사용자 정의 공급자를 제거하려면:**

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    현재 goose 데스크톱을 사용하여 사용자 정의 공급자를 제거할 수 없습니다.
  </TabItem>
  <TabItem value="cli" label="goose CLI">

    1. 터미널에서 다음 명령어를 실행하세요:

       ```sh
       goose configure
       ```

    2. `Custom Providers`를 선택합니다. 화살표 키(↑/↓)를 사용하여 옵션을 이동합니다.

       ```sh
       ┌   goose-configure
       │
       ◆  What would you like to configure?
       │  ○ Configure Providers
       // highlight-start
       │  ● Custom Providers (Add custom provider with compatible API)
       // highlight-end
       │  ○ Add Extension
       │  ○ Toggle Extensions
       │  ○ Remove Extension
       │  ○ goose Settings
       └
       ```

    3. `Remove Custom Provider`를 선택합니다.

       ```sh
       ┌   goose-configure
       │
       ◇  What would you like to configure?
       │  Custom Providers
       │
       ◆  What would you like to do?
       │  ○ Add A Custom Provider
       // highlight-start
       │  ● Remove Custom Provider (Remove an existing custom provider)
       // highlight-end
       └
       ```

    4. 제거하려는 사용자 정의 공급자를 선택합니다.

    공급자 구성 파일이 `custom_providers` 디렉토리에서 제거되고 키가 키체인에서 제거됩니다.

  </TabItem>
  <TabItem value="config" label="구성 파일">

    :::tip
    공급자의 API 키가 키체인에 저장된 경우 goose CLI를 사용하여 사용자 정의 공급자를 제거하세요. 이렇게 하면 저장된 API 키도 제거됩니다.
    :::

    `custom_providers` 디렉토리에서 사용자 정의 공급자 구성 파일을 삭제하세요:
    - macOS/Linux: `~/.config/goose/custom_providers/`
    - Windows: `%APPDATA%\Block\goose\config\custom_providers\`

  </TabItem>
</Tabs>

## goose 무료로 사용하기

goose는 바로 사용할 수 있는 무료 오픈 소스 AI 에이전트이지만, 모든 지원되는 [LLM 공급자][providers]가 무료 티어를 제공하는 것은 아닙니다.

아래에서 몇 가지 무료 옵션과 시작 방법을 설명합니다.

:::warning 제한 사항
이러한 무료 옵션은 goose를 시작하고 기능을 탐색하는 좋은 방법입니다. 그러나 더 나은 성능을 위해 LLM을 업그레이드해야 할 수 있습니다.
:::


### Groq
Groq는 고속 추론으로 오픈 소스 모델에 대한 무료 액세스를 제공합니다. goose와 함께 Groq를 사용하려면 [Groq Console](https://console.groq.com/keys)에서 API 키가 필요합니다.

Groq는 도구 호출을 지원하는 여러 오픈 소스 모델을 제공합니다:
- **moonshotai/kimi-k2-instruct** - 1조 개의 파라미터를 가진 Mixture-of-Experts 모델로, 에이전트 지능 및 도구 사용에 최적화됨
- **qwen/qwen3-32b** - 고급 추론 및 다국어 기능을 갖춘 328억 개 파라미터 모델
- **gemma2-9b-it** - 명령어 튜닝이 적용된 Google의 Gemma 2 모델
- **llama-3.3-70b-versatile** - 다양한 애플리케이션을 위한 Meta의 Llama 3.3 모델

goose와 함께 Groq를 설정하려면 다음 단계를 따르세요:

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  **LLM 공급자 및 API 키를 업데이트하려면:**

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
    2. 사이드바에서 `Settings` 버튼을 클릭합니다.
    3. `Models` 탭을 클릭합니다.
    4. `Configure Providers`를 클릭합니다
    5. 목록에서 `Groq`를 공급자로 선택합니다.
    6. `Configure`를 클릭하고 API 키를 입력한 다음 `Submit`을 클릭합니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    1. 실행:
    ```sh
    goose configure
    ```
    2. 메뉴에서 `Configure Providers`를 선택합니다.
    3. 프롬프트를 따라 `Groq`를 공급자로 선택합니다.
    4. 메시지가 표시되면 API 키를 입력합니다.
    5. 원하는 Groq 모델을 입력합니다(예: `moonshotai/kimi-k2-instruct`).
  </TabItem>
</Tabs>

### Google Gemini
Google Gemini는 무료 티어를 제공합니다. goose와 함께 Gemini API를 사용하려면 [Google AI studio](https://aistudio.google.com/app/apikey)에서 API 키가 필요합니다.

goose와 함께 Google Gemini를 설정하려면 다음 단계를 따르세요:

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  **LLM 공급자 및 API 키를 업데이트하려면:**

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
    2. 사이드바에서 `Settings` 버튼을 클릭합니다.
    3. `Models` 탭을 클릭합니다.
    4. `Configure Providers`를 클릭합니다
    5. 목록에서 `Google Gemini`를 공급자로 선택합니다.
    6. `Configure`를 클릭하고 API 키를 입력한 다음 `Submit`을 클릭합니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    1. 실행:
    ```sh
    goose configure
    ```
    2. 메뉴에서 `Configure Providers`를 선택합니다.
    3. 프롬프트를 따라 `Google Gemini`를 공급자로 선택합니다.
    4. 메시지가 표시되면 API 키를 입력합니다.
    5. 원하는 Gemini 모델을 입력합니다.

    ```
    ┌   goose-configure
    │
    ◇ What would you like to configure?
    │ Configure Providers
    │
    ◇ Which model provider should we use?
    │ Google Gemini
    │
    ◇ Provider Google Gemini requires GOOGLE_API_KEY, please enter a value
    │▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪
    │
    ◇ Enter a model from that provider:
    │ gemini-2.0-flash-exp
    │
    ◇ Hello! You're all set and ready to go, feel free to ask me anything!
    │
    └ Configuration saved successfully
    ```
  </TabItem>
</Tabs>


### 로컬 LLM

goose는 로컬 AI 에이전트이며, 로컬 LLM을 사용하면 데이터를 비공개로 유지하고 환경을 완전히 제어하며 클라우드 액세스에 의존하지 않고 완전히 오프라인으로 작업할 수 있습니다. 그러나 로컬 LLM은 goose와 함께 사용하기 전에 약간의 추가 설정이 필요합니다.

:::warning 도구 호출이 없는 모델에 대한 제한된 지원
goose는 도구 호출을 광범위하게 사용하므로 도구 호출이 없는 모델은 채팅 완성만 수행할 수 있습니다. 도구 호출이 없는 모델을 사용하는 경우 모든 goose [확장 기능을 비활성화](/docs/getting-started/using-extensions#확장-기능-활성화비활성화)해야 합니다.
:::

다음은 지원하는 일부 로컬 공급자입니다:

<Tabs groupId="local-llms">
  <TabItem value="ollama" label="Ollama" default>
    <Tabs groupId="ollama-models">
      <TabItem value="ramalala" label="Ramalama">
        1. [Ramalama 다운로드](https://github.com/containers/ramalama?tab=readme-ov-file#install).
        2. 터미널에서 [도구 호출을 지원하는](https://ollama.com/search?c=tools) Ollama 모델 또는 [GGUF 형식 HuggingFace 모델](https://huggingface.co/search/full-text?q=%22tools+support%22+%2B+%22gguf%22&type=model)을 실행합니다:

          Ramalama가 goose Ollama 공급자와 함께 작동하려면 `--runtime-args="--jinja"` 플래그가 필요합니다.

          예:

          ```sh
          ramalama serve --runtime-args="--jinja" ollama://qwen2.5
          ```

          3. 별도의 터미널 창에서 goose로 구성합니다:

          ```sh
          goose configure
          ```

          4. `Configure Providers`를 선택합니다

          ```
          ┌   goose-configure
          │
          ◆  What would you like to configure?
          │  ● Configure Providers (Change provider or update credentials)
          │  ○ Toggle Extensions
          │  ○ Add Extension
          └
          ```

          5. Ramalama가 API 호환되고 goose Ollama 공급자를 사용할 수 있으므로 `Ollama`를 모델 공급자로 선택합니다

          ```
          ┌   goose-configure
          │
          ◇  What would you like to configure?
          │  Configure Providers
          │
          ◆  Which model provider should we use?
          │  ○ Anthropic
          │  ○ Databricks
          │  ○ Google Gemini
          │  ○ Groq
          │  ● Ollama (Local open source models)
          │  ○ OpenAI
          │  ○ OpenRouter
          └
          ```

          6. 모델이 실행 중인 호스트를 입력합니다

          :::info 엔드포인트
          Ollama 공급자의 경우 호스트를 제공하지 않으면 `localhost:11434`로 설정됩니다. URL을 구성할 때 스킴이 `http` 또는 `https`가 아니면 `http://`를 앞에 추가합니다. Ramalama의 기본 서비스 포트는 8080이므로 `OLLAMA_HOST=http://0.0.0.0:8080`으로 설정합니다
          :::

          ```
          ┌   goose-configure
          │
          ◇  What would you like to configure?
          │  Configure Providers
          │
          ◇  Which model provider should we use?
          │  Ollama
          │
          ◆  Provider Ollama requires OLLAMA_HOST, please enter a value
          │  http://0.0.0.0:8080
          └
          ```


          7. 실행 중인 모델을 입력합니다

          ```
          ┌   goose-configure
          │
          ◇  What would you like to configure?
          │  Configure Providers
          │
          ◇  Which model provider should we use?
          │  Ollama
          │
          ◇  Provider Ollama requires OLLAMA_HOST, please enter a value
          │  http://0.0.0.0:8080
          │
          ◇  Enter a model from that provider:
          │  qwen2.5
          │
          ◇  Welcome! You're all set to explore and utilize my capabilities. Let's get started on solving your problems together!
          │
          └  Configuration saved successfully
          ```

          :::tip 컨텍스트 길이
          goose가 확장 기능을 사용하는 데 문제가 있거나 [.goosehints](/docs/guides/context-engineering/using-goosehints)를 무시하는 경우 모델의 기본 컨텍스트 길이인 2048 토큰이 너무 낮을 가능성이 높습니다. `ramalama serve`를 사용하여 `--ctx-size, -c` 옵션을 [더 높은 값](https://github.com/containers/ramalama/blob/main/docs/ramalama-serve.1.md#--ctx-size--c)으로 설정하세요.
          :::

      </TabItem>
      <TabItem value="deepseek" label="DeepSeek-R1">
        네이티브 `DeepSeek-r1` 모델은 도구 호출을 지원하지 않지만, goose와 함께 사용할 수 있는 [사용자 정의 모델](https://ollama.com/michaelneale/deepseek-r1-goose)이 있습니다.

        :::warning
        이것은 70B 모델 크기이며 원활하게 실행하려면 강력한 장치가 필요합니다.
        :::


        1. [Ollama 다운로드](https://ollama.com/download).
        2. 터미널 창에서 다음 명령어를 실행하여 사용자 정의 DeepSeek-r1 모델을 설치합니다:

        ```sh
        ollama run michaelneale/deepseek-r1-goose
        ```

        3. 별도의 터미널 창에서 goose로 구성합니다:

        ```sh
        goose configure
        ```

        4. `Configure Providers`를 선택합니다

        ```
        ┌   goose-configure
        │
        ◆  What would you like to configure?
        │  ● Configure Providers (Change provider or update credentials)
        │  ○ Toggle Extensions
        │  ○ Add Extension
        └
        ```

        5. `Ollama`를 모델 공급자로 선택합니다

        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Configure Providers
        │
        ◆  Which model provider should we use?
        │  ○ Anthropic
        │  ○ Databricks
        │  ○ Google Gemini
        │  ○ Groq
        │  ● Ollama (Local open source models)
        │  ○ OpenAI
        │  ○ OpenRouter
        └
        ```

        6. 모델이 실행 중인 호스트를 입력합니다

        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Configure Providers
        │
        ◇  Which model provider should we use?
        │  Ollama
        │
        ◆  Provider Ollama requires OLLAMA_HOST, please enter a value
        │  http://localhost:11434
        └
        ```

        7. 위에서 설치한 모델을 입력합니다

        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Configure Providers
        │
        ◇  Which model provider should we use?
        │  Ollama
        │
        ◇   Provider Ollama requires OLLAMA_HOST, please enter a value
        │  http://localhost:11434
        │
        ◇  Enter a model from that provider:
        │  michaelneale/deepseek-r1-goose
        │
        ◇  Welcome! You're all set to explore and utilize my capabilities. Let's get started on solving your problems together!
        │
        └  Configuration saved successfully
        ```
      </TabItem>
      <TabItem value="others" label="기타 모델" default>
        1. [Ollama 다운로드](https://ollama.com/download).
        2. 터미널에서 [도구 호출을 지원하는](https://ollama.com/search?c=tools) 모델을 실행합니다

          예:

          ```sh
          ollama run qwen2.5
          ```

        3. 별도의 터미널 창에서 goose로 구성합니다:

          ```sh
          goose configure
          ```

        4. `Configure Providers`를 선택합니다

        ```
        ┌   goose-configure
        │
        ◆  What would you like to configure?
        │  ● Configure Providers (Change provider or update credentials)
        │  ○ Toggle Extensions
        │  ○ Add Extension
        └
        ```

        5. `Ollama`를 모델 공급자로 선택합니다

        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Configure Providers
        │
        ◆  Which model provider should we use?
        │  ○ Anthropic
        │  ○ Databricks
        │  ○ Google Gemini
        │  ○ Groq
        │  ● Ollama (Local open source models)
        │  ○ OpenAI
        │  ○ OpenRouter
        └
        ```

        6. 모델이 실행 중인 호스트를 입력합니다

        :::info 엔드포인트
        Ollama의 경우 호스트를 제공하지 않으면 `localhost:11434`로 설정됩니다.
        URL을 구성할 때 스킴이 `http` 또는 `https`가 아니면 `http://`를 앞에 추가합니다.
        다른 서버에서 Ollama를 실행하는 경우 `OLLAMA_HOST=http://{host}:{port}`를 설정해야 합니다.
        :::

        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Configure Providers
        │
        ◇  Which model provider should we use?
        │  Ollama
        │
        ◆  Provider Ollama requires OLLAMA_HOST, please enter a value
        │  http://localhost:11434
        └
        ```


        7. 실행 중인 모델을 입력합니다

        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Configure Providers
        │
        ◇  Which model provider should we use?
        │  Ollama
        │
        ◇  Provider Ollama requires OLLAMA_HOST, please enter a value
        │  http://localhost:11434
        │
        ◇  Enter a model from that provider:
        │  qwen2.5
        │
        ◇  Welcome! You're all set to explore and utilize my capabilities. Let's get started on solving your problems together!
        │
        └  Configuration saved successfully
        ```

        :::tip 컨텍스트 길이
        goose가 확장 기능을 사용하는 데 문제가 있거나 [.goosehints](/docs/guides/context-engineering/using-goosehints)를 무시하는 경우 모델의 기본 컨텍스트 길이인 4096 토큰이 너무 낮을 가능성이 높습니다. `OLLAMA_CONTEXT_LENGTH` 환경 변수를 [더 높은 값](https://github.com/ollama/ollama/blob/main/docs/faq.mdx#how-can-i-specify-the-context-window-size)으로 설정하세요.
        :::

      </TabItem>
    </Tabs>
  </TabItem>
  <TabItem value="docker" label="Docker Model Runner" default>
    1. [Docker 설치](https://docs.docker.com/get-started/get-docker/)
    2. [Docker Model Runner 활성화](https://docs.docker.com/ai/model-runner/#enable-dmr-in-docker-desktop)
    3. [모델 풀](https://docs.docker.com/ai/model-runner/#pull-a-model), 예를 들어 Docker Hub [AI 네임스페이스](https://hub.docker.com/u/ai), [Unsloth](https://hub.docker.com/u/unsloth) 또는 [HuggingFace에서](https://www.docker.com/blog/docker-model-runner-on-hugging-face/)

    예:

    ```sh
    docker model pull hf.co/unsloth/gemma-3n-e4b-it-gguf:q6_k
    ```

    4. OpenAI API 호환 엔드포인트를 사용하여 Docker Model Runner를 사용하도록 goose를 구성합니다:

    ```sh
    goose configure
    ```

    5. `Configure Providers`를 선택합니다

    ```
    ┌   goose-configure
    │
    ◆  What would you like to configure?
    │  ● Configure Providers (Change provider or update credentials)
    │  ○ Toggle Extensions
    │  ○ Add Extension
    └
    ```

    6. `OpenAI`를 모델 공급자로 선택합니다:

    ```
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Configure Providers
    │
    ◆  Which model provider should we use?
    │  ○ Anthropic
    │  ○ Amazon Bedrock
    │  ○ Claude Code
    │  ● OpenAI (GPT-4 and other OpenAI models, including OpenAI compatible ones)
    │  ○ OpenRouter
    ```

    7. Docker Model Runner 엔드포인트를 `OPENAI_HOST`로 구성합니다:

    ```
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Configure Providers
    │
    ◇  Which model provider should we use?
    │  OpenAI
    │
    ◆  Provider OpenAI requires OPENAI_HOST, please enter a value
    │  https://api.openai.com (default)
    └
    ```

    호스트 측 포트 Docker Model Runner의 기본값은 12434이므로 `OPENAI_HOST` 값은:
    `http://localhost:12434`가 될 수 있습니다.

    8. 베이스 경로를 구성합니다:

    ```
    ◆  Provider OpenAI requires OPENAI_BASE_PATH, please enter a value
    │  v1/chat/completions (default)
    └
    ```

    Docker model runner는 베이스 경로로 `/engines/llama.cpp/v1/chat/completions`를 사용합니다.

    9. 마지막으로 goose에서 사용할 Docker Model Runner에서 사용 가능한 모델을 구성합니다: `hf.co/unsloth/gemma-3n-e4b-it-gguf:q6_k`

    ```
    │
    ◇  Enter a model from that provider:
    │  gpt-4o
    │
    ◒  Checking your configuration...
    └  Configuration saved successfully
    ```
  </TabItem>
</Tabs>



## GitHub Copilot 인증

GitHub Copilot은 인증에 디바이스 플로우를 사용하므로 API 키가 필요하지 않습니다:

1. [`goose configure`](#공급자-및-모델-구성)를 실행하고 **GitHub Copilot**을 선택합니다
2. 8자 코드가 자동으로 클립보드에 복사됩니다
3. GitHub의 디바이스 활성화 페이지로 브라우저가 열립니다
4. 코드를 붙여넣어 애플리케이션을 인증합니다
5. goose로 돌아오면 CLI와 데스크톱 모두에서 GitHub Copilot을 공급자로 사용할 수 있습니다.

## Azure OpenAI 자격 증명 체인

goose는 Azure OpenAI에 대해 두 가지 인증 방법을 지원합니다:

1. **API 키 인증** - 직접 인증을 위해 `AZURE_OPENAI_API_KEY`를 사용
2. **Azure 자격 증명 체인** - API 키 없이 Azure CLI 자격 증명을 자동으로 사용

Azure 자격 증명 체인을 사용하려면:
- `az login`으로 로그인되어 있는지 확인
- Azure OpenAI 서비스에 대한 적절한 Azure 역할 할당이 있어야 함
- `goose configure`로 구성하고 Azure OpenAI를 선택한 후 API 키 필드를 비워 둡니다

이 방법은 기업 환경에서 인증을 간소화하고 보안을 강화합니다.

## 다중 모델 구성

단일 모델 설정 외에도 goose는 특수 작업에 다른 모델과 공급자를 사용할 수 있는 [다중 모델 구성](/docs/guides/multi-model/)을 지원합니다:

- **리드/워커 모델** - 초기 턴에 리드 모델과 실행 작업에 워커 모델 간의 자동 전환
- **계획 모드** - 실행 전에 상세한 프로젝트 분석을 만들기 위해 전용 모델을 사용하는 수동 계획 단계

---

특정 공급자에 대한 질문이 있거나 도움이 필요하면 [Discord](https://discord.gg/goose-oss) 또는 [goose 저장소](https://github.com/block/goose)에서 문의하세요.


[providers]: /docs/getting-started/providers
[function-calling-leaderboard]: https://gorilla.cs.berkeley.edu/leaderboard.html
