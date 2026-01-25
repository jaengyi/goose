---
title: Container Use 확장 프로그램
description: Container-Use MCP를 goose 확장 프로그램으로 사용하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/X3tf61_Tak0" />

이 튜토리얼에서는 goose가 격리된 환경에서 작업할 수 있도록 [Container Use MCP 서버](https://container-use.com)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=container-use&arg=stdio&id=container-use&name=container%20use&description=use%20containers%20with%20dagger%20and%20git%20for%20isolated%20environments)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  container-use stdio
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
시스템에 [Docker](https://www.docker.com/)가 설치되어 있어야 합니다. 원격 MCP 서버를 사용하려면 [Node.js](https://nodejs.org/)도 설치되어 있어야 합니다.
:::

<Tabs groupId="online_offline">
  <TabItem value="remote-mcp" label="Remote MCP" default>

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose Desktop" default>
        <GooseDesktopInstaller
            extensionId="container-use"
            extensionName="Container Use"
            description="container-use로 컨테이너 자동화 실행"
            command="npx"
            args={["-y", "mcp-remote", "https://container-use.com/mcp"]}
            cliCommand="npx -y mcp-remote https://container-use.com/mcp"
            timeout={300}
            note="Node.js와 Docker가 설치되어 있어야 합니다; [container-use.com/quickstart](https://container-use.com/quickstart)를 참조하세요."
        />
    </TabItem>
      <TabItem value="cli" label="goose CLI">
          <CLIExtensionInstructions
            name="Container Use"
            description="container-use로 컨테이너 자동화 실행"
            command="npx -y mcp-remote https://container-use.com/mcp"
            timeout={300}
          />
      </TabItem>
    </Tabs>

  </TabItem>
  <TabItem value="local-mcp" label="Local MCP">

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose Desktop" default>
        <GooseDesktopInstaller
            extensionId="container-use"
            extensionName="Container Use"
            description="container-use로 컨테이너 자동화 실행"
            command="container-use"
            args={["stdio"]}
            cliCommand="container-use stdio"
            timeout={300}
            note="Docker가 설치되어 있어야 합니다; [container-use.com/quickstart](https://container-use.com/quickstart)를 참조하세요."
        />
    </TabItem>
      <TabItem value="cli" label="goose CLI">
          <CLIExtensionInstructions
            name="Container Use"
            description="container-use로 컨테이너 자동화 실행"
            command="container-use stdio"
            timeout={300}
          />
      </TabItem>
    </Tabs>

  </TabItem>
</Tabs>

## 사용 예제

격리된 컨테이너 환경 내에서 파일 기반 할 일 앱을 SQLite 데이터베이스를 사용하도록 업데이트하고 전체 테스트까지 수행하는 간단한 예제입니다.

#### 프롬프트

```
컨테이너 에이전트를 실행하여 할 일 목록 데이터를 sqlite에 저장하는 기능을 추가하고, 빌드 및 테스트를 실행하되, 메인 코드가 안전하게 유지되도록 별도의 Git 브랜치를 사용해줘.
```

#### goose 출력

```
별도의 Git 브랜치에서 할 일 목록 애플리케이션에 SQLite 기능을 추가하기 위한 컨테이너 환경을 만드는 것을 도와드리겠습니다. 먼저 환경을 만들고 현재 코드 구조를 살펴보겠습니다.
─── environment_create | containeruse ──────────────────────────
environment_source: ./examples/todo-list
explanation: ...
title: ...

이제 필요한 도구로 환경을 설정하고 SQLite 기능 개발을 진행하겠습니다:
─── environment_config | containeruse ──────────────────────────
config:
    base_image: python:3.11-slim
    setup_commands:
        -
            [REDACTED: 63 chars]
        -
            pip install pytest pytest-cov

environment_id: thankful-whale
environment_source: ./examples/todo-list
explanation: ...

환경이 실행된 후 pytest를 설치하겠습니다:

---

좋습니다! 할 일 목록 앱에 SQLite 기능을 안전하게 추가하기 위한 컨테이너 환경을 만들었습니다. 먼저 현재 코드 구조를 탐색한 다음 새 Git 브랜치를 만들겠습니다.

---

이제 SQLite 기반 스토리지 구현을 만들겠습니다. 먼저, SQLite를 지원하는 새 버전의 TaskManager를 만들겠습니다:

---

## ✅ SQLite 기능 성공적으로 추가!

컨테이너 환경을 성공적으로 만들고 할 일 목록 애플리케이션에 SQLite 기능을 추가했습니다! 달성한 내용은 다음과 같습니다:

### 🎯 **구현된 기능**

1. **SQLite 데이터베이스 저장소**
   - 일반 텍스트 파일 저장소를 SQLite 데이터베이스로 대체
   - 자동 증가 ID가 있는 적절한 데이터베이스 스키마 생성

2. **향상된 작업 관리**
   - 모든 원래 기능 유지 (추가, 목록, 완료/미완료 표시, 삭제, 지우기)
   - 완료율을 보여주는 새로운 **통계 기능** 추가
   - 데이터베이스 ID가 있는 개선된 작업 표시

3. **두 가지 구현 버전**
   - `taskman_sqlite.py` - Rich UI가 있는 전체 기능
   - `taskman_sqlite_simple.py` - 외부 종속성 없는 단순화된 버전

### 🧪 **테스트 완료**

- ✅ 포괄적인 테스트 스위트 생성 (`test_taskman_sqlite.py`)
- ✅ 간단한 테스트 러너 구축 (`test_simple.py`)
- ✅ 모든 테스트 성공적으로 통과
- ✅ 수동 테스트로 기능 완벽하게 작동 확인

### 📊 **라이브 데모 결과**

  # 작업 성공적으로 추가됨
  ✓ Added task: Buy groceries
  ✓ Added task: Walk the dog

  # SQLite ID가 있는 작업 목록
  📋 Task List (SQLite)
  #   ID   Status   Task
  1   1    ✓ Done   Buy groceries
  2   2    ○ Todo   Walk the dog

  # 통계 작동
  Task Statistics:
  Total tasks: 2
  Completed: 1
  Pending: 1
  Completion rate: 50.0%

  # 데이터베이스 확인
  sqlite3 tasks.db "SELECT * FROM tasks;"
  1|Buy groceries|1
  2|Walk the dog|0


### 🔒 **안전한 개발**

- ✅ 별도의 컨테이너 환경 사용
- ✅ 모든 변경 사항이 메인 코드와 격리됨
- ✅ 원래 구현 유지
- ✅ 역호환성 유지

### 📁 **생성된 파일**

- `taskman_sqlite_simple.py` - 메인 SQLite 구현
- `test_simple.py` - 테스트 스위트
- `SQLITE_FEATURE_README.md` - 포괄적인 문서
- `tasks.db` - SQLite 데이터베이스 파일

SQLite 구현이 완전히 작동하며 사용할 준비가 되었습니다! 원래 파일 기반 버전은 그대로 유지되므로 기존 코드에 대한 위험 없이 새 SQLite 기능을 안전하게 평가할 수 있습니다.

```
