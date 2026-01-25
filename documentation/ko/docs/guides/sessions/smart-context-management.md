---
title: 스마트 컨텍스트 관리
sidebar_position: 3
sidebar_label: 스마트 컨텍스트 관리
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { ScrollText } from 'lucide-react';
import { PanelLeft } from 'lucide-react';

[대규모 언어 모델(LLM)](/docs/getting-started/providers)로 작업할 때 한 번에 처리할 수 있는 대화 기록의 양에 제한이 있습니다. goose는 컨텍스트 및 대화 제한을 처리하여 생산적인 세션을 유지하는 데 도움이 되는 스마트 컨텍스트 관리 기능을 제공합니다. 다음은 몇 가지 핵심 개념입니다:

- **컨텍스트 길이**: LLM이 고려할 수 있는 대화 기록의 양, 컨텍스트 윈도우라고도 함
- **컨텍스트 제한**: 모델이 처리할 수 있는 최대 토큰 수
- **컨텍스트 관리**: goose가 이러한 제한에 접근하는 대화를 처리하는 방법
- **턴**: goose와 LLM 간의 하나의 완전한 프롬프트-응답 상호작용

## goose의 컨텍스트 관리 방법
goose는 컨텍스트 관리를 위해 2단계 접근 방식을 사용합니다:

1. **자동 압축**: 토큰 제한에 가까워지면 사전에 대화를 요약
2. **컨텍스트 전략**: 자동 압축 후에도 컨텍스트 제한을 초과하는 경우 사용되는 백업 전략

이 계층화된 접근 방식을 통해 goose는 토큰 및 컨텍스트 제한을 우아하게 처리할 수 있습니다.

## 자동 압축
goose는 토큰 제한에 가까워지면 대화의 오래된 부분을 자동으로 압축(요약)하여 수동 개입 없이 장시간 세션을 유지할 수 있게 합니다.
자동 압축은 goose 데스크톱 및 goose CLI에서 토큰 제한의 80%에 도달하면 기본적으로 트리거됩니다.

`GOOSE_AUTO_COMPACT_THRESHOLD` [환경 변수](/docs/guides/environment-variables.md#session-management)로 자동 압축 동작을 제어합니다.
값을 `0.0`으로 설정하여 이 기능을 비활성화합니다.

```
# 사용 가능한 토큰의 60%가 사용되면 자동으로 세션 압축
export GOOSE_AUTO_COMPACT_THRESHOLD=0.6
```

자동 압축 임계값에 도달하면:
  1. goose가 공간을 만들기 위해 자동으로 대화 압축을 시작합니다.
  2. 완료되면 대화가 압축되고 요약되었다는 확인 메시지가 표시됩니다.
  3. 세션을 계속합니다. 이전 대화는 계속 표시되지만 goose의 활성 컨텍스트에는 압축된 변환만 포함됩니다.

### 수동 압축
컨텍스트 또는 토큰 제한에 도달하기 전에 압축을 수동으로 트리거할 수도 있습니다:

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 앱 하단의 모델 이름 옆에 있는 토큰 사용량 표시 점을 가리킵니다
  2. 나타나는 컨텍스트 창에서 <ScrollText className="inline" size={16} /> `Compact now`를 클릭합니다
  3. 완료되면 대화가 압축되고 요약되었다는 확인 메시지가 표시됩니다.
  4. 세션을 계속합니다. 이전 대화는 계속 표시되지만 goose의 활성 컨텍스트에는 압축된 변환만 포함됩니다.

  :::info
  `Compact now` 버튼이 활성화되려면 채팅에서 최소 하나의 메시지를 보내야 합니다.
  :::

</TabItem>
<TabItem value="cli" label="goose CLI" default>

컨텍스트 제한에 도달하기 전에 사전에 요약을 트리거하려면 `/summarize` 명령을 사용합니다:

```sh
( O)> /summarize
◇  Are you sure you want to summarize this conversation? This will condense the message history.
│  Yes
│
Summarizing conversation...
Conversation has been summarized.
Key information has been preserved while reducing context length.
```

</TabItem>
</Tabs>

## 컨텍스트 제한 전략

자동 압축이 비활성화되었거나 대화가 여전히 컨텍스트 제한을 초과하는 경우 goose는 다른 처리 방법을 제공합니다:

| 기능 | 설명 | 적합한 사용 | 가용성 | 영향 |
|---------|-------------|-----------|-----------|---------|
| **요약** | 핵심 포인트를 유지하면서 대화를 압축 | 길고 복잡한 대화 | 데스크톱 및 CLI | 대부분의 컨텍스트 유지 |
| **잘라내기** | 공간을 만들기 위해 가장 오래된 메시지 제거 | 단순하고 선형적인 대화 | CLI만 | 오래된 컨텍스트 손실 |
| **지우기** | 세션을 활성 상태로 유지하면서 새로 시작 | 대화의 새로운 방향 | CLI만 | 모든 컨텍스트 손실 |
| **프롬프트** | 위의 옵션 중 선택하도록 사용자에게 요청 | 대화형 세션에서 각 결정에 대한 제어 | CLI만 | 선택에 따라 다름 |

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

goose 데스크톱은 컨텍스트를 관리하기 위해 대화를 압축하여 요약만 사용하며, 크기를 줄이면서 핵심 정보를 보존합니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">

CLI는 모든 컨텍스트 제한 전략을 지원합니다: `summarize`, `truncate`, `clear`, `prompt`.

기본 동작은 실행 모드에 따라 다릅니다:
- **대화형 모드**: 사용자에게 선택하도록 프롬프트 (`prompt`와 동일)
- **헤드리스 모드** (`goose run`): 자동으로 요약 (`summarize`와 동일)

`GOOSE_CONTEXT_STRATEGY` 환경 변수를 설정하여 goose가 컨텍스트 제한을 처리하는 방법을 구성할 수 있습니다:

```bash
# 자동 전략 설정 (하나 선택)
export GOOSE_CONTEXT_STRATEGY=summarize  # 자동으로 요약 (권장)
export GOOSE_CONTEXT_STRATEGY=truncate   # 가장 오래된 메시지 자동 제거
export GOOSE_CONTEXT_STRATEGY=clear      # 세션 자동 지우기

# 사용자에게 프롬프트하도록 설정
export GOOSE_CONTEXT_STRATEGY=prompt
```

컨텍스트 제한에 도달하면 구성에 따라 동작이 달라집니다:

**기본 설정 (`GOOSE_CONTEXT_STRATEGY`가 설정되지 않음)**에서는 관리 옵션을 선택하라는 프롬프트가 표시됩니다:

```sh
◇  The model's context length is maxed out. You will need to reduce the # msgs. Do you want to?
│  ○ Clear Session
│  ○ Truncate Message
// highlight-start
│  ● Summarize Session
// highlight-end

final_summary: [대화 요약이 여기에 표시됩니다]

Context maxed out
--------------------------------------------------
goose summarized messages for you.
```

**`GOOSE_CONTEXT_STRATEGY`가 구성됨**에서 goose는 선택한 전략을 자동으로 적용합니다:

```sh
# GOOSE_CONTEXT_STRATEGY=summarize 예시
Context maxed out - automatically summarized messages.
--------------------------------------------------
goose automatically summarized messages for you.

# GOOSE_CONTEXT_STRATEGY=truncate 예시
Context maxed out - automatically truncated messages.
--------------------------------------------------
goose tried its best to truncate messages for you.

# GOOSE_CONTEXT_STRATEGY=clear 예시
Context maxed out - automatically cleared session.
--------------------------------------------------
```
  </TabItem>
</Tabs>

## 최대 턴 수
`Max Turns` 제한은 사용자 입력 없이 goose가 취할 수 있는 연속 턴의 최대 수입니다 (기본값: 1000). 제한에 도달하면 goose가 중지하고 프롬프트합니다: "사용자 입력 없이 수행할 수 있는 최대 작업 수에 도달했습니다. 계속하시겠습니까?" 사용자가 긍정적으로 응답하면 goose는 제한에 도달할 때까지 계속한 다음 다시 프롬프트합니다.

이 기능은 에이전트 자율성에 대한 제어를 제공하고 프로덕션 환경에서 상당한 비용 결과나 손상 영향을 미칠 수 있는 무한 루프 및 런어웨이 동작을 방지합니다. 다음 용도로 사용합니다:

- 자동화된 작업에서 무한 루프 및 과도한 API 호출 또는 리소스 소비 방지
- 자율 작업 중 사람의 감독 또는 상호작용 활성화
- 에이전트 동작 테스트 및 디버깅 중 루프 제어

이 설정은 [config.yaml 파일](/docs/guides/config-files)에 `GOOSE_MAX_TURNS` 환경 변수로 저장됩니다. 데스크톱 앱 또는 CLI를 사용하여 구성할 수 있습니다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>

      1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
      2. 사이드바에서 `Settings` 버튼을 클릭합니다
      3. `Chat` 탭을 클릭합니다
      4. `Conversation Limits`로 스크롤하여 `Max Turns` 값을 입력합니다

    </TabItem>
    <TabItem value="cli" label="goose CLI">

      1. `configuration` 명령을 실행합니다:
      ```sh
      goose configure
      ```

      2. `goose settings`를 선택합니다:
      ```sh
      ┌   goose-configure
      │
      ◆  What would you like to configure?
      │  ○ Configure Providers
      │  ○ Add Extension
      │  ○ Toggle Extensions
      │  ○ Remove Extension
      // highlight-start
      │  ● goose settings (Set the goose mode, Tool Output, Tool Permissions, Experiment, goose recipe github repo and more)
      // highlight-end
      └
      ```

      3. `Max Turns`를 선택합니다:
      ```sh
      ┌   goose-configure
      │
      ◇  What would you like to configure?
      │  goose settings
      │
      ◆  What setting would you like to configure?
      │  ○ goose mode
      │  ○ Router Tool Selection Strategy
      │  ○ Tool Permission
      │  ○ Tool Output
      // highlight-start
      │  ● Max Turns (Set maximum number of turns without user input)
      // highlight-end
      │  ○ Toggle Experiment
      │  ○ goose recipe github repo
      │  ○ Scheduler Type
      └
      ```

      4. 최대 턴 수를 입력합니다:
      ```sh
      ┌   goose-configure
      │
      ◇  What would you like to configure?
      │  goose settings
      │
      ◇  What setting would you like to configure?
      │  Max Turns
      │
        // highlight-start
      ◆  Set maximum number of agent turns without user input:
      │  10
        // highlight-end
      │
      └  Set maximum turns to 10 - goose will ask for input after 10 consecutive actions
      ```

      :::tip
      영구적인 `Max Turns` 설정 외에도 `goose session --max-turns` 및 `goose run --max-turns` [CLI 명령](/docs/guides/goose-cli-commands)을 통해 특정 세션이나 작업에 대한 런타임 재정의를 제공할 수 있습니다.
      :::

    </TabItem>

</Tabs>

**적절한 값 선택**

적절한 최대 턴 값은 사용 사례와 자동화에 대한 편안함 수준에 따라 다릅니다:

- **5-10턴**: 탐색 작업, 디버깅 또는 빈번한 체크인을 원하는 경우에 적합합니다. 예를 들어 각 단계를 검토하려는 "이 코드베이스를 분석하고 개선 사항을 제안해 주세요"
- **25-50턴**: 중간 복잡도의 잘 정의된 작업에 효과적입니다. 예를 들어 "이 모듈을 새 API를 사용하도록 리팩토링" 또는 "기본 CI/CD 파이프라인 설정"
- **100턴 이상**: goose가 독립적으로 작업하도록 신뢰하는 복잡한 다단계 자동화에 더 적합합니다. 예를 들어 "이 전체 프로젝트를 React 16에서 React 18로 마이그레이션" 또는 "이 서비스에 대한 포괄적인 테스트 커버리지 구현"

간단해 보이는 작업에도 종종 여러 턴이 필요합니다. 예를 들어 goose에게 "실패한 테스트 수정"을 요청하면 테스트 출력 분석(1턴), 근본 원인 식별(1턴), 코드 변경(1턴), 수정 확인(1턴)이 포함될 수 있습니다.

## 토큰 사용량
첫 번째 메시지를 보낸 후 goose 데스크톱 및 goose CLI에 토큰 사용량이 표시됩니다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
    데스크톱은 세션 창 하단의 모델 이름 옆에 색상 원을 표시합니다. 색상은 세션의 토큰 사용량을 시각적으로 나타냅니다.
      - **녹색**: 정상 사용량 - 충분한 컨텍스트 공간 사용 가능
      - **주황색**: 경고 상태 - 제한에 가까워짐 (용량의 80%)
      - **빨간색**: 오류 상태 - 컨텍스트 제한 도달

    이 원 위에 마우스를 올리면 다음이 표시됩니다:
      - 사용된 토큰 수
      - 사용된 가용 토큰의 백분율
      - 총 가용 토큰
      - 현재 토큰 사용량을 보여주는 진행률 막대

    </TabItem>
    <TabItem value="cli" label="goose CLI">
    CLI는 각 명령 프롬프트 위에 컨텍스트 레이블을 표시하여 다음을 보여줍니다:
      - 토큰 사용량을 나타내는 점(●○)과 색상을 사용한 시각적 표시기:
        - **녹색**: 50% 미만 사용량
        - **노란색**: 50-85% 사용량
        - **빨간색**: 85% 이상 사용량
      - 사용량 백분율
      - 현재 토큰 수 및 컨텍스트 제한

    </TabItem>
</Tabs>

## 모델 컨텍스트 제한 재정의

컨텍스트 제한은 모델 이름에 따라 자동으로 감지되지만 goose는 기본 제한을 재정의하는 설정을 제공합니다:

| 모델 | 설명 | 적합한 사용 | 설정 |
|-------|-------------|----------|---------|
| **Main** | 메인 모델의 컨텍스트 제한 설정 (다른 모델의 폴백 역할도 함) | LiteLLM 프록시, 비표준 이름의 사용자 정의 모델 | `GOOSE_CONTEXT_LIMIT` |
| **Lead** | [리드/워커 모드](/docs/tutorials/lead-worker)에서 계획을 위한 더 큰 컨텍스트 설정 | 더 많은 컨텍스트가 필요한 복잡한 계획 작업 | `GOOSE_LEAD_CONTEXT_LIMIT` |
| **Worker** | 리드/워커 모드에서 실행을 위한 더 작은 컨텍스트 설정 | 실행 단계에서 비용 최적화 | `GOOSE_WORKER_CONTEXT_LIMIT` |
| **Planner** | [플래너 모델](/docs/guides/creating-plans)의 컨텍스트 설정 | 광범위한 컨텍스트가 필요한 대규모 계획 작업 | `GOOSE_PLANNER_CONTEXT_LIMIT` |

:::info
이 설정은 표시되는 토큰 사용량 및 진행률 표시기에만 영향을 미칩니다. 실제 컨텍스트 관리는 LLM에서 처리되므로 표시되는 것과 관계없이 설정한 제한보다 더 많거나 적은 사용량을 경험할 수 있습니다.
:::

이 기능은 다음과 함께 특히 유용합니다:

- **LiteLLM 프록시 모델**: goose의 패턴과 일치하지 않는 사용자 정의 모델 이름으로 LiteLLM을 사용하는 경우
- **엔터프라이즈 배포**: 비표준 이름의 사용자 정의 모델 배포
- **파인튜닝된 모델**: 기본 버전과 다른 컨텍스트 제한이 있는 사용자 정의 모델
- **개발/테스트**: 테스트 목적으로 컨텍스트 제한을 일시적으로 조정

goose는 다음 우선순위(높은 것부터 낮은 것)로 컨텍스트 제한을 해결합니다:

1. 모델 구성에서 명시적 context_limit (프로그래밍 방식으로 설정된 경우)
2. 특정 환경 변수 (예: `GOOSE_LEAD_CONTEXT_LIMIT`)
3. 글로벌 환경 변수 (`GOOSE_CONTEXT_LIMIT`)
4. 이름 패턴 매칭을 기반으로 한 모델별 기본값
5. 글로벌 기본값 (128,000 토큰)

**구성**

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

     모델 컨텍스트 제한 재정의는 아직 goose 데스크톱 앱에서 사용할 수 없습니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    컨텍스트 제한 재정의는 구성 파일이 아닌 [환경 변수](/docs/guides/environment-variables#model-context-limit-overrides)로만 작동합니다.

    ```bash
    export GOOSE_CONTEXT_LIMIT=1000
    goose session
    ```

  </TabItem>

</Tabs>

**시나리오**

1. 사용자 정의 모델 이름이 있는 LiteLLM 프록시

```bash
# 사용자 정의 모델 이름이 있는 LiteLLM 프록시
export GOOSE_PROVIDER="openai"
export GOOSE_MODEL="my-custom-gpt4-proxy"
export GOOSE_CONTEXT_LIMIT=200000  # 32k 기본값 재정의
```

2. 다른 컨텍스트 제한이 있는 리드/워커 설정

```bash
# 계획 vs 실행에 대한 다른 컨텍스트 제한
export GOOSE_LEAD_MODEL="claude-opus-custom"
export GOOSE_LEAD_CONTEXT_LIMIT=500000    # 계획을 위한 대형 컨텍스트
export GOOSE_WORKER_CONTEXT_LIMIT=128000  # 실행을 위한 소형 컨텍스트
```

3. 대형 컨텍스트가 있는 플래너

```bash
# 복잡한 계획을 위한 대형 컨텍스트
export GOOSE_PLANNER_MODEL="gpt-4-custom"
export GOOSE_PLANNER_CONTEXT_LIMIT=1000000
```

## 비용 추적
세션의 실시간 예상 비용을 표시합니다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
실시간 비용 추적을 관리하려면:
  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
  2. 사이드바에서 `Settings` 버튼을 클릭합니다
  3. `App` 탭을 클릭합니다
  4. `Cost Tracking`을 켜거나 끕니다

세션 비용은 goose 창 하단에 표시되며 토큰이 소비됨에 따라 동적으로 업데이트됩니다. 비용 위에 마우스를 올리면 토큰 사용량의 상세 내역이 표시됩니다. 세션에서 여러 모델이 사용된 경우 모델별 비용 내역이 포함됩니다. Ollama 및 로컬 배포는 항상 $0.00의 비용을 표시합니다.

가격 데이터는 OpenRouter API에서 정기적으로 가져와 로컬에 캐시됩니다. `Advanced settings` 탭은 데이터가 마지막으로 업데이트된 시기를 보여주고 새로 고침할 수 있게 합니다.

이 비용은 예상치일 뿐이며 실제 공급자 청구서와 연결되지 않습니다. 표시되는 비용은 토큰 수와 공개 가격 데이터를 기반으로 한 근사치입니다.
</TabItem>
    <TabItem value="cli" label="goose CLI">
    `GOOSE_CLI_SHOW_COST` [환경 변수](/docs/guides/environment-variables.md#session-management)를 설정하거나 [구성 파일](/docs/guides/config-files.md)에 포함하여 goose CLI에서 예상 비용을 표시합니다.

  ```
  # 환경 변수 설정
  export GOOSE_CLI_SHOW_COST=true

  # config.yaml
  GOOSE_CLI_SHOW_COST: true
  ```
  </TabItem>
</Tabs>
