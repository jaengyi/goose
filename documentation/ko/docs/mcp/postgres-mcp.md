---
title: PostgreSQL 확장 프로그램
description: PostgreSQL MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/PZlYQ5IthYM" />

서버 아카이브됨

PostgreSQL MCP 서버 확장 프로그램을 사용하면 goose가 PostgreSQL 데이터베이스와 직접 상호 작용하여 데이터베이스 작업, 쿼리, 스키마 관리 기능을 활성화할 수 있습니다. 이를 통해 자연어 상호 작용을 통해 데이터베이스와 쉽게 작업할 수 있습니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=@modelcontextprotocol/server-postgres&arg=Your%20PostgreSQL%20connection%20URL&id=postgres&name=PostgreSQL&description=PostgreSQL%20database%20integration)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb
  ```
  </TabItem>
</Tabs>
:::

## 연결 사용자 정의

현재 이 MCP 서버는 단일 사전 정의된 데이터베이스에만 연결할 수 있으며, 연결 URL은 명령에 지정해야 합니다. 여기서는 로컬 데이터베이스에 액세스하기 위해 `postgresql://localhost/mydb`를 예제로 사용하고 있지만, 자신의 환경에 맞게 구성할 수 있습니다.

PostgreSQL 연결 URL은 다음 형식을 따릅니다:
```
postgresql://username:password@hostname:5432/database
```

각 항목:
- `username`: PostgreSQL 사용자 이름
- `password`: PostgreSQL 비밀번호
- `hostname`: PostgreSQL이 실행되는 호스트 (예: localhost, IP 주소 또는 도메인)
- `5432`: 기본 PostgreSQL 포트 (다른 포트를 사용하는 경우 변경)
- `database`: 데이터베이스 이름

예제:
- 로컬 데이터베이스: `postgresql://localhost/mydb`
- 자격 증명이 있는 로컬: `postgresql://myuser:mypass@localhost/mydb`
- 원격 데이터베이스: `postgresql://user:pass@db.example.com:5432/production`

:::caution
자격 증명이 포함된 연결 문자열은 절대 버전 관리에 커밋하지 마세요! 환경 변수나 안전한 구성 관리를 사용하세요.
:::



## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="postgres"
    extensionName="PostgreSQL"
    description="PostgreSQL 데이터베이스 통합"
    command="npx"
    args={["-y", "@modelcontextprotocol/server-postgres", "Your PostgreSQL connection URL"]}
  />

  :::info
  PostgreSQL 연결 URL을 다음 형식으로 입력하세요: `postgresql://username:password@hostname:5432/database`
  :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">
  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Command-line Extension` 추가 선택
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◆  What type of extension would you like to add?
    │  ○ Built-in Extension
    // highlight-start
    │  ● Command-line Extension (Run a local command or script)
    // highlight-end
    │  ○ Remote Extension (Streamable HTTP)
    └
  ```

  3. 확장 프로그램 이름 지정
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    // highlight-start
    ◆  What would you like to call this extension?
    │  PostgreSQL
    // highlight-end
    └
  ```

  4. 데이터베이스 연결 URL과 함께 명령 입력
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What would you like to call this extension?
    │  PostgreSQL
    │
    // highlight-start
    ◆  What command should be run?
    │  npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb
    // highlight-end
    └
  ```

  5. 타임아웃 설정 (기본값 300초면 보통 충분합니다)
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What would you like to call this extension?
    │  PostgreSQL
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb
    │
    // highlight-start
    ◆  Please set the timeout for this tool (in secs):
    │  300
    // highlight-end
    └
  ```

  6. 설명 추가 여부 선택. "Yes"를 선택하면 확장 프로그램 설명을 입력하라는 메시지가 표시됩니다.
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What would you like to call this extension?
    │  PostgreSQL
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    // highlight-start
    ◆  Would you like to add a description?
    │  No
    // highlight-end
    └
  ```

  7. PostgreSQL 연결 URL 구성
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What would you like to call this extension?
    │  PostgreSQL
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-postgres postgresql://localhost/mydb
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    ◇  Would you like to add a description?
    │  No
    │
    // highlight-start
    ◆  Would you like to add environment variables?
    │  No
    // highlight-end
    └  Added PostgreSQL extension
  ```

  </TabItem>
</Tabs>

## 사용 예제

PostgreSQL 확장 프로그램을 사용하면 데이터를 쿼리하고 분석하거나 테이블 구조에 대한 정보를 얻을 수 있습니다. 이 확장 프로그램은 *읽기 전용* 작업만 가능하므로 데이터베이스에 대한 정보를 얻는 데 사용할 수 있지만 수정하는 데는 사용할 수 없습니다.

원하는 작업을 자연어로 설명하기만 하면 goose가 적절한 SQL 명령과 PostgreSQL 기능을 사용하여 수행하도록 도와줍니다.

#### goose 프롬프트
```
Search my database for any students whose current GPA is higher than the average in their current grade level. Tell me their name, grade, and how much they exceed their grade's average GPA, and the average GPA.
```

#### goose 출력
```
Based on the query results, here are the students who are performing above their grade level average:

Grade 11
  Emma Johnson
    GPA: 4.00
    Grade Average: 3.90
    Exceeds average by: 0.10 points

Grade 12
  John Smith
    GPA: 3.85
    Grade Average: 3.78
    Exceeds average by: 0.08 points

Both students are performing well above their respective grade level averages. Emma Johnson in Grade 11 shows the highest difference above the grade average, exceeding it by 0.10 points, while John Smith in Grade 12 exceeds his grade's average by 0.08 points.
```
