---
description: goose와 Langfuse를 통합하여 성능 관찰하기
---

# Langfuse로 관찰성 확보

이 튜토리얼에서는 goose 요청을 모니터링하고 에이전트가 어떻게 수행되는지 이해하기 위해 goose를 Langfuse와 통합하는 방법을 다룹니다.

## Langfuse란?

[Langfuse](https://langfuse.com/)는 팀이 LLM 애플리케이션을 협업하여 모니터링, 평가 및 디버깅할 수 있게 해주는 [오픈 소스](https://github.com/langfuse/langfuse) LLM 엔지니어링 플랫폼입니다.


## Langfuse 설정

[여기](https://cloud.langfuse.com)에서 Langfuse Cloud에 가입하거나 [Docker Compose](https://langfuse.com/self-hosting/local)로 Langfuse를 셀프 호스팅하여 Langfuse API 키를 받으세요.

## goose를 Langfuse에 연결하도록 구성

goose(Rust로 작성됨)가 Langfuse 서버에 연결할 수 있도록 환경 변수를 설정합니다.

```bash
export LANGFUSE_INIT_PROJECT_PUBLIC_KEY=pk-lf-...
export LANGFUSE_INIT_PROJECT_SECRET_KEY=sk-lf-...
export LANGFUSE_URL=https://cloud.langfuse.com # EU 데이터 리전 🇪🇺

# 미국 리전을 사용하는 경우 https://us.cloud.langfuse.com 🇺🇸
# 셀프 호스팅하는 경우 https://localhost:3000
```

## Langfuse 통합으로 goose 실행

이제 goose를 실행하고 Langfuse를 통해 AI 요청 및 작업을 모니터링할 수 있습니다.

goose가 실행 중이고 환경 변수가 설정되면 Langfuse가 goose 활동의 추적을 캡처하기 시작합니다.

_[Langfuse의 예제 추적 (공개)](https://cloud.langfuse.com/project/cloramnkj0002jz088vzn1ja4/traces/cea4ed38-0c44-4b0a-8c20-4b0b6b9e8d73?timestamp=2025-01-31T15%3A52%3A30.362Z&observation=7c8e5807-3c29-4c28-9c6f-7d7427be401f)_

![Langfuse의 goose 추적](https://langfuse.com//images/docs/goose-integration/goose-example-trace.png)
