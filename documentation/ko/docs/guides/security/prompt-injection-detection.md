---
sidebar_position: 1
title: 프롬프트 인젝션 감지
sidebar_label: 프롬프트 인젝션 감지
description: 잠재적으로 유해한 명령이 실행되기 전에 감지하여 워크플로우를 보호하세요.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Settings } from 'lucide-react';

프롬프트 인젝션은 실행 가능한 콘텐츠 내에 악성 지침이 숨겨져 있을 때 발생합니다. AI 세계에서 프롬프트 인젝션은 AI 에이전트(goose와 같은)가 환경이나 데이터를 손상시킬 수 있는 안전하지 않은 명령을 실행하도록 유도하는 데 사용될 수 있습니다.

프롬프트 인젝션 감지를 활성화하여 goose 워크플로우를 보호할 수 있습니다. 이 기능은 패턴 매칭을 사용하여 다음과 같은 일반적인 공격 기술을 감지합니다:
- 시스템 파일이나 디렉토리 삭제 시도
- 원격 스크립트를 다운로드하고 실행하는 명령
- SSH 키와 같은 민감한 데이터에 액세스하거나 유출하려는 시도
- 보안을 손상시킬 수 있는 시스템 수정

또한 지정된 모델을 사용하여 선택적으로 [ML 기반 스캔](#머신-러닝을-통한-향상된-감지)을 활성화할 수 있습니다.

:::important
이러한 검사는 보호 장치를 제공하지만 보장은 아닙니다. 알려진 패턴을 감지하지만 모든 가능한 위협, 특히 새롭거나 정교한 공격을 포착할 수는 없습니다.
:::

## 감지 작동 방식

활성화되면 goose는 위협이 실행되기 전에 다층 접근 방식을 사용하여 감지합니다:

1. **도구 호출이 가로채지고 분석됨** - goose가 도구를 실행하려고 준비할 때 보안 시스템은 도구 매개변수 텍스트를 추출하고 [위협 패턴](https://github.com/block/goose/blob/main/crates/goose/src/security/patterns.rs)과 대조합니다. ML 기반 감지가 활성화된 경우 머신 러닝을 사용하여 도구 호출과 최근 대화 메시지의 의미적 콘텐츠를 분석하여 컨텍스트를 더 잘 이해하고 오탐을 줄입니다.
2. **위험 평가** - 감지된 위협에 신뢰도 점수가 할당됨
3. **실행 일시 중지** - 구성된 임계값을 초과하는 위협은 사용자의 결정이 필요함
4. **보안 경고 표시** - 경고에는 신뢰 수준, 발견 사항 설명 및 고유 발견 ID가 표시됩니다. 예:
   ```
   🔒 Security Alert: This tool call has been flagged as potentially dangerous.

   Confidence: 95%
   Explanation: Detected 1 security threat: Recursive file deletion with rm -rf
   Finding ID: SEC-abc123...

   [Allow Once] [Deny]
   ```
5. **사용자가 선택** - 경고 세부 정보를 검토한 후 진행하거나 취소할 수 있습니다. 참고:
   - 각 결정은 발견 ID와 함께 [goose 시스템 로그](/docs/guides/logs#system-logs)에 기록됩니다
   - 허용된 명령은 여전히 전체 권한으로 실행됩니다

**경고에 대응하기:**

- 감지를 트리거한 내용을 이해하려면 설명을 읽으세요
- 컨텍스트를 고려하세요&mdash;수행하려는 작업과 일치합니까?
- 요청을 더 구체적으로 다시 표현해 보세요
- 소스를 확인하고 알 수 없는 소스의 프롬프트에는 특히 주의하세요

의심스러우면 거부하세요.

## 감지 활성화

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Settings`를 클릭합니다
    3. `Chat` 탭을 클릭합니다
    4. `Enable Prompt Injection Detection`을 켭니다
    5. 선택적으로 `Detection Threshold`를 조정하여 [감도를 구성](#감지-임계값-구성)합니다
    6. 선택적으로 ML 기반 감지를 활성화합니다:
       1. `Enable ML-based Detection`을 켭니다
       2. 추론 엔드포인트를 구성합니다:
          - `Endpoint URL`: 분류 서비스 URL(예: Hugging Face)
          - `API Token`: 서비스에서 필요한 경우 인증 토큰

  </TabItem>
  <TabItem value="config" label="goose 구성 파일">

    [`config.yaml`](/docs/guides/config-files)에 보안 프롬프트 설정을 추가합니다:

    ```yaml
    SECURITY_PROMPT_ENABLED: true
    SECURITY_PROMPT_THRESHOLD: 0.8  # 선택사항, 기본값은 0.8

    # 선택사항: ML 기반 감지 활성화 (Hugging Face 예시)
    SECURITY_PROMPT_CLASSIFIER_ENABLED: true
    SECURITY_PROMPT_CLASSIFIER_ENDPOINT: "https://router.huggingface.co/hf-inference/models/protectai/deberta-v3-base-prompt-injection-v2"
    SECURITY_PROMPT_CLASSIFIER_TOKEN: "YOUR_HUGGING_FACE_TOKEN"
    ```

  </TabItem>
</Tabs>

:::info 기타 보안 기능
프롬프트 인젝션 감지 외에도 goose는 자동으로:
- 새롭거나 업데이트된 레시피를 실행하기 전에 경고합니다
- 보이지 않는 유니코드 태그 블록 문자가 포함된 레시피를 가져올 때 경고합니다
- 로컬 실행 MCP 서버용 확장 기능을 설치할 때 [알려진 악성코드를 확인](/docs/troubleshooting/known-issues#malicious-package-detected)합니다
:::

### 감지 임계값 구성

임계값(0.01-1.0)은 감지가 얼마나 엄격한지 제어합니다:

| 임계값 | 감도 | 사용 시기 |
|-----------|------------|----------|
| **0.01-0.50** | 매우 관대 | 경험이 풍부하고 위험을 이해하는 경우 |
| **0.50-0.70** | 균형 | 일반 개발 작업 (좋은 기본값) |
| **0.70-0.90** | 엄격 | 민감한 데이터나 시스템으로 작업하는 경우 |
| **0.90-1.00** | 최대 | 고보안 환경 |

인젝션 프롬프트 감지 기능이 활성화되면 기본 임계값은 0.8입니다(대부분의 사용자에게 권장).

낮은 임계값은 경고가 적지만 위협을 놓칠 수 있습니다. 높은 임계값은 더 많은 잠재적 문제를 포착하지만 합법적인 작업을 표시할 수 있습니다. 필요에 따라 이 감도/편의성 균형을 제어할 수 있습니다.

## 머신 러닝을 통한 향상된 감지

기본적으로 프롬프트 인젝션 감지는 패턴 매칭을 사용하지만 향상된 정확도와 더 적은 오탐을 위해 선택적으로 ML 기반 감지를 활성화할 수 있습니다.

ML 기반 감지:
- 도구 호출과 최근 메시지의 의미적 콘텐츠 분석
- 패턴이 놓칠 수 있는 정교한 공격 감지
- 대화 컨텍스트를 이해하여 오탐 감소
- 분류 엔드포인트 URL 및 API 토큰(필요한 경우) 제공 필요

:::warning 개인정보 고려사항
ML 기반 감지가 활성화되면 도구 호출 콘텐츠와 최근 메시지가 분석을 위해 구성된 엔드포인트로 전송됩니다.
:::

#### ML 감지 엔드포인트 자체 호스팅
자체 분류 엔드포인트를 실행하려면 구현 세부 정보는 [분류 API 사양](/docs/guides/security/classification-api-spec)을 참조하세요. API는 Hugging Face Inference API 형식을 따릅니다.

## 참조

- [goose 권한 모드](/docs/guides/goose-permissions) - goose의 자율성 수준 제어
- [도구 권한 관리](/docs/guides/managing-tools/tool-permissions) - 세분화된 도구 제어
