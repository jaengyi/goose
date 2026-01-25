---
title: 코드베이스 분석
sidebar_label: 코드베이스 분석
description: 의미론적 분석 및 호출 그래프로 코드베이스 이해하기
sidebar_position: 111
---

[Developer 확장 기능](/docs/mcp/developer-mcp)에는 코드베이스 전체에서 코드 구조를 이해하고, 심볼 사용을 추적하고, 호출 그래프를 탐색하는 데 도움이 되는 `analyze` 도구가 포함되어 있습니다. Developer 확장 기능이 활성화되면 자동으로 사용할 수 있으며 [여러 프로그래밍 언어](https://github.com/block/goose/blob/main/crates/goose-mcp/src/developer/analyze/languages/mod.rs)의 파일 유형을 지원합니다.

<details>
<summary>분석 예시: 파일 간 함수 추적</summary>

**프롬프트:**

"authenticate 함수의 호출 그래프를 보여줘"

**출력:**

:::info 데스크톱
코드베이스에서 `authenticate` 함수가 어떻게 사용되는지 분석하겠습니다.

분석 결과, 코드베이스에서 `authenticate` 함수가 다음과 같이 사용됩니다:

#### 함수 정의
`authenticate` 함수는 `auth.py`의 15번째 줄에 정의되어 있습니다. username과 password 매개변수를 받아 사용자 인증을 처리합니다.

#### 수신 호출 (authenticate를 호출하는 함수)
- **login_handler()** `api.py:25` - 로그인 요청을 처리하는 API 엔드포인트
- **admin_login()** `admin.py:102` - 관리자 사용자를 위한 특별 인증 흐름
- **test_auth()** `tests/test_auth.py:45` - 인증 테스트 케이스

#### 발신 호출 (authenticate가 호출하는 함수)
- **validate_credentials()** `db.py:102` - 데이터베이스에서 자격 증명 확인
- **log_attempt()** `logger.py:45` - 인증 시도 기록
- **check_rate_limit()** `security.py:78` - 무차별 대입 공격 방지

이는 `authenticate`가 인증 흐름의 중심 함수이며, 여러 진입점에서 호출되고 데이터베이스 검증, 로깅 및 보안 검사에 의존함을 보여줍니다.
:::

</details>

## 분석 모드

`analyze` 도구는 디렉토리, 파일 또는 심볼을 분석하는지에 따라 구조, 의미론적, 집중의 세 가지 모드로 작동합니다. 자연어 또는 [매개변수](#공통-매개변수)를 사용한 직접 명령을 통해 호출할 수 있습니다.

### 프로젝트 구성 이해하기

디렉토리를 분석하여 코드베이스의 구조적 개요를 얻으세요—프로젝트 구성을 이해하고, 큰 파일을 식별하고, 코드베이스 메트릭을 확인합니다.

**자연어:**
- "src/ 디렉토리의 구조를 분석해줄 수 있어?"
- "이 프로젝트의 코드 구조 개요를 알려줘"
- "이 Python 프로젝트의 주요 진입점은 뭐야?"

**직접 명령:**
```bash
# 기본 깊이(3 레벨)로 개요 얻기
analyze path="src/"

# 2개의 하위 디렉토리 레벨로 제한된 개요 얻기
analyze path="." max_depth=2
```

### 파일 검사

단일 파일에 대한 의미론적 세부 정보를 얻으세요—함수, 클래스 및 import를 확인하여 구조를 이해하고 특정 구현을 찾습니다.

**자연어:**
- "main.py에 어떤 함수들이 있어?"
- "src/utils.py의 구조를 보여줘"

**직접 명령:**
```bash
# 파일 세부 정보 얻기
analyze path="main.py"

# 특정 파일 분석
analyze path="src/utils.py"
```

### 파일 간 심볼 추적

특정 함수, 클래스 또는 메서드에 집중하여 정의된 위치와 파일 간에 호출되는 방식을 확인하세요—리팩토링 및 디버깅에 유용합니다.

**자연어:**
- "authenticate 함수의 의존성을 추적해줘"
- "UserClass의 호출 그래프를 보여줘"

**직접 명령:**
```bash
# 함수 사용 추적
analyze path="src/" focus="authenticate"

# 더 깊은 호출 체인으로 추적
analyze path="." focus="UserClass" follow_depth=3
```

## 공통 매개변수

| 매개변수 | 기본값 | 설명 |
|-----------|---------|-------------|
| `path` | None (필수) | 분석할 파일 또는 디렉토리의 절대 또는 상대 경로 |
| `focus` | None | 추적할 심볼 이름. 파일 간 추적의 경우 `path`는 디렉토리여야 합니다. |
| `follow_depth` | 2 | 집중된 심볼에서 추적할 단계 수(0=정의된 위치, 1=직접 호출자/피호출자, 2=그들의 호출자/피호출자 등). `focus` 매개변수와 함께 사용됩니다. |
| `max_depth` | 3 | `path`가 디렉토리일 때 분석할 하위 디렉토리 레벨 수(0=무제한) |
| `force` | false | 전체 분석 결과 수신(그렇지 않으면 결과가 1000줄을 초과할 때 경고 메시지만 표시됨) |

## 모범 사례

### 큰 출력 처리

분석 결과가 1000줄을 초과하면 도구는 분석 대신 경고 메시지를 반환합니다. 큰 출력을 관리하는 옵션:

- **`force=true` 사용**하여 경고를 우회하고 전체 출력을 확인(대화 컨텍스트를 상당히 소모할 수 있음)
- **범위 좁히기**로 특정 하위 디렉토리나 파일 분석
- **깊이 줄이기**로 디렉토리에 `max_depth=1` 또는 `max_depth=2` 사용
- **[서브에이전트](/docs/guides/subagents)에 위임**하여 대화 기록을 채우지 않고 분석하고 요약, 예: "서브에이전트를 사용하여 전체 src/ 디렉토리를 분석하고 주요 구성 요소를 요약해줘"

### 성능 팁

- 전체 프로젝트를 분석하기 전에 더 작은 범위(특정 파일 또는 하위 디렉토리)로 시작하세요
- 디렉토리 탐색 깊이를 제한하려면 `max_depth=1` 또는 `max_depth=2`를 사용하세요
- [`.gooseignore`](/docs/guides/using-gooseignore) 및 `.gitignore` 파일을 사용하여 분석에서 불필요한 파일(예: `node_modules/`, 빌드 아티팩트 또는 민감한 파일)을 제외하세요
