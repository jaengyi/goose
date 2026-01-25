---
title: Google Drive 확장 프로그램
description: Google Drive MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/p9HGYbJk9wU" />

서버 아카이브됨

이 튜토리얼에서는 Google Drive에서 파일을 나열하고, 읽고, 검색할 수 있도록 [Google Drive MCP 서버](https://www.pulsemcp.com/servers/modelcontextprotocol-gdrive)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40modelcontextprotocol%2Fserver-gdrive&id=google-drive&name=Google%20Drive&description=Google%20Drive%20integration&env=GDRIVE_CREDENTIALS_PATH%3DPath%20to%20Google%20Drive%20credentials&env=GDRIVE_OAUTH_PATH%3DPath%20to%20OAuth%20token)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  GDRIVE_OAUTH_PATH=$USER_HOME/.config/gcp-oauth.keys.json \
  GDRIVE_CREDENTIALS_PATH=$USER_HOME/.config/.gdrive-server-credentials.json \
  npx -y @modelcontextprotocol/server-gdrive auth \
  npx -y @modelcontextprotocol/server-gdrive
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  GDRIVE_CREDENTIALS_PATH: $USER_HOME/.config/.gdrive-server-credentials.json
  GDRIVE_OAUTH_PATH: $USER_HOME/.config/gcp-oauth.keys.json
  ```
:::

:::info
환경 변수에는 반드시 절대 경로를 사용해야 합니다. `$USER_HOME`을 실제 홈 디렉토리로 교체하세요.
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

Google Drive 서버 자격 증명과 oauth 키를 얻으려면 아래 단계를 따르세요:

  1. API 액세스를 활성화하기 위해 Google Cloud 자격 증명 설정:
        - Google Cloud 프로젝트 생성
            - [Google Cloud Console](https://console.cloud.google.com/projectcreate)로 이동하여 새 프로젝트 생성
            - `location`은 `No organization`으로 둘 수 있습니다
        - Google Drive API 활성화
            - 프로젝트에서 [API 제품 라이브러리](https://console.cloud.google.com/workspace-api/products)로 이동
            - 왼쪽 상단 모서리를 확인하여 올바른 프로젝트에 있는지 확인
            - `Google Drive API`를 검색하고 활성화

  2. OAuth 동의 화면 구성
        -  [OAuth 동의 화면](https://console.cloud.google.com/auth/overview/create)으로 이동
        -  `project name`, `user support email` 등 필수 정보 입력
        -  `Audience`로 `Internal`을 선택하고 `create` 누르기
        -  `Internal`을 선택할 수 없는 경우 `External`을 선택하고 다음 추가 단계를 따르세요:
            - [Audience](https://console.cloud.google.com/auth/audience) 화면으로 이동
            - `Test users` 아래에서 `Add Users` 클릭

 3. OAuth 자격 증명 생성
        -  [OAuth Clients](https://console.cloud.google.com/apis/credentials/oauthclient)로 이동
        -  `Create Client` 클릭
        -  **Application Type: Desktop App** 선택
        -  JSON 키 파일 다운로드
        -  `gcp-oauth.keys.json`으로 이름 변경
        -  확장 프로그램이 액세스할 수 있는 안전한 위치로 이동:
            ```sh
            mv ~/Downloads/gcp-oauth.keys.json ~/.config/gcp-oauth.keys.json
            ```
  4. Google 계정 연결

     Google 계정을 연결하려면 터미널에서 다음 인증 명령을 실행하세요:
          ```sh
          GDRIVE_OAUTH_PATH=$USER_HOME/.config/gcp-oauth.keys.json \
          GDRIVE_CREDENTIALS_PATH=$USER_HOME/.config/.gdrive-server-credentials.json \
          npx -y @modelcontextprotocol/server-gdrive auth
          ```
         :::info
         `$USER_HOME`을 실제 홈 디렉토리로 교체하세요.
         :::

      인증을 위해 브라우저 창이 열립니다. 프롬프트를 따라 Google 계정을 연결하고 OAuth 프로세스를 완료하세요. 이 단계에서 환경 변수 `GDRIVE_CREDENTIALS_PATH`가 저장된 자격 증명으로 설정됩니다.

:::tip
Google Drive 확장 프로그램을 사용할 때 하루에 한 번 재인증해야 합니다. 재인증하려면 터미널에서 인증 명령을 다시 실행하세요.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="google-drive"
    extensionName="Google Drive"
    description="Google Drive 통합"
    command="npx"
    args={["-y", "@modelcontextprotocol/server-gdrive"]}
    envVars={[
      { name: "GDRIVE_CREDENTIALS_PATH", label: "Google Drive 자격 증명 경로" },
      { name: "GDRIVE_OAUTH_PATH", label: "OAuth 토큰 경로" }
    ]}
  />

  :::info
  - `GDRIVE_CREDENTIALS_PATH`에는 `$USER_HOME/.config/.gdrive-server-credentials.json` 입력
  - `GDRIVE_OAUTH_PATH`에는 `$USER_HOME/.config/gcp-oauth.keys.json` 입력

  `$USER_HOME`을 실제 홈 디렉토리로 교체하세요. 이 확장 프로그램이 작동하려면 절대 경로를 지정해야 합니다.
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
    │  google drive
    // highlight-end
    └
  ```

  4. 명령 입력
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  google drive
    │
    // highlight-start
    ◆  What command should be run?
    │  npx -y @modelcontextprotocol/server-gdrive
    // highlight-end
    └
  ```

  5. goose가 작업 완료를 기다리는 타임아웃 시간(초) 입력. 기본값은 300초입니다
   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  google drive
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-gdrive
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
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  google drive
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-gdrive
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    // highlight-start
    ◇  Would you like to add a description?
    │  No
    // highlight-end
    └
  ```

  7. 환경 변수 추가

   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  google drive
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-gdrive
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    ◇  Would you like to add a description?
    │  No
    │
    // highlight-start
    ◆  Would you like to add environment variables?
    │  Yes
    │
    ◇  Environment variable name:
    │  GDRIVE_CREDENTIALS_PATH
    │
    ◇  Environment variable value:
    │  ~/.config/.gdrive-server-credentials.json
    │
    ◇  Add another environment variable?
    │  yes
    |
    ◇  Environment variable name:
    │  GDRIVE_OAUTH_PATH
    │
    ◇  Environment variable value:
    │  ~/.config/gcp-oauth.keys.json
    │
    // highlight-end
    └  Added google drive extension
  ```

  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose를 사용하여 30분 후에 있는 중요한 마케팅 예산 검토 회의를 빠르게 준비합니다. goose에게 Google Drive에서 관련 문서를 찾고, Q1 성과를 요약하고, 마케팅 자동화 및 비디오 제작에 대한 주요 결정 사항을 강조하고, 지난 회의에서 남은 실행 항목을 식별하도록 요청합니다.

### goose 프롬프트
```
I have an important marketing budget review meeting in 30 minutes and I need your help getting prepared. I have several documents in my Google Drive from our previous meetings and planning sessions. Could you help me by:

1. Finding all relevant documents about our marketing budget and performance
2. Giving me a quick summary of our Q1 performance
3. Highlighting the key decisions we need to make about the marketing automation tool and video production
4. Identifying any outstanding action items from our last meeting

I need a quick but comprehensive overview so I can walk into this meeting well-prepared.
```

### goose 출력

```
I'll help you prepare for your meeting. Let me first list available resources from Google Drive to make sure we access the correct files.

[... list resources ...]

Great! I can see several relevant documents. Let me access them one by one to gather the information you need.

[... reads resources ...]

Based on all these documents, here's your comprehensive overview for the meeting:

Q1 Performance Summary:

  - Overall 5% under budget ($104,200 spent vs $107,000 budgeted)
  - Digital Marketing performing well with highest ROI:
      - Email marketing: 3.2 ROI
      - Google Ads: 2.4 ROI
      - Content Marketing: 2.1 ROI
  - Social media engagement up 23% from previous quarter
  - Content marketing blog posts generating 2x expected leads

Key Decisions Needed:

  1. Marketing Automation Tool (License Renewal Due):
    - Current tool (BasicAutomation Pro): $12,000/year
    - Options:
        - GrowthStack ($15,000/year) - recommended option, balanced features/price
        - MarketingHub Enterprise ($18,000/year) - premium option with AI features
        - Decision needed on whether to upgrade or maintain current solution
    - Implementation would take 6 weeks if changing providers

  2. Video Production Investment:
    - Three vendor options:
        - CreativeForce Media ($16,200) - most comprehensive, unlimited revisions
        - VisualCraft Studios ($14,500) - solid middle option
        - Digital Narrative Pro ($12,800) - basic package
    - CreativeForce Media is recommended despite higher cost due to comprehensive deliverables
    - Timeline ranges from 3-5 weeks depending on vendor choice

Outstanding Action Items from Last Meeting:

  1. Sarah needs to present ROI analysis for video content investment
  2. John needs to complete software license review
  3. Lisa to present new PPC strategy
  4. Mike has completed the video production agency quotes

Q2 Planning Considerations:

  - Proposed 12% budget increase for Q2 ($120,000 total)
  - Potential reallocation needed based on new tool and video production decisions
  - PPC strategy review pending

Would you like me to focus on any particular aspect of this overview in more detail?
```
