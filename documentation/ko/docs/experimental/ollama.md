---
title: Ollama Tool Shim
sidebar_position: 2
sidebar_label: Ollama Tool Shim
---

Ollama tool shim은 도구 호출을 기본적으로 지원하지 않는 언어 모델(예: DeepSeek)에 도구 호출 기능을 활성화합니다.

:::warning 실험적 기능
Ollama tool shim은 실험적 기능입니다. 동작과 구성은 향후 릴리스에서 변경될 수 있습니다.
:::

tool shim은 주요 모델에 의도된 도구 사용을 위해 json을 출력하도록 지시하는 방식으로 작동합니다. 인터프리터 모델은 ollama 구조화된 출력을 사용하여 주요 모델의 메시지를 유효한 json으로 변환한 다음, 해당 json이 호출할 유효한 도구 호출로 변환됩니다.


#### Ollama Tool Shim 사용 방법

1. [Ollama](https://ollama.com/download)가 설치되어 실행 중인지 확인
2. 기본 인터프리터 모델은 `mistral-nemo`입니다. 이것을 사용하려면 다음을 실행하여 ollama 서버에서 가져와야 합니다:

   ```bash
   ollama pull mistral-nemo
   ```
3. 다른 모델을 사용하려면 먼저 Ollama 서버에서 가져와야 합니다. 그런 다음 `GOOSE_TOOLSHIM_OLLAMA_MODEL` 환경 변수를 사용하여 기본 인터프리터 모델을 재정의합니다. 예를 들어, `llama3.2` 모델을 사용하려면 다음을 실행합니다:

   ```bash
   ollama pull llama3.2
   ```
   그런 다음,

   ```bash
   GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
   ```

4. 최적의 성능을 위해 증가된 컨텍스트 길이로 Ollama 서버를 실행합니다:
   ```bash
   OLLAMA_CONTEXT_LENGTH=32768 ollama serve
   ```

5. `GOOSE_TOOLSHIM` 환경 변수를 설정하여 tool shim을 활성화합니다:

   ```bash
   GOOSE_TOOLSHIM=1
   ```

tool shim 기본 설정으로 새 goose 세션을 시작합니다:

  ```bash
  GOOSE_TOOLSHIM=1 GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2 cargo run --bin goose session
  ```
