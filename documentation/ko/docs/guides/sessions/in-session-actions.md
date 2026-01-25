---
sidebar_position: 2
title: 세션 내 작업
sidebar_label: 세션 내 작업
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Paperclip, Edit2, Send, GripVertical, X, ChevronUp, ChevronDown, FolderDot, Puzzle, Bot, Tornado } from 'lucide-react';

goose는 세션 중에 대화를 관리하고 정보를 공유하는 데 사용할 수 있는 기능을 제공합니다.

## 메시지 편집

이전에 보낸 메시지를 편집하여 대화를 개선하거나, 방향을 수정하거나, 다른 접근 방식을 시도할 수 있습니다.

**메시지 흐름 예시:**

원래 대화에 5개의 메시지가 있습니다. 메시지 3을 편집하면 메시지 4와 5의 모든 메시지 및 응답 컨텍스트가 삭제됩니다.

```
┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐   ┌─────┐
│  1  │ → │  2  │ → │  3  │ → │  4  │ → │  5  │
└─────┘   └─────┘   └─────┘   └─────┘   └─────┘

                  여기서 편집
                       ↓
┌─────┐   ┌─────┐   ┌─────┐    현재 세션에서 계속하거나
│  1  │ → │  2  │ → │  3  │ →  새 세션으로 복사
└─────┘   └─────┘   └─────┘
```

현재 세션에서 계속 작업하거나 새 세션에서 포크를 만들 수 있습니다:

- [현재 위치에서 편집](#현재-위치에서-편집): 현재 세션을 업데이트하고 편집된 메시지 이후의 모든 메시지 및 응답 컨텍스트를 삭제합니다. 이렇게 하면 특정 시점에서 대화를 다시 시작할 수 있습니다.
- [세션 포크](#세션-포크): 편집된 메시지를 포함하여 그 이전의 모든 대화 기록이 있는 새 세션을 만듭니다. 이렇게 하면 원래 대화를 유지하면서 다른 접근 방식을 탐색할 수 있습니다.

### 현재 위치에서 편집

현재 위치에서 편집은 편집된 메시지 이후의 모든 컨텍스트를 덮어쓰어 대화 기록을 완전히 제어할 수 있게 합니다. 마지막 메시지의 경로를 수정하는 것처럼 간단하거나 특정 시점에서 완전히 다시 시작하는 것일 수 있습니다.

현재 위치에서 편집이 유용한 경우:

- 보낸 프롬프트가 불명확하거나 불완전하다는 것을 깨달았을 때
- goose가 의도를 잘못 이해하고 잘못된 방향으로 갔을 때

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>

        1. 이전 메시지 위에 마우스를 올립니다
        2. 나타나는 <Edit2 className="inline" size={16} /> `Edit` 버튼을 클릭합니다
        3. 인라인 편집기에서 변경합니다
        4. `Edit in Place`를 클릭하여 변경 사항을 저장하고 goose에게 다시 프롬프트합니다

        goose는 편집된 메시지 이후의 모든 대화 기록을 제거하고 해당 시점부터 컨텍스트에 맞게 응답합니다.

        :::warning 삭제된 컨텍스트
        현재 위치에서 편집을 사용하면 후속 대화 기록이 세션에서 영구적으로 삭제되고 goose의 컨텍스트에서 제거됩니다. goose가 편집된 메시지 이후의 컨텍스트를 기억할 필요가 없는 경우에만 이 옵션을 사용하세요.
        :::

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        메시지 편집 옵션은 goose CLI에서 사용할 수 없습니다.
    </TabItem>
</Tabs>

### 세션 포크

세션 포크는 원래 대화를 유지하면서 편집된 메시지가 있는 새 세션을 만듭니다. 원래 세션을 참조점으로 유지하면서 변형을 실험하고 결과를 비교할 수 있습니다.

세션 포크가 유용한 경우:
- 같은 문제에 대해 다른 접근 방식을 나란히 비교
- 다른 프롬프트가 goose의 응답에 어떤 영향을 미치는지 테스트

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>

        1. 이전 메시지 위에 마우스를 올립니다
        2. 나타나는 <Edit2 className="inline" size={16} /> `Edit` 버튼을 클릭합니다
        3. 인라인 편집기에서 변경합니다
        4. `Fork Session`을 클릭하여 변경 사항을 저장하고 새 세션을 시작합니다 (또는 `Cmd+Enter` (macOS) 또는 `Ctrl+Enter` (Windows/Linux) 사용)

        goose는 원래 세션의 대화 기록을 새 세션으로 복사합니다. 새 세션은 "(edited)"로 이름이 지정되고 원래 세션은 변경되지 않습니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        메시지 편집 옵션은 goose CLI에서 사용할 수 없습니다.
    </TabItem>
</Tabs>

### 편집 시나리오 팁

- **반복적 프롬프트 개선**: 기본 프롬프트로 시작한 다음 goose의 응답을 기반으로 편집하고 개선합니다. 이것이 처음부터 완벽한 프롬프트를 만들려고 하는 것보다 더 잘 작동하는 경우가 많습니다.
- **편집할 때 vs. 중단할 때**: 대화가 잘못된 방향으로 갔을 때 이전 메시지를 편집하는 것이 새 메시지나 [중단](#작업-중단)을 사용하여 방향을 수정하는 것보다 더 효과적일 수 있습니다. 메시지를 편집하면 기록을 다시 쓰게 됩니다. 중단을 사용하면 현재 메시지 이후부터의 대화에만 영향을 미칩니다.
- **진행 상황 유지**: 좋은 진행을 했지만 다른 접근 방식을 시도하고 싶을 때 세션 포크를 사용합니다. 이렇게 하면 새 방향이 잘 되지 않더라도 항상 원래대로 돌아갈 수 있습니다.

## 메시지 대기열

goose가 작업을 처리하는 동안 메시지를 대기열에 넣어 워크플로우를 관리합니다. 다음과 같은 경우에 유용합니다:

- goose가 작업하는 동안 다음 단계를 준비하고 싶을 때
- 완료해야 할 관련 작업의 시퀀스가 있을 때
- [음성 받아쓰기](#음성-받아쓰기)를 사용하고 생각을 빠르게 캡처해야 할 때

:::tip
복잡한 작업을 하위 작업으로 분할하면 goose가 더 잘 수행할 수 있습니다. 이 기술을 [*프롬프트 체이닝*](https://www.promptingguide.ai/techniques/prompt_chaining)이라고 합니다. 이 구조화된 접근 방식은 정확도를 높이고 프로세스에 대한 더 많은 제어를 제공할 수 있습니다.
:::

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
      대기열에 메시지를 추가합니다:
      1. goose가 응답을 처리하는 동안 다음 메시지를 입력합니다
      2. `Enter`를 눌러 대기열에 추가합니다 ([중단 키워드](#작업-중단)를 사용하면 중단됨)

      대기열에 추가된 메시지는 대기열 순서를 보여주는 번호가 매겨진 카드로 표시됩니다. 대기열의 첫 번째 메시지는 goose가 각 응답을 완료하면 자동으로 전송됩니다.

      :::info 관련 기능
      - 일반적으로 goose가 작업을 처리하는 동안 `Enter`를 누르면 메시지가 대기열에 추가되지만, `Send`를 클릭하면 메시지가 즉시 전송되고 [작업이 중단](#작업-중단)됩니다
      - 대기열에 추가된 메시지에 "stop", "wait", "hold on"과 같은 일반적인 중단 키워드를 입력하면 goose는 다음 메시지를 입력하거나 보낼 때까지 일시 중지한 다음 대기열 처리를 계속합니다
      :::

      #### 대기열 관리 컨트롤

      대기열에 추가된 메시지는 goose가 각 작업을 완료하면 자동으로 순서대로 실행되지만 대기열을 관리할 수 있습니다:
      - **메시지 편집**: 메시지 텍스트를 클릭하여 편집 컨트롤을 표시한 다음 변경 사항을 입력하고 `Save`를 클릭합니다
      - **메시지 순서 변경**: 메시지 카드 위에 마우스를 올려 <GripVertical className="inline" size={16} /> 버튼을 표시한 다음 잡아서 위나 아래로 끕니다
      - **메시지 전송**: <Send className="inline" size={16} /> 버튼을 클릭하여 메시지를 즉시 전송하고 현재 작업을 중단합니다
      - **메시지 삭제**: <X className="inline" size={16} /> 버튼을 클릭하여 메시지를 삭제합니다
      - **대기열 지우기**: **Message Queue** 카드에서 `Clear All`을 클릭합니다
      - **대기열 접기 또는 펼치기**: **Message Queue** 카드에서 <ChevronUp className="inline" size={16} /> 또는 <ChevronDown className="inline" size={16} /> 버튼을 클릭합니다

      #### 메시지 흐름 예시

      **대기열 없이:**

      보내는 내용: "인증 코드를 OAuth 2.0을 지원하도록 리팩토링하고 적절한 오류 처리를 추가해 주세요. OAuth 흐름에 대한 단위 테스트도 포함하고, 이러한 변경 사항을 반영하도록 API 문서를 업데이트하고, 기존 사용자가 새 시스템으로 전환하는 데 도움이 되는 마이그레이션 스크립트를 만들어 주세요."

      이 접근 방식은 중요한 세부 사항이 누락되거나 작업이 피상적으로 처리되는 압도적인 응답으로 이어질 수 있습니다. 명확한 순차적 단계가 있는 단일 프롬프트를 보내더라도 goose가 각 작업에 개별적으로 집중하거나 점진적으로 컨텍스트를 구축할 수 없습니다.

      **대기열 사용:**

      1. 보내는 내용: "인증 코드를 OAuth 2.0을 지원하도록 리팩토링"
      2. goose가 작업하는 동안 다음 메시지를 대기열에 넣습니다:
         - "적절한 오류 처리 추가"
         - "OAuth 흐름에 대한 단위 테스트 추가"
         - "API 문서 업데이트"
         - "기존 사용자를 위한 마이그레이션 스크립트 만들기"

      각 작업은 이전 작업을 기반으로 합니다.

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        메시지 대기열은 goose CLI에서 사용할 수 없습니다.
    </TabItem>
</Tabs>

## 작업 중단

goose가 작업을 처리하는 동안 중단하여 대화를 제어합니다. 다음과 같은 경우에 유용합니다:

- goose가 잘못된 방향으로 가고 있을 때
- 중요한 컨텍스트를 추가해야 한다는 것을 깨달았을 때
- 완전히 다른 작업으로 전환하고 싶을 때

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>

        작업을 중단하는 두 가지 방법이 있습니다:

        #### 중단 키워드 보내기
        1. `stop`, `wait`, `hold on`, `actually`, `instead`와 같은 일반적인 중단 키워드가 포함된 프롬프트를 입력합니다. 키워드만 사용하거나 문장 시작 부분에 사용하면 안정적인 감지가 가장 잘 작동합니다.
        2. `Send`를 클릭합니다

        goose는 현재 작업 처리를 중지하고 추가 정보를 요청합니다.

        #### 즉시 방향 전환 제공
        1. 더 많은 컨텍스트와 설명이 있거나 방향을 변경하는 프롬프트를 입력합니다. 예를 들어:
           - "이것이 모바일 앱용이라는 것을 언급하는 것을 잊었습니다"
           - "TypeScript 대신 React에 집중합시다"
        2. `Send`를 클릭합니다

        goose는 현재 작업 처리를 중지하고 새 요청 컨텍스트로 전환합니다.

        :::info 관련 기능
        - goose가 작업을 처리하는 동안 `Send`를 클릭하면 작업이 중단되지만 `Enter`를 누르면 [메시지가 대기열에 추가](#메시지-대기열)됩니다
        - 대기열에 추가된 메시지에 중지 또는 일시 중지 키워드를 입력하면 goose가 현재 작업 처리를 중지합니다
        - 세션 중에 더 많은 컨텍스트와 설명을 제공하거나 방향을 변경하기 위해 [보낸 메시지를 편집](#메시지-편집)할 수도 있습니다
        :::

        <details>
          <summary>중단 키워드 목록</summary>

          **고우선순위 키워드** (모든 상황에서 중단):
          ```
          stop, halt, cease, quit, end, abort, cancel, wait, hold, pause, hold on, wait up, hold up
          ```

          **중간 우선순위 키워드** (정확히 일치하거나 문장 시작 부분에서만 중단):
          ```
          no, nope, nah, wrong, incorrect, not right, actually, instead, rather, better idea, change of plans, nevermind, never mind, forget it, ignore that, disregard
          ```

          **감지 규칙**:
          - **정확한 일치** (100% 신뢰도): 단어/구문이 정확히 일치하고 항상 중단
          - **문장 시작** (매우 높은 신뢰도): 단어/구문이 메시지를 시작하고 항상 중단
          - **짧은 메시지만** (높은 신뢰도): 20자 이하의 메시지에서는 고우선순위 키워드만 중단
          - **대소문자 구분 없음**: 모든 감지는 대소문자를 구분하지 않음

          **예시**:
          - ✅ "stop"은 중단 (정확한 일치)
          - ✅ "Wait, I meant something else"는 중단 (문장 시작)
          - ✅ "no"는 중단 (짧은 메시지, 고우선순위)
          - ❌ 짧은 메시지에서 "actually"는 중단하지 않음 (짧은 메시지에서 중간 우선순위)
          - ✅ "Actually, let's try React instead"는 중단 (문장 시작)

        </details>

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        1. `Ctrl+C`를 눌러 현재 작업을 중단합니다
        2. 더 많은 컨텍스트를 제공하거나 방향을 변경하는 프롬프트를 입력합니다
        3. `Enter`를 누릅니다

        goose는 새 요청에 컨텍스트에 맞게 응답합니다.

    </TabItem>
</Tabs>

## 음성 받아쓰기
프롬프트를 입력하는 대신 goose에게 직접 말합니다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
    음성 받아쓰기를 활성화하려면:
        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
        2. 사이드바에서 `Settings`를 클릭합니다
        3. `Chat`을 클릭합니다
        4. `Voice Dictation`에서 `Enable Voice Dictation`을 켭니다
        5. 받아쓰기 공급자로 `OpenAI Whisper` 또는 `ElevenLabs` 중에서 선택합니다
        6. 선택한 공급자의 API 키를 입력합니다

    음성 받아쓰기를 사용하려면:
        1. 채팅 인터페이스로 돌아갑니다 (사이드바에서 `Chat` 클릭)
        2. 채팅 상자 오른쪽에 있는 마이크를 클릭하고 말하기 시작합니다

        음성 받아쓰기를 처음 사용할 때 goose가 마이크 액세스를 요청합니다. 녹음하는 동안 입력 필드에 오디오의 실시간 파형, 타이머 및 녹음의 현재 크기가 표시됩니다. 녹음을 완료하려면 마이크 버튼을 다시 클릭합니다.

        **마이크가 보이지 않으면** [구성한 모델](/docs/getting-started/providers.md)을 확인하세요. ElevenLabs는 모든 LLM과 함께 받아쓰기 공급자로 사용할 수 있지만 OpenAI Whisper는 다른 LLM 공급자를 채팅에 사용하더라도 goose에 OpenAI 모델이 구성되어 있어야 합니다.

       #### 중요 참고 사항
        * 최대 10분 또는 25MB의 오디오를 녹음할 수 있습니다
        * 오디오는 선택한 공급자(OpenAI 또는 ElevenLabs)에서 처리됩니다
        * 음성 입력은 텍스트 입력 필드의 기존 텍스트에 추가되므로 타이핑과 음성을 결합하여 프롬프트를 작성할 수 있습니다
        * 녹음은 전사 후 로컬에 저장되지 않습니다

  </TabItem>
    <TabItem value="cli" label="goose CLI">
        음성 받아쓰기는 goose CLI에서 사용할 수 없습니다.
    </TabItem>
</Tabs>

## 세션에서 파일 공유

코드베이스, 문서 및 기타 파일에서 goose에게 컨텍스트를 제공하여 더 관련성 있고 정확한 지원을 받습니다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        여러 방법으로 goose와 파일을 공유합니다:

        1. **끌어다 놓기**: 컴퓨터의 파일 탐색기/파인더에서 파일을 드래그하여 채팅 창 어디에나 놓습니다. 파일 경로가 자동으로 메시지에 추가됩니다.

        2. **파일 브라우저**: 앱 하단의 <Paperclip className="inline" size={16} /> 버튼을 클릭하여 시스템의 파일 브라우저를 열고 파일을 선택합니다

        3. **수동 경로**: 채팅 입력에 파일 경로를 직접 입력하거나 붙여넣습니다

        4. **빠른 파일 검색**: [`@` 단축키](/docs/guides/file-management#quick-file-search-in-goose-desktop)를 사용하여 빠르게 파일을 찾고 포함합니다
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        메시지에서 경로로 직접 파일을 참조할 수 있습니다. 이미 터미널에 있으므로 표준 셸 명령을 사용하여 파일 경로를 도울 수 있습니다:

        ```bash
        # 특정 파일 참조
        이 코드가 무엇을 하는지 알려주세요? ./src/main.rs

        # 탭 완성 사용
        ./src/lib<tab>에 있는 함수를 설명해 주세요

        # 셸 확장 사용
        이 테스트 파일들을 검토해 주세요: ./tests/*.rs
        ```
    </TabItem>
</Tabs>

## 세션 중 변경

세션 중에 일부 설정을 변경하면 새 세션을 시작할 필요 없이 즉시 적용됩니다. 이렇게 하면 goose와 상호작용하는 동안 컨텍스트와 기능을 더 잘 제어할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  앱 하단의 도구 모음을 사용하여 세션 중에 지원되는 설정을 변경합니다:

  | 설정 | 도구 모음 항목 | 지속성* |
  |---------|--------------|-------------|
  | **작업 디렉토리** | <FolderDot className="inline" size={16} /> 디렉토리 전환기 | 새 세션 (재시작 후) |
  | [**활성화된 확장 기능**](/docs/getting-started/using-extensions#change-extensions-mid-session) | <Puzzle className="inline" size={16} /> 아이콘 | 현재 세션만 |
  | [**모델**](/docs/getting-started/providers#configure-provider-and-model) | <Bot className="inline" size={16} /> 모델 전환기 | 새 세션 |
  | [**goose 모드**](/docs/guides/goose-permissions#configuring-goose-mode) | <Tornado className="inline" size={16} /> 모드 전환기 | 새 세션 |

  </TabItem>
  <TabItem value="cli" label="goose CLI">

  슬래시 명령을 사용하여 세션 중에 지원되는 설정을 변경합니다:

  | 설정 | 슬래시 명령 | 지속성* |
  |---------|--------------|-------------|
  | [**활성화된 확장 기능**](/docs/getting-started/using-extensions#change-extensions-mid-session) | `/extension` 또는 `/builtin` | 현재 세션만 |
  | [**goose 모드**](/docs/guides/goose-permissions#configuring-goose-mode) | `/mode [옵션]` | 새 세션 |

  :::info
  CLI는 [추가 슬래시 명령](/docs/guides/goose-cli-commands#slash-commands)을 지원하지만 작업 디렉토리나 모델의 세션 중 변경은 지원하지 않습니다.
  :::

  </TabItem>
</Tabs>

*지속성은 변경 사항이 현재 세션에만 적용되는지 또는 새 세션으로 이어지는지를 나타냅니다
