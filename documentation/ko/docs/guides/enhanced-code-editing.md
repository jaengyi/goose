---
title: AI 모델을 활용한 향상된 코드 편집
sidebar_label: 향상된 코드 편집
description: AI 모델을 사용하여 지능적으로 코드 변경 적용하기
sidebar_position: 110
---

[Developer 확장 기능](/docs/mcp/developer-mcp)은 `str_replace` 명령을 통해 향상된 코드 편집을 위한 AI 모델 사용을 지원합니다. 구성되면 단순 문자열 교체 대신 AI 모델을 사용하여 지능적으로 코드 변경을 적용합니다.

코드 편집에 특화된 모델을 사용하면 주요 LLM 공급자의 부하를 줄이면서 정확도, 품질, 속도를 높이고 비용을 낮출 수 있습니다. 이 향상된 접근 방식은 다음을 제공합니다:

- **컨텍스트 인식 편집**: AI가 코드 구조를 이해하고 더 지능적인 변경을 할 수 있습니다
- **더 나은 포맷팅**: 일관된 코드 스타일과 포맷팅을 유지합니다
- **오류 방지**: 편집 중 잠재적인 문제를 감지하고 수정할 수 있습니다
- **유연한 모델 지원**: 모든 OpenAI 호환 API에서 작동합니다
- **깔끔한 구현**: 구성 검사에 예외 처리 대신 적절한 제어 흐름을 사용합니다

## 구성

AI 기반 코드 편집을 활성화하려면 다음 [환경 변수](/docs/guides/environment-variables#enhanced-code-editing)를 설정하세요:

```bash
export GOOSE_EDITOR_API_KEY="your-api-key-here"
export GOOSE_EDITOR_HOST="https://api.openai.com/v1"
export GOOSE_EDITOR_MODEL="gpt-4o"
```

**기능을 활성화하려면 세 가지 환경 변수가 모두 설정되고 비어 있지 않아야 합니다.**

이 선택적 기능은 완전히 하위 호환됩니다 - 구성되지 않으면 확장 기능은 이전과 동일하게 단순 문자열 교체로 작동합니다.

### 지원되는 공급자

모든 OpenAI 호환 API 엔드포인트가 작동해야 합니다. 예시:

**OpenAI:**
```bash
export GOOSE_EDITOR_API_KEY="sk-..."
export GOOSE_EDITOR_HOST="https://api.openai.com/v1"
export GOOSE_EDITOR_MODEL="gpt-4o"
```

**Anthropic (OpenAI 호환 프록시 통해):**
```bash
export GOOSE_EDITOR_API_KEY="sk-ant-..."
export GOOSE_EDITOR_HOST="https://api.anthropic.com/v1"
export GOOSE_EDITOR_MODEL="claude-3-5-sonnet-20241022"
```

**Morph:**
```bash
export GOOSE_EDITOR_API_KEY="sk-..."
export GOOSE_EDITOR_HOST="https://api.morphllm.com/v1"
export GOOSE_EDITOR_MODEL="morph-v0"
```

**Relace:**
```bash
export GOOSE_EDITOR_API_KEY="rlc-..."
export GOOSE_EDITOR_HOST="https://instantapply.endpoint.relace.run/v1/apply"
export GOOSE_EDITOR_MODEL="auto"
```

**로컬/사용자 정의 엔드포인트:**
```bash
export GOOSE_EDITOR_API_KEY="your-key"
export GOOSE_EDITOR_HOST="http://localhost:8000/v1"
export GOOSE_EDITOR_MODEL="your-model"
```

## 작동 방식

`str_replace` 도구를 사용하여 코드를 편집할 때:

1. **구성 검사**: goose는 세 가지 환경 변수가 모두 올바르게 설정되고 비어 있지 않은지 확인합니다.

2. **AI 활성화 시**: 구성되면 goose는 원본 코드와 요청한 변경 사항을 구성된 AI 모델로 보내 처리합니다.

3. **폴백**: AI API가 구성되지 않았거나 API 호출이 실패하면 단순 문자열 교체로 대체됩니다.

4. **사용자 피드백**: AI 구성 없이 `str_replace`를 처음 사용하면 기능 활성화 방법을 설명하는 유용한 메시지가 표시됩니다.
