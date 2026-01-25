---
sidebar_position: 35
title: CLI 명령어
sidebar_label: CLI 명령어
toc_max_heading_level: 4
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

goose는 세션, 구성 및 확장 기능을 관리하기 위한 여러 명령어가 포함된 명령줄 인터페이스(CLI)를 제공합니다. 이 가이드는 사용 가능한 모든 CLI 명령어와 대화형 세션 기능을 다룹니다.

## 플래그 이름 규칙

goose CLI는 명령어를 직관적이고 예측 가능하게 만들기 위해 플래그 이름 지정에 일관된 패턴을 따릅니다:

- **`--session-id`**: 세션 식별자에 사용(예: `20251108_1`)
- **`--schedule-id`**: 스케줄 작업 식별자에 사용(예: `daily-report`)
- **`-n, --name`**: 사람이 읽을 수 있는 이름에 사용
- **`--path`**: 파일 경로에 사용(레거시 지원)
- **`-o, --output`**: 출력 파일 경로에 사용
- **`-r, --resume` 또는 `-r, --regex`**: 컨텍스트에 따라 다름(세션의 경우 resume, 필터의 경우 regex)
- **`-v, --verbose`**: 상세 출력에 사용
- **`-l, --limit`**: 결과 수 제한에 사용
- **`-f, --format`**: 출력 형식 지정에 사용
- **`-w, --working_dir`**: 작업 디렉토리 필터에 사용

### 핵심 명령어

#### help
도움말 메뉴를 표시합니다.

**사용법:**
```bash
goose --help
```

---

#### configure
goose 설정을 구성합니다 - 공급자, 확장 기능 등.

**사용법:**
```bash
goose configure
```

---

#### info [options]
버전, 구성 파일 위치, 세션 저장소 및 로그를 포함한 goose 정보를 표시합니다.

**옵션:**
- **`-v, --verbose`**: 환경 변수 및 활성화된 확장 기능을 포함한 상세 구성 설정 표시

**사용법:**
```bash
goose info
```

---

#### version
설치된 현재 goose 버전을 확인합니다.

**사용법:**
```bash
goose --version
```

---

#### update [options]
goose CLI를 최신 버전으로 업데이트합니다.

**옵션:**
- **`--canary, -c`**: 안정 버전 대신 카나리(개발) 버전으로 업데이트
- **`--reconfigure, -r`**: 업데이트 과정에서 goose가 구성 설정을 재설정하도록 강제

**사용법:**
```bash
# 최신 안정 버전으로 업데이트
goose update

# 최신 카나리 버전으로 업데이트
goose update --canary

# 업데이트 및 설정 재구성
goose update --reconfigure
```

---

#### completion
goose 명령어, 하위 명령어 및 옵션의 탭 완성을 활성화하기 위한 셸별 스크립트를 생성합니다. 스크립트가 stdout으로 출력되므로 셸에 적합한 위치로 리디렉션한 다음 셸 구성을 다시 로드하거나 소스해야 합니다.

설치 후 다음을 수행할 수 있습니다:
- Tab을 눌러 사용 가능한 명령어와 하위 명령어 확인
- 명령어 이름과 플래그 자동 완성
- `--help`를 확인하지 않고 옵션 검색

**인수:**
- **`<SHELL>`**: 완성을 생성할 셸. 지원되는 셸: `bash`, `elvish`, `fish`, `powershell`, `zsh`

**사용법:**
```bash
# 셸에 대한 완성 스크립트 생성(stdout으로 출력)
goose completion bash
goose completion zsh
goose completion fish
```

**셸별 설치:**

<Tabs groupId="shells">
<TabItem value="zsh" label="Zsh" default>

`~/.zshrc`에 다음 줄을 추가하세요:

```bash
eval "$(goose completion zsh)"
```

그런 다음 셸을 다시 로드합니다:
```bash
source ~/.zshrc
```

</TabItem>
<TabItem value="bash" label="Bash">

`~/.bashrc` 또는 `~/.bash_profile`에 다음 줄을 추가하세요:

```bash
eval "$(goose completion bash)"
```

그런 다음 셸을 다시 로드합니다:
```bash
source ~/.bashrc
```

</TabItem>
<TabItem value="fish" label="Fish">

```bash
goose completion fish > ~/.config/fish/completions/goose.fish
```

그런 다음 터미널을 다시 시작하거나 `exec fish`를 실행합니다.

</TabItem>
<TabItem value="powershell" label="PowerShell">

PowerShell 프로필에 다음 줄을 추가하세요:

```powershell
goose completion powershell | Out-String | Invoke-Expression
```

그런 다음 프로필을 다시 로드합니다:
```powershell
. $PROFILE
```

</TabItem>
</Tabs>

:::tip 테스트
설치 및 셸 다시 로드 후 `goose `를 입력하고 Tab을 눌러 사용 가능한 명령어를 확인하거나, `goose session --`을 입력하고 Tab을 눌러 사용 가능한 옵션을 확인하여 완성을 테스트하세요.
:::

---

### 세션 관리

:::info 세션 저장소 마이그레이션
버전 1.10.0부터 goose는 개별 `.jsonl` 파일 대신 SQLite 데이터베이스(`sessions.db`)를 사용합니다.
기존 세션은 자동으로 데이터베이스로 가져옵니다. 레거시 `.jsonl` 파일은 디스크에 남아 있지만 더 이상 goose에서 관리하지 않습니다.
:::

#### session [options]
대화형 채팅 세션을 시작하거나 재개합니다.

**기본 옵션:**
- **`--session-id <session_id>`**: ID로 세션 지정(예: '20251108_1')
- **`-n, --name <name>`**: 세션에 이름 지정
- **`--path <path>`**: 파일 경로로 세션을 지정하기 위한 레거시 파라미터
- **`-r, --resume`**: 이전 세션 재개
- **`--history`**: 세션 재개 시 이전 메시지 표시
- **`--debug`**: 완전한 도구 응답, 상세 파라미터 값 및 전체 파일 경로를 출력하는 디버그 모드 활성화
- **`--max-tool-repetitions <NUMBER>`**: 동일한 파라미터로 동일한 도구를 연속으로 호출할 수 있는 최대 횟수 설정. 무한 루프 방지에 도움이 됩니다.
- **`--max-turns <NUMBER>`**: 사용자 입력 없이 허용되는 최대 턴 수 설정(기본값: 1000)

**확장 기능 옵션:**
- **`--with-extension <command>`**: stdio 확장 기능 추가
- **`--with-streamable-http-extension <url>`**: Streamable HTTP를 통해 원격 확장 기능 추가
- **`--with-builtin <id>`**: 내장 확장 기능 활성화(예: 'developer', 'computercontroller')

**사용법:**
```bash
# 기본 세션 시작
goose session -n my-project

# 이전 세션 재개
goose session --resume -n my-project
goose session --resume --session-id 20251108_2
goose session --resume --path ./session.json    # 내보낸 세션
goose session --resume --path ./session.jsonl   # 레거시 세션 저장소

# 확장 기능과 함께 시작
goose session --with-extension "npx -y @modelcontextprotocol/server-memory"
goose session --with-builtin developer
goose session --with-streamable-http-extension "http://localhost:8080/mcp"

# 고급: 여러 확장 기능 유형 혼합
goose session \
  --with-extension "echo hello" \
  --with-streamable-http-extension "http://localhost:8080/mcp" \
  --with-builtin "developer"

# 세션 동작 제어
goose session -n my-session --debug --max-turns 25
```

---

#### session list [options]
저장된 모든 세션을 나열합니다.

**옵션:**
- **`-f, --format <format>`**: 출력 형식 지정(`text` 또는 `json`). 기본값은 `text`
- **`--ascending`**: 날짜를 오름차순으로 세션 정렬(가장 오래된 것 먼저)
- **`-w, --working_dir <path>`**: 작업 디렉토리로 세션 필터링
- **`-l, --limit <number>`**: 결과 수 제한

**사용법:**
```bash
# 텍스트 형식으로 모든 세션 나열(기본값)
goose session list

# JSON 형식으로 세션 나열
goose session list --format json

# 날짜 오름차순으로 세션 정렬
goose session list --ascending

# 작업 디렉토리로 세션 필터링
goose session list -w ~/projects/myapp

# 가장 최근 10개 세션만 나열
goose session list --limit 10
```

---

#### session remove [options]
하나 이상의 저장된 세션을 제거합니다.

**옵션:**
- **`--session-id <session_id>`**: 세션 ID로 특정 세션 제거
- **`-n, --name <name>`**: 이름으로 특정 세션 제거
- **`-r, --regex <pattern>`**: 정규식 패턴과 일치하는 세션 제거
- **`--path <path>`**: 파일 경로로 특정 세션 제거(레거시)

**사용법:**
```bash
# 대화형 제거(세션 선택 프롬프트)
goose session remove

# ID로 특정 세션 제거
goose session remove --session-id 20251108_3

# 이름으로 특정 세션 제거
goose session remove -n my-project

# "project-"로 시작하는 모든 세션 제거
goose session remove -r "project-.*"

# "migration"을 포함하는 모든 세션 제거
goose session remove -r ".*migration.*"
```

:::caution
세션 제거는 영구적이며 실행 취소할 수 없습니다. goose는 삭제하기 전에 제거될 세션을 표시하고 확인을 요청합니다.
:::

---

#### session export [options]
백업, 공유, 마이그레이션 또는 문서화 목적으로 세션을 다양한 형식으로 내보냅니다.

**옵션:**
- **`--session-id <session_id>`**: ID로 특정 세션 내보내기
- **`-n, --name <name>`**: 이름으로 특정 세션 내보내기
- **`--path <path>`**: 파일 경로로 특정 세션 내보내기(레거시)
- **`-o, --output <file>`**: 내보낸 콘텐츠를 파일로 저장(기본값: stdout)
- **`--format <format>`**: 출력 형식: `markdown`, `json`, `yaml`. 기본값은 `markdown`

**내보내기 형식:**
- **`json`**: 대화 기록, 메타데이터 및 설정을 포함한 모든 데이터를 보존하는 완전한 세션 백업
- **`yaml`**: YAML 형식의 완전한 세션 백업
- **`markdown`**: 문서화 및 공유를 위해 대화의 형식화되고 읽기 쉬운 버전을 만드는 기본 형식

**사용법:**
```bash
# 대화형 내보내기
goose session export

# 백업을 위해 특정 세션을 JSON으로 내보내기
goose session export -n my-session --format json -o session-backup.json

# 읽기 쉬운 마크다운으로 특정 세션 내보내기
goose session export -n my-session -o session.md

# 다양한 형식으로 stdout에 내보내기
goose session export --session-id 20251108_4 --format json
goose session export -n my-session --format yaml

# 경로로 세션 내보내기(레거시)
goose session export --path ./my-session.jsonl -o exported.md
```

---

#### session diagnostics [options]
특정 세션의 문제 해결을 위한 포괄적인 진단 번들을 생성합니다.

**옵션:**
- **`--session-id <session_id>`**: ID로 특정 세션에 대한 진단 생성
- **`-n, --name <name>`**: 이름으로 특정 세션에 대한 진단 생성
- **`--path <path>`**: 파일 경로로 특정 세션에 대한 진단 생성(레거시)
- **`-o, --output <file>`**: 특정 파일 경로에 진단 번들 저장(기본값: `diagnostics_{session_id}.zip`)

**포함 내용:**
- **시스템 정보**: 앱 버전, 운영 체제, 아키텍처 및 타임스탬프
- **세션 데이터**: 지정된 세션의 완전한 대화 메시지 및 기록
- **구성 파일**: [구성 파일](/docs/guides/config-files)(존재하는 경우)
- **로그 파일**: 디버깅을 위한 최근 애플리케이션 로그

**사용법:**
```bash
# ID로 특정 세션에 대한 진단 생성
goose session diagnostics --session-id 20251108_5

# 이름으로 세션에 대한 진단 생성
goose session diagnostics -n my-project-session

# 사용자 정의 위치에 진단 저장
goose session diagnostics --session-id 20251108_5 -o /path/to/my-diagnostics.zip

# 대화형 선택(세션 선택 프롬프트)
goose session diagnostics
```

:::warning 개인정보 보호 안내
진단 번들에는 세션 메시지와 시스템 정보가 포함됩니다. 세션에 민감한 데이터(API 키, 개인 정보, 독점 코드)가 포함된 경우 공개적으로 공유하기 전에 내용을 검토하세요.
:::

:::tip
버그를 보고하기 전에 진단을 생성하여 더 빠른 해결에 도움이 되는 기술 세부 정보를 제공하세요. ZIP 파일은 GitHub 이슈에 첨부하거나 지원팀과 공유할 수 있습니다.
:::

---

### 작업 실행

#### run [options]
명령 파일 또는 stdin에서 명령을 실행합니다. 자세한 내용은 [전체 가이드](/docs/guides/running-tasks)를 확인하세요.

**입력 옵션:**
- **`-i, --instructions <FILE>`**: 명령이 포함된 명령 파일 경로. stdin의 경우 `-` 사용
- **`-t, --text <TEXT>`**: goose에 직접 제공할 입력 텍스트
- **`--system <TEXT>`**: 에이전트의 동작을 사용자 정의하기 위한 추가 시스템 지침 제공
- **`--recipe <RECIPE_FILE_NAME> <OPTIONS>`**: 현재 세션에서 사용자 정의 레시피 로드
- **`--params <KEY=VALUE>`**: 레시피 파일에 전달할 키-값 파라미터. 여러 번 지정 가능
- **`--sub-recipe <RECIPE>`**: 메인 레시피와 함께 포함할 서브 레시피 지정. 여러 번 지정 가능

**세션 옵션:**
- **`-s, --interactive`**: 초기 입력 처리 후 대화형 모드로 계속
- **`-n, --name <name>`**: 이 실행 세션의 이름(예: `daily-tasks`)
- **`-r, --resume`**: 이전 실행에서 재개
- **`--path <PATH>`**: 이 실행 세션의 경로(예: `./playground.jsonl`). 레거시 파일 기반 세션 저장소에 사용.
- **`--no-session`**: 세션 파일을 생성하거나 저장하지 않고 goose 명령 실행

**확장 기능 옵션:**
- **`--with-extension <COMMAND>`**: stdio 확장 기능 추가(여러 번 사용 가능)
- **`--with-streamable-http-extension <URL>`**: Streamable HTTP를 통해 원격 확장 기능 추가(여러 번 사용 가능)
- **`--with-builtin <name>`**: 이름으로 내장 확장 기능 추가(예: 'developer' 또는 여러 개: 'developer,github')

**제어 옵션:**
- **`--debug`**: 완전한 도구 응답, 상세 파라미터 값 및 전체 파일 경로 출력
- **`--max-tool-repetitions <NUMBER>`**: 동일한 파라미터로 동일한 도구를 연속으로 호출할 수 있는 최대 횟수. 무한 루프 방지에 도움
- **`--max-turns <NUMBER>`**: 사용자 입력 없이 허용되는 최대 턴 수(기본값: 1000)
- **`--explain`**: 레시피의 제목, 설명 및 파라미터 표시
- **`--render-recipe`**: 실행하지 않고 렌더링된 레시피 출력
- **`-q, --quiet`**: 조용 모드. 비응답 출력을 억제하고 모델 응답만 stdout에 출력
- **`--output-format <FORMAT>`**: 출력 형식(`text`, `json` 또는 `stream-json`). 기본값은 `text`. 자동화 및 스크립팅을 위해 JSON 구조화된 출력 사용: 완료 후 결과에는 `json`, 발생하는 이벤트에는 `stream-json`
- **`--provider`**: 이 세션에 사용할 공급자 지정(환경 변수 재정의)
- **`--model`**: 이 세션에 사용할 모델 지정(환경 변수 재정의)

**사용법:**
```bash
# 명령 파일에서 실행
goose run --instructions plan.md

# goose가 실행하고 종료하는 프롬프트로 레시피 로드
goose run --recipe recipe.yaml

# 레시피를 로드하고 대화형 세션에 머무름
goose run --recipe recipe.yaml --interactive

# 디버그 모드로 레시피 로드
goose run --recipe recipe.yaml --debug

# 레시피 세부 정보 표시
goose run --recipe recipe.yaml --explain

# 파라미터와 함께 레시피 실행
goose run --recipe recipe.yaml --params environment=production --params region=us-west-2

# 세션 저장 없이 파일에서 명령 실행
goose run --no-session -i instructions.txt

# 지정된 공급자와 모델로 실행
goose run --provider anthropic --model claude-4-sonnet -t "initial prompt"

# 사용자에게 프롬프트하기 전에 제한된 턴으로 실행
goose run --recipe recipe.yaml --max-turns 10
```

---

#### bench
다양한 실용적인 작업에서 시스템 구성을 평가하는 데 사용됩니다. 자세한 내용은 [상세 가이드](/docs/tutorials/benchmarking)를 참조하세요.

**사용법:**
```bash
goose bench ...등
```

---

#### recipe
레시피 파일 유효성 검사, 레시피 공유 관리, 사용 가능한 레시피 목록 및 goose 데스크톱에서 레시피 열기에 사용됩니다.

**명령어:**
- **`deeplink <RECIPE_NAME>`**: 레시피 파일의 공유 가능한 링크 생성
  - **`-p, --param <KEY=VALUE>`**: 레시피 파라미터 미리 채우기(여러 번 지정 가능)
- **`list [OPTIONS]`**: 로컬 디렉토리와 구성된 GitHub 저장소의 모든 사용 가능한 레시피 나열
  - **`--format <FORMAT>`**: 출력 형식(`text` 또는 `json`). 기본값은 `text`
  - **`-v, --verbose`**: 레시피 제목과 전체 파일 경로를 포함한 상세 정보 표시
- **`open <RECIPE_NAME>`**: goose 데스크톱에서 직접 레시피 파일 열기
  - **`-p, --param <KEY=VALUE>`**: 레시피 파라미터 미리 채우기(여러 번 지정 가능)
- **`validate <RECIPE_NAME>`**: 레시피 파일 유효성 검사

**사용법:**
```bash
# 공유 가능한 링크 생성
goose recipe deeplink my-recipe.yaml

# 딥링크 생성 및 파라미터 값 제공
goose recipe deeplink my-recipe.yaml -p environment=production -p region=us-west-2

# 모든 사용 가능한 레시피 나열
goose recipe list

# 상세 정보와 함께 레시피 나열
goose recipe list --verbose

# 자동화를 위해 JSON 형식으로 레시피 나열
goose recipe list --format json

# goose 데스크톱에서 레시피 열기
goose recipe open my-recipe.yaml

# 이름으로 레시피 열기
goose recipe open my-recipe

# 레시피 열기 및 파라미터 값 제공
goose recipe open my-recipe --param name=myproject

# 레시피 파일 유효성 검사
goose recipe validate my-recipe.yaml

# 레시피 명령어에 대한 도움말 보기
goose recipe help
```

---

#### schedule
[스케줄](/docs/guides/recipes/session-recipes.md#schedule-recipe)에서 레시피를 실행하여 자동화합니다.

**명령어:**
- `add <OPTIONS>`: 새로운 예약 작업 생성. 레시피의 현재 버전을 `~/.local/share/goose/scheduled_recipes`에 복사
- `list`: 모든 예약된 작업 보기
- `remove`: 예약된 작업 삭제
- `sessions`: 예약된 레시피로 생성된 세션 나열
- `run-now`: 예약된 레시피 즉시 실행
- `cron-help`: cron 표현식 예시 및 도움말 표시

**옵션:**
- `--schedule-id <NAME>`: 예약된 작업의 고유 ID(예: `daily-report`)
- `--cron "* * * * * *"`: [cron 표현식](https://en.wikipedia.org/wiki/Cron#Cron_expression)을 사용하여 작업 실행 시간 지정
- `--recipe-source <PATH>`: 레시피 YAML 파일 경로
- `-l, --limit <NUMBER>`: `sessions` 명령 사용 시 표시할 최대 세션 수

**사용법:**
```bash
goose schedule <COMMAND>

# 매일 오전 9시에 실행되는 새 예약 레시피 추가
goose schedule add --schedule-id daily-report --cron "0 0 9 * * *" --recipe-source ./recipes/daily-report.yaml

# 모든 예약된 작업 나열
goose schedule list

# 예약된 작업으로 생성된 가장 최근 10개 goose 세션 나열
goose schedule sessions --schedule-id daily-report -l 10

# 레시피 즉시 실행
goose schedule run-now --schedule-id daily-report

# 예약된 작업 제거
goose schedule remove --schedule-id daily-report
```

---

#### mcp
`<name>`(예: `'Google Drive'`)으로 지정된 활성화된 MCP 서버를 실행합니다.

**사용법:**
```bash
goose mcp <name>
```

---

#### acp
stdio를 통해 Agent Client Protocol(ACP) 에이전트 서버로 goose를 실행합니다. 이를 통해 goose가 Zed와 같은 ACP 호환 클라이언트와 함께 작동할 수 있습니다.

ACP는 AI 에이전트와 클라이언트 애플리케이션 간의 통신을 표준화하여 클라이언트가 다양한 AI 에이전트와 더 쉽게 통합할 수 있도록 하는 신흥 프로토콜 사양입니다.

**사용법:**
```bash
goose acp
```

:::info
이 명령은 ACP 호환 클라이언트에 의해 자동으로 호출되며 일반적으로 사용자가 직접 실행하지 않습니다. 클라이언트가 `goose acp` 프로세스의 수명 주기를 관리합니다. 자세한 내용은 [ACP 클라이언트에서 goose 사용](/docs/guides/acp-clients)을 참조하세요.
:::

---

### 프로젝트 관리

#### project
마지막 프로젝트에서 작업을 시작하거나 새 프로젝트를 만듭니다. 자세한 사용 예시와 워크플로우는 [프로젝트 관리 가이드](/docs/guides/managing-projects)를 참조하세요.

**별칭**: `p`

**사용법:**
```bash
goose project
```

---

#### projects
작업을 시작할 프로젝트 중 하나를 선택합니다.

**별칭**: `ps`

**사용법:**
```bash
goose projects
```

---

### 인터페이스

#### web
데스크톱 앱의 채팅 경험을 미러링하는 CLI를 통해 시작되는 경량 웹 기반 인터페이스인 goose 웹에서 새 세션을 시작합니다.

goose 웹은 다음과 같은 경우에 특히 유용합니다:
- 데스크톱 앱을 설치하지 않고 그래픽 인터페이스로 goose에 액세스하고 싶은 경우
- 모바일을 포함한 다른 장치에서 goose를 사용해야 하는 경우
- 데스크톱 앱 설치가 실용적이지 않은 환경에서 작업하는 경우

:::warning
적절한 보안 조치 없이 웹 인터페이스를 인터넷에 노출하지 마세요.
:::

**옵션:**
- **`-p, --port <PORT>`**: 웹 서버를 실행할 포트 번호. 기본값은 `3000`
- **`--host <HOST>`**: 웹 서버를 바인딩할 호스트. 기본값은 `127.0.0.1`
- **`--open`**: 서버 시작 시 자동으로 브라우저 열기
- **`--auth-token <TOKEN>`**: 웹 인터페이스에 액세스하는 데 비밀번호 필요

**사용법:**
```bash
# `http://127.0.0.1:3000`에서 웹 인터페이스 시작 및 브라우저 열기
goose web --open

# `http://127.0.0.1:8080`에서 웹 인터페이스 시작
goose web --port 8080

# `http://192.168.1.7:8080`에서 로컬 네트워크에서 액세스 가능한 웹 인터페이스 시작
goose web --host 192.168.1.7 --port 8080

# 인증이 필요한 웹 인터페이스 시작
goose web --auth-token <TOKEN>
```

:::info
서버를 중지하려면 `Ctrl+C`를 사용하세요.
:::

**제한 사항:**

웹 인터페이스는 대부분의 핵심 기능을 제공하지만 다음 제한 사항에 유의하세요:
- 일부 파일 시스템 작업은 추가 확인이 필요할 수 있습니다
- 확장 기능 관리는 CLI를 통해 수행해야 합니다
- 특정 도구 상호 작용에는 추가 설정이 필요할 수 있습니다
- 구성 변경은 서버 재시작이 필요합니다



---

### 터미널 통합

#### @goose / @g
명령 기록이 컨텍스트에 포함된 상태로 셸 프롬프트에서 직접 goose에 질문합니다. 이러한 별칭은 [터미널 통합](/docs/guides/terminal-integration.md)을 설정할 때 생성됩니다.

**예시:**
```bash
# 명령 기록 컨텍스트로 질문
@goose create a python script to process these files
@goose create a PR description summarizing these changes
@g how do I fix these permission denied errors?
```

---

## 대화형 세션 기능

### 슬래시 명령어

대화형 세션(`goose session` 또는 `goose run --interactive`를 통해)에 있으면 이러한 슬래시 명령어를 사용할 수 있습니다. 모든 명령어는 탭 완성을 지원합니다. `/` + `<Tab>`을 눌러 사용 가능한 명령어를 순환합니다.

**사용 가능한 명령어:**
- **`/?` 또는 `/help`** - 도움말 메뉴 표시
- **`/builtin <names>`** - 이름으로 내장 확장 기능 추가(쉼표로 구분)
- **`/clear`** - 현재 채팅 기록 지우기
- **`/endplan`** - 계획 모드를 종료하고 '일반' goose 모드로 돌아가기
- **`/exit` 또는 `/quit`** - 세션 종료
- **`/extension <command>`** - stdio 확장 기능 추가(형식: ENV1=val1 command args...)
- **`/mode <name>`** - 사용할 goose 모드 설정('auto', 'approve', 'chat', 'smart_approve')
- **`/plan <message_text>`** - 선택적 메시지와 함께 '계획' 모드 진입. 현재 메시지를 기반으로 계획을 만들고 사용자에게 실행 여부를 묻기
- **`/prompt <n> [--info] [key=value...]`** - 프롬프트 정보 가져오기 또는 프롬프트 실행
- **`/prompts [--extension <name>]`** - 선택적으로 확장 기능별로 필터링하여 사용 가능한 모든 프롬프트 나열
- **`/recipe [filepath]`** - 현재 대화에서 레시피를 생성하고 지정된 파일 경로에 저장(.yaml로 끝나야 함). 파일 경로가 제공되지 않으면 ./recipe.yaml에 저장
- **`/compact`** - 주요 정보를 보존하면서 컨텍스트 길이를 줄이기 위해 현재 대화 압축 및 요약
- **`/t`** - `light`, `dark`, `ansi` 테마 간 전환. [자세한 정보](#테마).
- **`/t <name>`** - 테마 직접 설정(light, dark, ansi)

**예시:**
```bash
# 테스트 실패 분류를 위한 계획 만들기
/plan let's create a plan for triaging test failures

# developer 확장 기능의 모든 프롬프트 나열
/prompts --extension developer

# 채팅 모드로 전환
/mode chat

# 세션 중 내장 확장 기능 추가
/builtin developer

# 현재 대화 기록 지우기
/clear
```
goose 데스크톱 또는 CLI에서 [레시피 실행을 위한 사용자 정의 슬래시 명령어](/docs/guides/context-engineering/slash-commands)를 만들 수도 있습니다.

---

### 테마

`/t` 명령은 goose CLI 응답에서 마크다운 콘텐츠의 구문 강조 테마를 제어합니다. 이는 응답 출력에서 헤더, 코드 블록, 굵게/기울임꼴 텍스트 및 기타 마크다운 요소에 사용되는 스타일에 영향을 줍니다.

**명령어:**
- `/t` - 테마 순환: `light` → `dark` → `ansi` → `light`
- `/t light` - `light` 테마 설정(미묘한 밝은 색상)
- `/t dark` - `dark` 테마 설정(미묘한 어두운 색상)
- `/t ansi` - `ansi` 테마 설정(더 밝은 색상으로 가장 시각적으로 구별되는 옵션)

**구성:**
- 기본 테마는 `dark`
- 테마 설정은 [구성 파일](/docs/guides/config-files)에 `GOOSE_CLI_THEME`으로 저장되며 세션 간에 유지
- 저장된 구성은 `GOOSE_CLI_THEME` [환경 변수](/docs/guides/environment-variables#session-management)를 사용하여 세션 동안 재정의 가능

:::info
구문 강조 스타일은 전체 터미널 인터페이스가 아닌 글꼴에만 영향을 줍니다. `light`와 `dark` 테마는 글꼴 색상과 두께에서 미묘한 차이가 있습니다.

goose CLI 테마는 goose 데스크톱 테마와 독립적입니다.
:::

**예시:**
```bash
# 환경 변수를 통해 세션에 ANSI 테마 설정
export GOOSE_CLI_THEME=ansi
goose session --name use-custom-theme

# 세션 중 테마 전환
/t

# 세션 중 라이트 테마 설정
/t light
```

---

## 탐색 및 제어

### 키보드 단축키

**세션 제어:**
- **`Ctrl+C`** - 현재 요청 중단
- **`Ctrl+J`** - 새 줄 추가

**탐색:**
- **`Cmd+Up/Down 화살표`** - 명령 기록 탐색
- **`Ctrl+R`** - 대화형 명령 기록 검색(역방향 검색). [자세한 정보](#명령-기록-검색).

---

### 명령 기록 검색

`Ctrl+R` 단축키는 저장된 CLI [명령 기록](/docs/guides/logs#command-history)을 대화형으로 검색합니다. 이 기능은 최근 명령을 다시 입력하지 않고 쉽게 찾아 재사용할 수 있게 합니다. 검색어를 입력하면 goose가 기록에서 일치 항목을 역방향으로 검색합니다.

**작동 방식:**
1. goose CLI 세션에서 `Ctrl+R`을 누릅니다
2. 검색어를 입력합니다
3. 다음을 사용하여 결과 탐색:
   - `Ctrl+R`로 이전 일치 항목으로 순환
   - `Ctrl+S`로 다음 일치 항목으로 순환
4. `Return`(또는 `Enter`)을 눌러 찾은 명령 실행 또는 `Esc`로 취소

예를 들어, 이 긴 명령을 다시 입력하는 대신:

```
analyze the performance issues in the sales database queries and suggest optimizations
```

`"sales database"` 또는 `"optimization"` 검색어를 사용하여 찾아 다시 실행합니다.

**검색 팁:**
- **구별되는 용어가 가장 효과적**: 결과 필터링에 도움이 되는 고유한 단어나 구문 선택
- **부분 일치 및 여러 단어 지원**: `"gith"` 및 `"run the unit test"`와 같은 구문 검색 가능
