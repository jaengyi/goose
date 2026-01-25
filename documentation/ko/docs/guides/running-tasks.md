---
sidebar_position: 75
title: 작업 실행
sidebar_label: 작업 실행
---

goose CLI로 작업할 때 `goose run` 명령에 파일과 지침을 전달하여 작업과 워크플로우를 실행할 수 있습니다. 이는 간단한 한 줄 명령이거나 파일에 저장된 복잡한 지침 세트일 수 있습니다.

## 기본 사용법

`goose run` 명령은 새 세션을 시작하고 제공된 인수를 사용하여 실행을 시작하며 작업이 완료되면 자동으로 세션을 종료합니다.

goose로 작업을 실행하는 여러 방법이 있습니다. [옵션 목록](/docs/guides/goose-cli-commands.md#run-options)을 확인하세요.

### 명령의 텍스트
```bash
goose run -t "your instructions here"
```

`-t` 플래그를 사용하면 명령에 직접 텍스트 지침을 전달할 수 있습니다. 이는 goose와의 대화형 세션이 필요하지 않은 빠른 일회성 명령에 적합합니다. 지침이 실행되고 세션이 종료됩니다. 예시 사용 사례로는 CI/CD 파이프라인에서 사용하거나 다른 스크립트와 함께 실행하는 것이 있습니다.

### 지침 파일 사용
자동화하려는 복잡한 지침 세트나 워크플로우가 있는 경우 파일에 저장하고 `goose run` 명령에 전달할 수 있습니다:

```bash
goose run -i instructions.md
```

다음은 프로젝트 종속성에 대한 보안 감사를 실행하는 지침 파일의 예입니다:

```md
# 종속성 보안 감사

1. 프로젝트 종속성 분석:
   - package.json 및 requirements.txt 파일 확인
   - 버전이 포함된 모든 종속성 나열
   - 오래된 패키지 식별

2. 보안 검사:
   - npm audit 실행(JavaScript 패키지용)
   - Python 패키지의 알려진 취약점 확인
   - 중요한 보안 문제가 있는 종속성 식별

3. 업그레이드 계획 작성:
   - 즉시 업데이트가 필요한 패키지 나열
   - 최신 버전의 주요 변경 사항 기록
   - 필요한 업데이트의 영향 추정

심각도 수준이 강조된 'security_audit.md'에 결과 저장.
```

### stdin 사용
`-i -`를 통해 표준 입력을 사용하여 goose에 지침을 전달할 수도 있습니다. 이는 다른 도구나 스크립트의 명령을 goose로 파이프하려는 경우 유용합니다.

#### 간단한 echo 파이프

```bash
echo "What is 2+2?" | goose run -i -
```

#### 여러 줄 지침
```bash
cat << EOF | goose run -i -
Please help me with these tasks:
1. Calculate 15% of 85
2. Convert 32°C to Fahrenheit
EOF
```

## 주요 기능

### 대화형 모드

작업 끝에 goose가 종료되지 않게 하려면 `-s` 또는 `--interactive` 플래그를 전달하여 초기 명령을 처리한 후 대화형 세션을 시작할 수 있습니다:

```bash
goose run -i instructions.txt -s
```

이는 초기 명령이 처리된 후 goose와 계속 작업하려는 경우 유용합니다.

### 세션 관리

세션에 이름을 지정하고 관리할 수 있습니다:

```bash
# 새로운 이름이 지정된 세션 시작
goose run -n my-project -t "initial instructions"

# 이전 세션 재개
goose run -n my-project -r
```

`--no-session` 플래그를 사용하여 세션 파일을 만들거나 저장하지 않고 명령을 실행할 수도 있습니다. 이는 대화 기록이나 상태를 유지할 필요가 없는 자동화된 스크립트나 일회성 작업에 유용합니다. 이 플래그는 세션 출력을 임시 null 경로(Unix의 `/dev/null` 또는 Windows의 `NUL`)로 라우팅하고 완료 시 삭제합니다.

```bash
# 세션 파일을 만들지 않고 명령 실행
goose run --no-session -t "your command here"
```
### 공급자 및 모델 설정
[환경 변수](/docs/guides/environment-variables.md)의 공급자 및 모델 설정을 재정의하여 특정 공급자 및 모델로 goose 세션을 실행할 수 있습니다.

```bash
goose run --provider anthropic --model claude-4-sonnet -t "initial prompt"
```

### 확장 기능 작업

작업을 실행할 때 특정 확장 기능을 사용할 수 있도록 하려면 인수로 표시할 수 있습니다. 이는 `--with-extension`, `--with-remote-extension`, `--with-streamable-http-extension` 또는 `--with-builtin` 플래그를 사용하여 수행할 수 있습니다:

- 내장 확장 기능 사용 예: developer 및 computercontroller 확장 기능

```bash
goose run --with-builtin "developer,computercontroller" -t "your instructions"
```

- 사용자 정의 확장 기능 사용

```bash
goose run --with-extension "ENV1=value1 custom-extension-args" -t "your instructions"
```

- 스트리밍 가능한 HTTP 확장 기능 사용

```bash
goose run --with-streamable-http-extension "https://example.com/streamable" -t "your instructions"
```

### 디버그 모드

복잡한 워크플로우를 문제 해결하거나 개발할 때 도구 실행에 대한 더 자세한 정보를 얻기 위해 디버그 모드를 활성화할 수 있습니다. `--debug` 플래그는 다음을 제공합니다:

- 완전한 도구 응답
- 상세한 파라미터 값
- 전체 파일 경로

디버그 모드는 다음과 같은 경우에 유용합니다:
- 새로운 자동화 스크립트 개발
- 확장 기능 동작 문제 해결
- 도구 파라미터 및 응답 확인

```bash
# 디버그 출력이 활성화된 작업 실행
goose run --debug -t "your instructions"

# 레시피 실행 디버그
goose run --debug --recipe recipe.yaml
```

### JSON 출력 형식

자동화, 스크립팅 및 CI/CD 통합을 위해 `--output-format` 플래그를 사용하여 `goose run`에서 구조화된 출력을 얻을 수 있습니다:

- `json` - 실행 완료 후 완전한 JSON 출력(CI 파이프라인, 로깅에 최적)
- `stream-json` - 이벤트가 발생할 때 실시간 구조화된 출력(진행 모니터링, 장시간 실행 작업에 최적)

```bash
# 자동화를 위해 JSON 출력으로 실행
goose run --output-format json -t "your instructions"

# 실시간으로 JSON 이벤트 스트리밍
goose run --output-format stream-json -t "your instructions"

# JSON 출력으로 레시피 실행
goose run --output-format json --recipe recipe.yaml

# 다른 옵션과 결합
goose run --output-format json --no-session -t "automated task"
```

JSON 출력에는 다음이 포함됩니다:
- 세션 메타데이터 및 실행 결과
- 도구 출력 및 오류
- 스크립트 및 CI/CD 파이프라인에서 파싱하기에 적합한 구조화된 데이터

## 일반적인 사용 사례

### 스크립트 파일 실행

지침 파일 생성(예: `build-script.txt`):
```text
Check the current branch
Run the test suite
Build the documentation
```

그런 다음 실행:
```bash
goose run -i build-script.txt
```

### 빠른 명령

일회성 명령의 경우 텍스트 옵션 사용:
```bash
goose run -t "Create a CHANGELOG.md entry comparing current git branch with main"
```

### 개발 워크플로우

특정 확장 기능으로 세션 시작:
```bash
goose run --with-builtin "developer,git" -n dev-session -s
```

### 옵션 결합

여러 옵션을 결합하여 강력한 워크플로우를 만들 수 있습니다:

```bash
# 여러 옵션을 결합한 복잡한 예시
goose run \
  --with-builtin "developer,git" \
  --with-extension "API_KEY=xyz123 custom-tool" \
  -n project-setup \
  -t "Initialize project"
```

이 명령은:
1. developer 및 git 내장 확장 기능 로드
2. API 키가 있는 사용자 정의 확장 기능 추가
3. 세션 이름을 "project-setup"으로 지정
4. "Initialize project" 지침으로 시작
5. 명령 처리 후 자동으로 종료.
