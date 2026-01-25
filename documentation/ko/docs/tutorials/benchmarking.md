---
title: goose로 벤치마킹하기
sidebar_label: goose로 벤치마크
---

goose 벤치마킹 시스템을 사용하면 하나 이상의 시스템 구성에서 복잡한 작업에 대한 goose 성능을 평가할 수 있습니다.<br></br>
이 가이드에서는 `goose bench` 명령을 사용하여 벤치마크를 실행하고 결과를 분석하는 방법을 다룹니다.

### 빠른 시작

1. 벤치마킹 시스템에는 여러 평가 스위트가 포함되어 있습니다.<br></br>
   다음을 실행하여 모든 유효한 선택자 목록을 확인하세요:

```bash
goose bench selectors
```

2. 기본 구성 파일 생성:

```bash
goose bench init-config -n bench-config.json
cat bench-config.json
{
  "models": [
    {
      "provider": "databricks",
      "name": "goose",
      "parallel_safe": true
    }
  ],
  "evals": [
    {
      "selector": "core",
      "parallel_safe": true
    }
  ],
  "repeat": 1
}
...등등.
```

2. 벤치마크 실행:

```bash
goose bench run -c bench-config.json
```

## 구성 파일

벤치마크 구성은 다음 구조의 JSON 파일로 지정됩니다:

```json
{
  "models": [
    {
      "provider": "databricks",
      "name": "goose",
      "parallel_safe": true,
      "tool_shim": {
        "use_tool_shim": false,
        "tool_shim_model": null
      }
    }
  ],
  "evals": [
    {
      "selector": "core",
      "post_process_cmd": null,
      "parallel_safe": true
    }
  ],
  "include_dirs": [],
  "repeat": 2,
  "run_id": null,
  "eval_result_filename": "eval-results.json",
  "run_summary_filename": "run-results-summary.json",
  "env_file": null
}
```

### 구성 옵션

#### Models 섹션

`models` 배열의 각 모델 항목은 다음을 지정합니다:

- `provider`: 모델 프로바이더 (예: "databricks")
- `name`: 모델 식별자
- `parallel_safe`: 모델을 병렬로 실행할 수 있는지 여부
- `tool_shim`: 도구 심(shim) 구성 (선택 사항)
    - `use_tool_shim`: 도구 심 활성화/비활성화
    - `tool_shim_model`: 도구 심에 사용할 모델 (선택 사항)

#### Evals 섹션

`evals` 배열의 각 평가 항목은 다음을 지정합니다:

- `selector`: 실행할 평가 스위트 (예: "core")
- `post_process_cmd`: 후처리 스크립트 경로 (선택 사항)
- `parallel_safe`: 평가를 병렬로 실행할 수 있는지 여부

#### 일반 옵션

- `include_dirs`: 평가에 포함할 추가 디렉토리
- `repeat`: 각 평가를 반복할 횟수
- `run_id`: 벤치마크 실행의 식별자 (선택 사항)
- `eval_result_filename`: 평가 결과 파일 이름
- `run_summary_filename`: 요약 결과 파일 이름
- `env_file`: 환경 파일 경로 (선택 사항)

##### include_dirs 옵션의 메커니즘

`include_dirs` 구성 매개변수는 옵션 내에 나열된 모든 경로의 항목을 모든 평가에서 사용할 수 있게 합니다.<br></br>
다음과 같이 작동합니다:

* 포함된 각 에셋을 각 모델/프로바이더 쌍에 대해 생성된 최상위 디렉토리에 복사
* 평가 실행 시
    * 평가에서 명시적으로 요구하는 에셋이 평가별 디렉토리에 복사됨
    * 평가 코드가 명시적으로 가져오는 경우에만
    * 구성된 선택자 중 하나에 해당하여 평가가 실제로 실행되는 경우에만

### 평가 커스터마이징

여러 가지 방법으로 실행을 커스터마이징할 수 있습니다:

1. 평가 후 후처리 명령 사용:

```json
{
  "evals": [
    {
      "selector": "core",
      "post_process_cmd": "/path/to/process-script.sh",
      "parallel_safe": true
    }
  ]
}
```

2. 추가 데이터 포함:

```json
{
  "include_dirs": [
    "/path/to/custom/eval/data"
  ]
}
```

3. 환경 변수 설정:

```json
{
  "env_file": "/path/to/env-file"
}
```

## 출력 및 결과

벤치마크는 다음과 유사한 파일 계층 구조 내에 두 개의 주요 출력 파일을 생성합니다.<br></br>
각 모델/프로바이더 쌍의 실행 결과는 자체 디렉토리에 저장됩니다:

```bash
benchmark-${datetime}/
  ${model}-${provider}[-tool-shim[-${shim-model}]]/
    run-${i}/
      ${an-include_dir-asset}
      run-results-summary.json
      core/developer/list_files/
        ${an-include_dir-asset}
        run-results-summary.json
```

1. `eval-results.json`: 다음을 포함한 각 평가의 상세 결과:
    - 개별 테스트 케이스 결과
    - 모델 응답
    - 점수 메트릭
    - 오류 로그

2. `run-results-summary.json`: 모든 스위트의 모든 평가 결과 모음.

### 디버그 모드

상세 로깅을 위해 디버그 모드를 활성화할 수 있습니다:

```bash
RUST_LOG=debug goose bench bench-config.json
```

## 고급 사용법

### 도구 심(Tool Shimming)

도구 심을 사용하면 Ollama가 시스템에 설치된 경우 도구 기능이 없는 모델을 goose와 함께 사용할 수 있습니다.

[도구 심](/docs/experimental/ollama)에 대한 중요한 세부 사항은 이 가이드를 참조하세요.
