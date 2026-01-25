---
title: 연구 → 계획 → 구현 패턴
description: 복잡한 소프트웨어 프로젝트에서 컨텍스트 엔지니어링 기법인 RPI를 사용하는 방법
---

import CodeBlock from '@theme/CodeBlock';
import researchDoc from '../../static/files/thoughts/research/2025-12-22-llm-tool-selection-strategy.raw';
import planDoc from '../../static/files/thoughts/plans/2025-12-23-remove-tool-selection-strategy.raw';


대부분의 사람들은 AI 에이전트를 실행으로 바로 점프하여 사용합니다: "이 코드를 리팩토링해", "이 기능을 제거해", "이 새로운 기능을 추가해". 때때로 이것은 잘 작동하며, 특히 작은 변경이나 코드베이스에서 그렇지만, 복잡한 변경에서는 종종 무너집니다.

HumanLayer가 도입한 **[RPI (Research, Plan, Implement)](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)**는 AI 에이전트와 작업하는 다른 방식을 제안합니다. 이 접근 방식은 속도를 명확성, 예측 가능성 및 정확성으로 교환합니다.

이 튜토리얼은 실제 시연을 통해 RPI가 어떻게 작동하는지 안내합니다. 끝까지 읽으면 자신의 코드베이스에서 동일한 워크플로우를 실행할 수 있을 것입니다.

## 사전 요구 사항

<details>
<summary>1. RPI 레시피 가져오기</summary>

아래 스니펫을 복사하여 터미널에 붙여넣습니다. 이렇게 하면 메인 RPI 레시피와 서브레시피가 다운로드되어 전역 레시피 디렉토리에 저장됩니다.

```sh
mkdir -p ~/.config/goose/recipes/subrecipes

curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/rpi-research.yaml -o ~/.config/goose/recipes/rpi-research.yaml
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/rpi-plan.yaml -o ~/.config/goose/recipes/rpi-plan.yaml
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/rpi-implement.yaml -o ~/.config/goose/recipes/rpi-implement.yaml
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/rpi-iterate.yaml -o ~/.config/goose/recipes/rpi-iterate.yaml

curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/subrecipes/rpi-codebase-locator.yaml -o ~/.config/goose/recipes/subrecipes/rpi-codebase-locator.yaml
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/subrecipes/rpi-codebase-analyzer.yaml -o ~/.config/goose/recipes/subrecipes/rpi-codebase-analyzer.yaml
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/subrecipes/rpi-pattern-finder.yaml -o ~/.config/goose/recipes/subrecipes/rpi-pattern-finder.yaml
```
</details>

<details>
<summary>2. 사용자 정의 슬래시 명령 추가</summary>

레시피를 가져왔으니 세션 내에서 빠르게 호출하려면 다음 레시피 각각에 대해 [사용자 정의 슬래시 명령을 추가](/docs/guides/context-engineering/slash-commands)합니다:

| 레시피 | 슬래시 명령 |
|--------|---------------|
| RPI Research Codebase | `research` |
| RPI Create Plan | `plan` |
| RPI Implement Plan | `implement` |
| RPI Iterate | `iterate` |
</details>

## RPI 워크플로우

goose에서는 레시피를 사용하는 구조화된 RPI 워크플로우를 사용하여 복잡한 코드베이스 변경을 체계적으로 처리합니다. 워크플로우는 goose가 규율 잡힌 작업 단계를 거치도록 안내하는 슬래시 명령으로 구성됩니다:

1. `research` – 오늘 존재하는 것을 문서화합니다. 의견 없이.
2. `plan` - 명확한 단계와 성공 기준으로 변경을 설계합니다.
3. `implement` - 검증과 함께 단계별로 계획을 실행합니다.
4. `iterate` – (선택 사항) 필요한 경우 계획을 조정합니다.

```md
┌─────────────────────────────────────────────────────────────────────────────-┐
│                           RPI 워크플로우                                       │
├─────────────────────────────────────────────────────────────────────────────-┤
│                                                                              │
│  /research "주제"                                                             │
│       │                                                                      │
│       ├──► 병렬 서브 에이전트 생성:                                             │
│       │    • find_files (rpi-codebase-locator)                               │
│       │    • analyze_code (rpi-codebase-analyzer)                            │
│       │    • find_patterns (rpi-pattern-finder)                              │
│       │                                                                      │
│       └──► 출력: thoughts/research/YYYY-MM-DD-HHmm-topic.md                  │
│                                                                              │
│  /plan "기능/작업"                                                            │
│       │                                                                      │
│       ├──► 연구 문서 읽기                                                     │
│       ├──► 명확한 질문 하기                                                   │
│       ├──► 설계 옵션 제안                                                     │
│       │                                                                      │
│       └──► 출력: thoughts/plans/YYYY-MM-DD-HHmm-description.md               │
│                                                                              │
│  /implement "계획 경로"                                                       │
│       │                                                                      │
│       ├──► 단계별 실행                                                        │
│       ├──► 각 단계 후 검증 실행                                               │
│       ├──► 계획에서 체크박스 업데이트                                          │
│       │                                                                      │
│       └──► 작동하는 코드                                                      │
│                                                                              │
│  /iterate "계획 경로" + 피드백                                                 │
│       │                                                                      │
│       ├──► 변경된 것만 연구                                                   │
│       ├──► 계획을 정밀하게 업데이트                                            │
│       │                                                                      │
│       └──► 업데이트된 계획                                                    │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────-┘
```

모든 RPI 출력은 예측 가능한 위치에 있습니다:

```md
thoughts/
├── research/
│   └── YYYY-MM-DD-HHmm-topic.md
└── plans/
    └── YYYY-MM-DD-HHmm-description.md
```

## 작업

이 튜토리얼에서는 대규모 코드베이스에서 기존 기능을 제거하려고 합니다.

이것은 작은 변경이 아닙니다. 이 기능은 다음에 영향을 미칩니다:

- 핵심 Rust 코드
- TypeScript
- 구성
- 테스트
- 문서

이것은 에이전트가 종종 어려움을 겪는 종류의 작업입니다. 능력이 없어서가 아니라 작업이 "그냥 하기"에는 너무 많은 컨텍스트에 걸쳐 있기 때문입니다.

그래서 구현으로 바로 점프하는 대신 RPI를 사용할 것입니다.

## 세션 1: 연구

구현 전 계획의 개념은 널리 받아들여진 관행이 되었습니다. 그러나 연구 없이 계획하면 나중에 문제가 될 수 있는 가정으로 이어질 수 있습니다. 그래서 RPI에서는 연구로 시작합니다.

자연어로 작성된 주제와 함께 `/research` 명령으로 프롬프트를 시작합니다

```
/research "look through the cloned goose repo and research how the LLM Tool Discovery is implemented"
```


이 명령은 **[RPI Research Codebase](https://raw.githubusercontent.com/block/goose/refs/heads/main/documentation/src/pages/recipes/data/recipes/rpi-research.yaml)** 레시피를 호출하며, 그 역할은 매우 엄격합니다:

- 존재하는 것을 문서화
- 변경 제안 없음
- 비판 없음
- 계획 없음

이 레시피로 goose는 자동으로 세 개의 병렬 서브에이전트를 생성합니다:

- `find_files`: 코드베이스 로케이터를 사용하여 관련 파일의 위치를 파악합니다.
- `analyze_code`: 해당 파일을 완전히 읽고 작동 방식을 문서화합니다.
- `find_patterns`: 리포지토리의 다른 곳에서 유사한 기능이나 규칙을 찾습니다.

이 서브에이전트는 독립적으로 실행되고 보고합니다. 직접 조율할 필요가 없습니다.

:::info 경로 수정
goose가 연구를 시작한 후 일반적으로 "도구 검색"을 연구하고 있다는 것을 알아챘습니다. 하지만 저는 Tool Selection Strategy라는 특정 기능만 제거하고 싶었습니다. 그래서 goose를 중지하고 더 정확한 주제로 연구를 다시 실행했습니다.

그것은 실패가 아니었습니다. 사실 이것이 바로 연구가 존재하는 이유입니다. goose에게 "LLM Tool Discovery 기능을 제거해"라고 말했다면 다른 도구 검색 방법도 제거했을 수 있습니다. 다행히 이런 종류의 실수를 일찍 잡는 것은 저렴하고 쉽게 복구할 수 있습니다.
:::

`/research` 세션의 출력은 자세한 연구 문서였습니다:

<details>
<summary>./thoughts/research/2025-12-22-llm-tool-selection-strategy.md</summary>

<CodeBlock language="markdown">
{researchDoc}
</CodeBlock>

</details>

이것은 다음을 포함하는 크고 구조화된 파일입니다:
- Git 메타데이터
- 파일 및 라인 참조
- 흐름 설명
- 주요 구성 요소
- 열린 질문

오늘 존재하는 기능의 기술 지도라고 생각하세요. 아직 아무것도 변경되지 않았으며 이것은 의도적입니다. 유일한 목표는 공유된 이해였습니다.

루프에서 인간으로서 연구를 반드시 검토하세요! 이것이 계획에 영향을 미치므로 정확한지 확인하세요.

## 세션 2: 계획

연구가 완료되면 계획으로 이동합니다.

:::tip 세션
각 단계를 새 세션에서 수행하여 LLM이 당면한 작업에만 집중하도록 하는 것이 중요합니다. 세션당 하나의 목표!
:::

```
/plan a removal of the Tool Selection Strategy feature
```

**[RPI Create Plan](https://raw.githubusercontent.com/block/goose/refs/heads/main/documentation/src/pages/recipes/data/recipes/rpi-plan.yaml)** 레시피는 goose가 만든 연구 문서를 읽는 것으로 시작합니다.

그런 다음 세 가지 주요 작업을 수행했습니다:

1. **명확한 질문 하기**

    예:
    - 완전 제거 vs 지원 중단?
    - 구성 정리는 어떻게 동작해야 하나요?
    - OpenAPI 아티팩트를 재생성해야 하나요?
    - 관련 테스트는 어디에 있나요?

2. **설계 옵션 제시**

    여러 가지 합리적인 접근 방식이 있는 경우 goose는 이를 제시하고 선택하라고 요청했습니다.

3. **단계별 구현 계획 생성**


출력은 자세한 계획이었습니다:

<details>
<summary>thoughts/plans/2025-12-23-remove-tool-selection-strategy.md</summary>

<CodeBlock language="markdown">
{planDoc}
</CodeBlock>

</details>

이 계획에는 다음이 포함됩니다:
- 10개의 명시적 단계
- 정확한 파일 경로
- 제거할 내용을 보여주는 코드 스니펫
- 자동화된 성공 기준
- 수동 검증 단계
- 진행 상황 추적을 위한 체크박스

이 시점에서 계획이 진실의 원천이 되었습니다. 여기서 핵심 전환은 이해에서 의사 결정으로 이동했지만 아직 코드를 건드리지 않는다는 것입니다.

계획은 다른 사람이 실행할 수 있을 만큼 명시적입니다. 그것은 우연이 아닙니다. 구현은 새로운 세션에서 이루어질 것이므로 계획에는 실제로 실행할 수 있을 만큼의 컨텍스트가 있어야 합니다.

다시 한 번, 인간으로서 여기서 계획을 검토하고 견고한지 확인해야 합니다. 문제가 있으면 처음부터 시작하는 대신 **[RPI Iterate Plan](https://raw.githubusercontent.com/block/goose/refs/heads/main/documentation/src/pages/recipes/data/recipes/rpi-iterate.yaml)** 계획(`/iterate`)을 문제에 대한 세부 사항과 함께 실행할 수 있습니다. goose는 기존 계획을 읽고, 재고가 필요한 것만 연구하고, 대상 업데이트를 제안하고, 그에 따라 계획을 편집합니다.

## 세션 3: 구현

연구와 계획이 완료된 후에만 구현으로 이동해야 합니다. 계획 문서를 전달합니다.

```
/implement thoughts/plans/2025-12-23-remove-tool-selection-strategy.md
```

**[RPI Implement Plan](https://raw.githubusercontent.com/block/goose/refs/heads/main/documentation/src/pages/recipes/data/recipes/rpi-implement.yaml)** 레시피는 의도적으로 지루합니다. 사실 goose가 실행하는 동안 잠이 들었습니다. 구현은 기계적으로 느껴져야 합니다. 창의적으로 느껴지면 상위에서 무언가가 누락된 것입니다. 그러나 견고한 계획이 있다는 것을 알고 있으므로 goose가 작업하는 동안 다른 일을 하러 가는 것이 좋습니다(계획에 수동 단계가 없는 한).

계획을 완전히 읽고, 순서대로 단계를 실행하고, 각 단계 후 검증을 실행하고, 진행하면서 계획 파일의 체크박스를 직접 업데이트합니다.

마지막 부분이 정말 도움이 되었는데, 제 컨텍스트 윈도우가 중간에 가득 차서 goose가 계획의 상태 업데이트 덕분에 압축하고 바로 중단한 곳에서 다시 시작할 수 있었기 때문입니다.

## 최종 결과

32개 파일에 걸친 10단계의 작업에서 연구 단계는 9분, 계획 단계는 4분, 구현 단계는 39분이 걸렸습니다. 총 1시간이 채 안 되었습니다... 정확히 52분. 여기에는 goose의 작업과 테스트, 그리고 제가 질문에 답하는 것이 포함되었습니다.

확실히 빠른 프로세스는 아닙니다. 하지만! [이 PR](https://github.com/block/goose/pull/6250)을 올렸을 때 빌드가 통과했고 별도의 코드 리뷰 에이전트가 단 하나의 코멘트도 없었습니다. 작업이 그렇게 잘 수행되었습니다.

AI 없이 이 작업을 했다면 기능이 복잡하고 깊게 통합되어 있어 아마 몇 시간의 작업이 걸렸을 것입니다. 그리고 AI가 바로 구현으로 점프하게 했다면 분명히 drift되어 무언가를 망쳤을 것입니다.

그래서 RPI는 AI가 바로 작업하는 것보다 느리지만 품질은 최고입니다. 매우 가치 있는 트레이드오프입니다.


## RPI를 언제 사용하나요

기본적인 작업에는 RPI가 과할 수 있습니다. 특히 빠른 프로세스가 아니기 때문입니다. 그러나 여러 파일에 걸친 복잡한 작업을 수행해야 하는 경우 훌륭한 선택입니다.

다음에 RPI를 사용할 수 있습니다:
- 리팩토링
- 마이그레이션
- 기능 추가
- 대규모 업그레이드
- 인시던트 정리
- 문서 대폭 개편

직접 사용해 보세요!
