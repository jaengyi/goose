---
title: goose 로깅 시스템
sidebar_label: 로깅 시스템
sidebar_position: 65
---


goose는 대화와 상호작용을 위한 통합 저장 시스템을 사용합니다. 모든 대화와 상호작용(CLI 및 데스크톱 모두)은 다음 위치에 **로컬로** 저장됩니다:

| **유형**            | **Unix 계열(macOS, Linux)**              | **Windows**                              |
|---------------------|----------------------------------------|---------------------------------------------|
| **명령 기록** | `~/.config/goose/history.txt`          | `%APPDATA%\Block\goose\data\history.txt`    |
| **세션 기록** | `~/.local/share/goose/sessions/sessions.db` | `%APPDATA%\Block\goose\data\sessions\sessions.db` |
| **시스템 로그**     | `~/.local/state/goose/logs/`           | `%APPDATA%\Block\goose\data\logs\`          |

:::info 개인정보 보호
goose는 로컬 애플리케이션이며 모든 goose 로그 파일은 로컬에 저장됩니다. 이러한 로그는 외부 서버나 제3자에게 전송되지 않으므로 모든 goose 데이터가 비공개로 유지되고 사용자의 통제 하에 있습니다.
goose가 사용자를 대신하여 사용하는 LLM과 도구에는 자체 로그와 개인정보 보호 고려사항이 있을 수 있습니다.
:::

## 명령 기록

goose는 채팅 세션 간에 명령 기록을 영구적으로 저장하여 goose가 이전 명령을 기억할 수 있습니다.

명령 기록 로그는 다음 위치에 저장됩니다:

* Unix 계열: ` ~/.config/goose/history.txt`
* Windows: `%APPDATA%\Block\goose\data\history.txt`

## 세션 기록

goose는 각 세션의 대화 기록과 상호작용을 추적하는 세션 기록을 유지합니다.
세션은 다음 위치의 SQLite 데이터베이스에 저장됩니다:
- **Unix 계열**: `~/.local/share/goose/sessions/sessions.db`
- **Windows**: `%APPDATA%\Block\goose\data\sessions\sessions.db`

:::info 세션 저장소 마이그레이션
버전 1.10.0 이전에는 goose가 `~/.local/share/goose/sessions/` 아래의 개별 `.jsonl` 파일에 세션 기록을 저장했습니다.
v1.10.0 이상으로 업그레이드하면 기존 세션이 자동으로 데이터베이스로 가져옵니다. 레거시 `.jsonl` 파일은 디스크에 남아 있지만 더 이상 goose에서 관리하지 않습니다.
:::

이 데이터베이스에는 다음을 포함한 모든 저장된 세션 데이터가 포함됩니다:
- 세션 메타데이터(ID, 이름, 작업 디렉토리, 타임스탬프)
- 대화 메시지(사용자 명령, 어시스턴트 응답, 역할 정보)
- 도구 호출 및 결과(ID, 인수, 응답, 성공/실패 상태)
- 토큰 사용 통계
- 확장 기능 데이터 및 구성

세션 ID는 `YYYYMMDD_<COUNT>` 형식으로 이름이 지정됩니다. 예: `20250310_2`. goose CLI는 각 세션 시작 시 세션 ID를 출력합니다. 세션 ID를 얻으려면 [`goose session list` 명령](/docs/guides/goose-cli-commands#session-list-options)을 사용하여 모든 사용 가능한 세션을 확인하세요.

세션 검색에 대한 자세한 내용은 [세션 관리](/docs/guides/sessions/session-management)도 참조하세요.

## 시스템 로그

goose는 다양한 구성요소에 대한 로그를 저장합니다. CLI 및 서버 로그는 날짜 기반 디렉토리로 자동 정리되며 과도한 디스크 사용을 방지하기 위해 2주 후에 정리됩니다.

[프롬프트 주입 감지](/docs/guides/security/prompt-injection-detection)가 활성화되면 CLI 및 서버 로그에 다음도 포함됩니다:
* 고유 ID(형식: `SEC-{uuid}`)가 있는 보안 발견 사항
* 발견 사항 ID와 연결된 사용자 결정(허용/거부)

:::info
확장 기능은 선택적으로 `~/.local/state/goose/logs/` 아래의 하위 디렉토리에 로그를 기록할 수 있습니다. 특정 하위 디렉토리 구조는 각 확장 기능의 구현에 따라 결정됩니다.
:::

### 데스크톱 애플리케이션 로그

데스크톱 애플리케이션은 자체 로그를 유지합니다:
* macOS: `~/Library/Application Support/Goose/logs/main.log`
* Windows: `%APPDATA%\Block\goose\logs\main.log`

데스크톱 애플리케이션은 자체 운영 로그와 상태 데이터에 대해 플랫폼 규칙을 따르지만 실제 대화와 상호작용에는 표준 goose [세션 기록](#세션-기록)을 사용합니다. 이는 goose와 상호작용하는 데 사용하는 인터페이스에 관계없이 대화 기록이 일관됨을 의미합니다.

### CLI 로그

CLI 로그는 다음 위치에 저장됩니다:
* Unix 계열: `~/.local/state/goose/logs/cli/`
* Windows: `%APPDATA%\Block\goose\data\logs\cli\`

로그는 날짜 기반 하위 디렉토리(예: `cli/2025-11-13/`)로 정리되며 2주보다 오래된 하위 디렉토리는 자동으로 삭제됩니다.

CLI 세션 로그에는 다음이 포함됩니다:
* 도구 호출 및 응답
* 명령 실행 세부 정보
* 세션 식별자
* 타임스탬프

CLI 로그는 다음을 포함한 확장 기능 관련 활동도 캡처합니다:
* 도구 초기화
* 도구 기능 및 스키마
* 확장 기능별 작업
* 명령 실행 결과
* 오류 메시지 및 디버깅 정보
* 확장 기능 구성 상태
* 확장 기능별 프로토콜 정보

### 서버 로그

서버 로그는 다음 위치에 저장됩니다:
* Unix 계열: `~/.local/state/goose/logs/server/`
* Windows: `%APPDATA%\Block\goose\data\logs\server\`

로그는 날짜 기반 하위 디렉토리(예: `server/2025-11-13/`)로 정리되며 2주보다 오래된 하위 디렉토리는 자동으로 삭제됩니다.

서버 로그에는 컴퓨터에서 실행되는 로컬 서버 프로세스인 goose 데몬(`goosed`)에 대한 정보가 포함됩니다. 이 서버 구성요소는 CLI, 확장 기능 및 LLM 간의 통신을 관리합니다.

서버 로그에는 다음이 포함됩니다:
* 서버 초기화 세부 정보
* JSON-RPC 통신 로그
* 서버 기능
* 프로토콜 버전 정보
* 클라이언트-서버 상호작용
* 확장 기능 로딩 및 초기화
* 도구 정의 및 스키마
* 확장 기능 지침 및 기능
* 디버그 수준 전송 정보
* 시스템 기능 및 구성
* 운영 체제 정보
* 작업 디렉토리 정보
* 전송 계층 통신 세부 정보
* 메시지 파싱 및 처리 정보
* 요청/응답 사이클
* 오류 상태 및 처리
* 확장 기능 초기화 시퀀스

### LLM 요청 로그

LLM 요청 로그는 언어 모델 공급자에게 전송된 원시 요청 및 응답 데이터를 캡처합니다:
* Unix 계열: `~/.local/state/goose/logs/llm_request.*.jsonl`
* Windows: `%APPDATA%\Block\goose\data\logs\llm_request.*.jsonl`

이러한 로그는 가장 최근 완료된 10개의 요청을 유지하는 번호가 지정된 회전 시스템(`llm_request.0.jsonl`에서 `llm_request.9.jsonl`까지)을 사용합니다. 각 로그에는 모델 구성, 입력 페이로드, 응답 데이터 및 토큰 사용 정보가 포함됩니다.
