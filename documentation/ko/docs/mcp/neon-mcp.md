---
title: Neon 확장 프로그램
description: Neon MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import { PanelLeft } from 'lucide-react';

이 튜토리얼에서는 Neon Postgres 데이터베이스와 상호 작용하고 프로젝트, 브랜치 등을 관리하기 위해 [Neon MCP 서버](https://github.com/neondatabase/mcp-server-neon)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

Neon은 두 가지 버전의 MCP 서버를 제공합니다:

1. **Remote MCP 서버**: Neon에서 호스팅하며, neon.com으로 리디렉션하여 Neon 계정에 로그인합니다.
2. **Local MCP 서버**: API 키를 사용하여 특정 조직 또는 개인 계정에 연결하여 자체 머신에서 실행할 수 있습니다.

:::warning 보안 참고 사항
Neon MCP 서버는 강력한 데이터베이스 관리 기능을 부여하며 로컬 개발용으로만 사용하도록 설계되었습니다. 실행하기 전에 항상 LLM이 요청한 작업을 검토하고 프로덕션 환경에서는 사용을 피하세요.
:::

## 구성

<Tabs groupId="remote-or-local">
  <TabItem value="remote" label="Neon Remote MCP" default>
  :::tip 요약
  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
    [설치 프로그램 실행](goose://extension?type=streamable_http&url=https%3A%2F%2Fmcp.neon.tech%2Fmcp&id=neon&name=Neon&description=Manage%20Neon%20Postgres%20databases%2C%20projects%2C%20and%20branches)
    </TabItem>
    <TabItem value="cli" label="goose CLI">
    `goose configure`를 사용하여 `Remote Extension (Streaming HTTP)` 확장 프로그램 유형을 추가합니다:

    **Endpoint URL**
    ```
    https://mcp.neon.tech/mcp
    ```
    </TabItem>
  </Tabs>
  :::

  :::info OAUTH 플로우
  브라우저에서 OAuth 창이 열립니다. 프롬프트를 따라 Neon 계정에 대한 액세스를 승인하세요.
  :::

  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
      <GooseDesktopInstaller
        extensionId="neon"
        extensionName="Neon"
        description="Neon Postgres 데이터베이스, 프로젝트 및 브랜치 관리"
        type="http"
        url="https://mcp.neon.tech/mcp"
      />
    </TabItem>
    <TabItem value="cli" label="goose CLI">
      <CLIExtensionInstructions
        name="neon-mcp-remote"
        description="Neon Postgres 데이터베이스, 프로젝트 및 브랜치 관리"
        type="http"
        url="https://mcp.neon.tech/mcp"
        timeout={300}
      />
      </TabItem>
  </Tabs>

  </TabItem>

  <TabItem value="local" label="Neon Local MCP">
  :::tip 요약
  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
      [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40neondatabase%2Fmcp-server-neon&arg=start&arg=%3CYOUR_NEON_API_KEY%3E&id=neon&name=Neon&description=Manage%20your%20Neon%20Postgres%20databases%2C%20projects%2C%20and%20branches)
    </TabItem>
    <TabItem value="cli" label="goose CLI">
      **Command**
      ```sh
      npx -y @neondatabase/mcp-server-neon start <YOUR_NEON_API_KEY>
      ```

      환경 변수는 필요하지 않습니다.
    </TabItem>
  </Tabs>
  :::

  :::info 필수 조건

  - 이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/) (v18.0.0 이상)가 설치되어 있어야 합니다
  - Neon MCP 서버를 Neon 계정으로 인증하기 위한 Neon API 키. [Neon Console](https://console.neon.tech)의 프로필 설정에서 생성할 수 있습니다. 자세한 지침은 [API 키에 대한 Neon 문서](https://neon.com/docs/manage/api-keys#creating-api-keys)를 참조하세요.
  :::

  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="neon"
      extensionName="Neon"
      description="Neon Postgres 데이터베이스, 프로젝트 및 브랜치 관리"
      command="npx"
      args={["-y", "@neondatabase/mcp-server-neon", "start", "<YOUR_NEON_API_KEY>"]}
    />
    </TabItem>

    <TabItem value="cli" label="goose CLI">
      <CLIExtensionInstructions
        name="Neon MCP"
        description="Neon Postgres 데이터베이스, 프로젝트 및 브랜치 관리"
        command="npx -y @neondatabase/mcp-server-neon start <YOUR_NEON_API_KEY>"
        timeout={300}
      />
    </TabItem>
  </Tabs>

  </TabItem>
</Tabs>

모든 설정 및 구성 옵션은 [공식 Neon MCP 서버 문서](https://neon.com/docs/ai/neon-mcp-server)를 참조하세요.

## 사용 예제 1

모든 Neon 데이터베이스 프로젝트와 그 구성에 대한 개요를 확인합니다.

### goose 프롬프트

```
List my Neon projects
```

### goose 출력

```
I'll help you list your Neon projects using the neon__list_projects tool. By default, it will show the first 10 projects.

Here are your Neon projects:

1. laravel-test (ID: mute-mountain-08595850)
- Region: AWS US East 2
- Created: July 23, 2025
- PostgreSQL: v17

2. rls-test (ID: noisy-hill-99207787)
- Region: AWS US East 2
- Created: July 20, 2025
- PostgreSQL: v17

3. neon-auth-test (ID: rapid-pond-81053507)
- Region: Azure East US 2
- Created: July 7, 2025
- PostgreSQL: v17

4. saas-starter-kit-nextjs (ID: misty-poetry-78149700)
- Region: AWS US West 2
- Created: June 23, 2025
- PostgreSQL: v17
- Note: Has logical replication enabled

5. neon-auth-mcp-oauth (ID: gentle-base-21700263)
- Region: AWS US West 2
- Created: June 4, 2025
- PostgreSQL: v17

All projects are running PostgreSQL 17 and have autoscaling configured between 1-2 compute units. Would you like more detailed information about any specific project?
```

## 사용 예제 2

특정 프로젝트 내에 어떤 테이블과 스키마가 있는지 확인합니다.

### goose 프롬프트

```
What tables do I have in my neon-auth-mcp-oauth project?
```

### goose 출력

```
In your neon-auth-mcp-oauth project, there is one table:

users_sync in the neon_auth schema
```

## 사용 예제 3

데이터베이스 테이블에 쿼리를 실행하여 데이터를 검색하고 분석합니다.

### goose 프롬프트

```
Count the rows in the users_sync table.
```

### goose 출력

```
I'll count the rows in the neon_auth.users_sync table using a COUNT query.

Total Records: 1 row
Table Location: Schema neon_auth, Table users_sync
Project: neon-auth-mcp-oauth (Project ID: gentle-base-21700263)
Content: Contains one user record
```
