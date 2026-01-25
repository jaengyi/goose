---
description: LLM을 페어링하여 작업을 완료하는 멀티 모델 기능 활성화
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 리드/워커 멀티 모델 설정

goose는 두 가지 다른 AI 모델을 페어링할 수 있는 리드/워커 모델 구성을 지원합니다 - 하나는 생각하는 데 뛰어나고 다른 하나는 실행하는 데 빠릅니다. 이 설정은 주요 페인 포인트를 해결합니다: 프리미엄 모델은 강력하지만 비싸고, 저렴한 모델은 빠르지만 복잡한 작업에서 어려움을 겪을 수 있습니다. 리드/워커 모드를 사용하면 두 가지 장점을 모두 얻을 수 있습니다.

<details>
  <summary>리드/워커 모드 연습</summary>
  <iframe
    className="aspect-ratio"
    src="https://youtube.com/embed/ZyhUTsChFUw"
    title="리드/워커 모드 설정 및 설정 설명"
    frameBorder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
  ></iframe>
</details>

리드/워커 모델은 스마트한 핸드오프 시스템입니다. "리드" 모델(예: GPT-4 또는 Claude Opus)이 시작하여 초기 계획과 큰 그림 추론을 처리합니다. 방향이 설정되면 goose는 작업을 "워커" 모델(예: GPT-4o-mini 또는 Claude Sonnet)에게 넘겨 단계를 수행하게 합니다.

문제가 발생하면(예: 워커 모델이 혼란스러워지거나 계속 실수를 하면) goose는 이를 알아채고 자동으로 리드 모델을 다시 불러 복구합니다. 상황이 다시 정상으로 돌아오면 워커가 다시 인수합니다.

## 턴 기반 시스템

**턴**은 하나의 완전한 상호 작용입니다 - 프롬프트와 모델의 응답. goose는 턴에 따라 모델을 전환합니다:

- **초기 턴** (기본값: 3)은 리드 모델로 진행됩니다
- **후속 턴**은 워커 모델을 사용합니다
- **폴백**은 워커 모델이 연속으로 너무 많이 실패하면 시작됩니다
- **복구**는 상황이 안정되면 세션을 워커 모델로 되돌립니다


## 빠른 예시

goose를 다음과 같이 구성할 수 있습니다:

```bash
export GOOSE_LEAD_MODEL="gpt-4o"          # 강력한 추론
export GOOSE_MODEL="gpt-4o-mini"          # 빠른 실행
export GOOSE_PROVIDER="openai"
```

goose는 처음 세 턴 동안 `gpt-4o`로 시작한 다음 `gpt-4o-mini`로 넘깁니다. 워커가 연속으로 두 번 문제를 겪으면 goose는 일시적으로 두 번의 폴백 턴 동안 리드 모델로 전환한 후 워커를 다시 시도합니다.

## 구성

:::tip
goose에 [LLM을 추가](/docs/getting-started/providers)했는지 확인하세요
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
   1. goose 데스크톱 창 하단의 모델 이름을 클릭합니다
   2. **Lead/Worker Settings**를 클릭합니다
   3. **Enable lead/worker mode** 체크박스를 선택합니다
   4. 드롭다운 메뉴에서 **Lead Model**과 **Worker Model**을 선택합니다
   5. (선택 사항) 기본 **초기 리드 턴** 수, 리드 모델로 전환하기 전 **실패 임계값**, 또는 폴백 시 리드 모델을 사용하는 **폴백 턴** 수를 변경합니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    필요한 유일한 구성은 `GOOSE_LEAD_MODEL` [환경 변수](/docs/guides/environment-variables#leadworker-model-configuration)를 설정하는 것입니다:

    ```bash
    export GOOSE_LEAD_MODEL="gpt-4o"
    ```
    그게 다입니다. goose는 기본적으로 일반 `GOOSE_MODEL`을 워커 모델로 처리합니다.

    더 많은 제어를 위해 다음 선택적 환경 변수를 설정할 수도 있습니다:

    ```bash
    export GOOSE_LEAD_PROVIDER="anthropic"         # 메인 프로바이더와 다른 경우
    export GOOSE_LEAD_TURNS=5                      # 처음 5턴 동안 리드 모델 사용
    export GOOSE_LEAD_FAILURE_THRESHOLD=3          # 3번 실패 후 리드로 전환
    export GOOSE_LEAD_FALLBACK_TURNS=2             # 워커를 다시 시도하기 전 2턴 동안 리드 모델 사용
    ```
    이러한 구성을 수행한 후 새 CLI 및 데스크톱 세션에서 리드/워커 모델이 사용됩니다.
    </TabItem>
</Tabs>

## 무엇이 실패로 간주되나요?

goose는 API 오류가 아닌 실제 작업 실패를 감지하는 데 똑똑합니다. 워커가 다음을 수행할 때 폴백이 시작됩니다:

- 손상된 코드 생성 (구문 오류, 도구 실패, 누락된 파일)
- 권한 문제 발생
- 사용자에 의해 수정됨 ("틀렸어", "다시 시도해 봐" 등)

타임아웃, 인증 문제 또는 서비스 다운타임과 같은 기술적 문제는 폴백 모드를 트리거하지 않습니다. goose는 조용히 이를 재시도합니다.

## 리드/워커를 사용하는 이유

- 일상적인 실행에 저렴한 모델을 사용하여 **비용 절감**
- 더 강력한 모델에서 견고한 계획을 얻으면서 **속도 향상**
- **프로바이더 혼합** (예: 추론에 Claude, 실행에 OpenAI)
- 모델 피로나 성능 걱정 없이 **긴 개발 세션 처리**

## 모범 사례

이제 막 시작하는 경우 기본 설정이 잘 작동합니다. 하지만 조정하는 방법은 다음과 같습니다:

- 더 많은 계획이 필요한 경우 `GOOSE_LEAD_TURNS`를 5-7로 늘리세요
- goose가 문제를 빠르게 수정하도록 하려면 `GOOSE_LEAD_FAILURE_THRESHOLD`를 1로 낮추세요
- 일상적인 작업에는 빠르고 가벼운 워커 모델(Claude Haiku, GPT-4o-mini)을 선택하세요

디버깅을 위해 이 로그를 켜서 모델 전환 동작을 볼 수 있습니다:

```bash
export RUST_LOG=goose::providers::lead_worker=info
```

## 계획 모드 호환성
리드/워커 모델은 [goose CLI의 `/plan` 명령](/docs/guides/creating-plans)에 대한 자동 대안입니다. 리드/워커 및 계획 모델로 사용할 별도의 모델을 할당할 수 있습니다. 예:

```bash
export GOOSE_PROVIDER="openai"
export GOOSE_MODEL="gpt-4o-mini"        # 메인 대화 모델

export GOOSE_LEAD_MODEL="o1-preview"    # 자동으로 사용되는 리드 모델
export GOOSE_PLANNER_MODEL="gpt-4o"     # /plan을 명시적으로 호출할 때 사용되는 모델
```

실행 전에 검토하고 승인할 수 있는 포괄적인 전략을 생성하기 위해 전용 추론 모델이 필요할 때 **계획 모드**를 사용하세요. 기능 구현, 문제 디버깅 또는 탐색적 코딩과 같이 중단 없이 스마트한 자동화가 필요한 반복적인 개발 작업에는 **리드/워커 모델**을 사용하세요. 워크플로우는 둘 다 결합할 수 있습니다: 주요 결정을 전략화하기 위해 `/plan`을 사용한 다음 리드/워커 모델이 자동 최적화와 함께 전술적 구현을 처리하게 하세요.
