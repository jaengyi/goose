---
title: Supabase 확장 프로그램
description: Supabase MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

이 튜토리얼에서는 Supabase 프로젝트와 상호 작용하고, 테이블을 관리하고, 데이터를 쿼리하고, Edge Functions를 배포하고, Supabase 백엔드와 직접 상호 작용하기 위해 [Supabase MCP 서버](https://github.com/supabase-community/supabase-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?type=streamable_http&url=https%3A%2F%2Fmcp.supabase.com%2Fmcp&id=supabase&name=Supabase&description=Connect%20your%20Supabase%20projects%20to%20AI%20assistants.%20Manage%20tables%2C%20query%20data%2C%20deploy%20Edge%20Functions%2C%20and%20interact%20with%20your%20Supabase%20backend%20directly%20from%20your%20MCP%20client.)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  `goose configure`를 사용하여 `Remote Extension (Streaming HTTP)` 확장 프로그램 유형을 추가합니다:

  **Endpoint URL**
  ```
  https://mcp.supabase.com/mcp
  ```
  </TabItem>
</Tabs>
:::

## 구성

### Supabase Remote MCP

Supabase MCP 서버는 Supabase에서 호스팅되며 OAuth 기반 인증을 제공합니다. 연결하면 supabase.com으로 리디렉션되어 계정에 로그인하게 됩니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>

    <GooseDesktopInstaller
      extensionId="supabase"
      extensionName="Supabase"
      description="Supabase 프로젝트를 AI 어시스턴트에 연결합니다. 테이블을 관리하고, 데이터를 쿼리하고, Edge Functions를 배포하고, MCP 클라이언트에서 직접 Supabase 백엔드와 상호 작용합니다."
      type="http"
      url="https://mcp.supabase.com/mcp"
      envVars={[]}
    />

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    <CLIExtensionInstructions
      name="Supabase"
      description="Supabase 프로젝트를 AI 어시스턴트에 연결합니다. 테이블을 관리하고, 데이터를 쿼리하고, Edge Functions를 배포하고, MCP 클라이언트에서 직접 Supabase 백엔드와 상호 작용합니다."
      type="http"
      url="https://mcp.supabase.com/mcp"
      timeout={300}
      envVars={[]}
    />

  </TabItem>
</Tabs>

#### OAuth 플로우

goose를 통해 Supabase MCP 서버에 처음 연결하면 브라우저에서 OAuth 창이 열립니다. 프롬프트를 따라 Supabase 프로젝트에 대한 액세스를 승인하세요.

:::note 보안 참고 사항
Supabase MCP 서버는 데이터베이스 및 프로젝트 관리 기능에 대한 액세스를 부여합니다. 실행하기 전에 항상 LLM이 요청한 작업을 검토하고 적절한 환경(개발, 스테이징, 프로덕션)에서 작업하고 있는지 확인하세요.
:::

## 사용 예제

### 예제 1: Supabase 프로젝트 목록 보기

모든 Supabase 프로젝트와 그 구성에 대한 개요를 확인합니다.

#### goose 프롬프트

```
List all my Supabase projects
```

#### goose 출력

:::note Desktop

```
Here are your Supabase projects:

1. my project (ID: abc123xyz789defg)
   - Region: US West 1
   - Created: October 30, 2025
   - Status: ACTIVE_HEALTHY
   - Database: PostgreSQL 17.6.1.031 (Engine v17)
   - Host: db.abc123xyz789defg.supabase.co

2. new project (ID: jkl456mno012pqrs)
   - Region: US West 1
   - Created: October 31, 2025
   - Status: INACTIVE
   - Database: PostgreSQL 17.6.1.032 (Engine v17)
   - Host: db.jkl456mno012pqrs.supabase.co

Would you like more detailed information about any specific project?
```

:::

### 예제 2: 데이터베이스 테이블 쿼리

Supabase 프로젝트 내의 테이블을 검색하고 쿼리합니다.

#### goose 프롬프트

```
What tables exist in my project?
```

#### goose 출력

:::note Desktop

```
Tables in my project (public schema):

1. users
   - RLS Enabled: Yes
   - Row count: 150
   - Columns: id (uuid), email (text), name (text), created_at (timestamptz), role (text)
   - Primary Key: id
   - Foreign Keys: Referenced by posts.user_id, profiles.user_id

2. posts
   - RLS Enabled: Yes
   - Row count: 342
   - Columns: id (uuid), user_id (uuid), title (text), content (text), created_at (timestamptz)
   - Primary Key: id
   - Foreign Keys: References users.id, referenced by comments.post_id

3. comments
   - RLS Enabled: Yes
   - Row count: 1089
   - Columns: id (uuid), post_id (uuid), user_id (uuid), content (text), created_at (timestamptz)
   - Primary Key: id
   - Foreign Keys: References posts.id, references users.id

4. profiles
   - RLS Enabled: Yes
   - Row count: 150
   - Columns: id (uuid), user_id (uuid), bio (text), avatar_url (text), updated_at (timestamptz)
   - Primary Key: id
   - Foreign Keys: References users.id

5. categories
   - RLS Enabled: No
   - Row count: 12
   - Columns: id (uuid), name (text), description (text), created_at (timestamptz)
   - Primary Key: id

Would you like to query data from any of these tables?
```

:::

### 예제 3: Edge Function 배포

goose를 통해 직접 Supabase Edge Functions를 배포하고 관리합니다.

#### goose 프롬프트

```
Deploy a new Edge Function called 'hello-world' that returns a simple JSON response
```

#### goose 출력

:::note Desktop

```
Edge Function deployed successfully!

Function Name: hello-world
Version: 1
Status: ACTIVE
Function ID: c4e03748-3e72-45e2-bdca-03f7e4800874
Project: my project (tabowbdrhvtlvzvfyinw)
Entrypoint: index.ts
JWT Verification: Enabled
Created: November 8, 2025

Function code:
```typescript
Deno.serve(async (req) => {
  return new Response(
    JSON.stringify({ message: "Hello World!" }),
    { headers: { "Content-Type": "application/json" } },
  )
})
```

Endpoint:
https://tabowbdrhvtlvzvfyinw.supabase.co/functions/v1/hello-world

You can test the function by sending a request to the endpoint above.
```

:::

## 추가 리소스

Supabase MCP 서버와 그 기능에 대한 자세한 내용은 다음을 참조하세요:
- [Supabase MCP 서버 GitHub 저장소](https://github.com/supabase-community/supabase-mcp)
- [Supabase 문서](https://supabase.com/docs)
- [Supabase 대시보드](https://supabase.com/dashboard)
