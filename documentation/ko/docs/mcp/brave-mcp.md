---
title: Brave Search 확장 프로그램
description: Brave Search API를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/kD2YA61NTLU" />

서버 이전됨

이 튜토리얼에서는 웹 검색과 로컬 검색을 모두 활성화하기 위해 [Brave Search MCP 서버](https://www.pulsemcp.com/servers/brave-search)를 goose 확장 프로그램으로 시작하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40modelcontextprotocol%2Fserver-brave-search&id=brave-search&name=Brave%20Search&description=Brave%20Search%20API&env=BRAVE_API_KEY%3DYour%20API%20Key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @modelcontextprotocol/server-brave-search
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  BRAVE_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="brave-search"
    extensionName="Brave Search"
    description="Brave Search API"
    command="npx"
    args={["-y", "@modelcontextprotocol/server-brave-search"]}
    envVars={[{ name: "BRAVE_API_KEY", label: "Your Brave Search API Key" }]}
    apiKeyLink="https://api-dashboard.search.brave.com/app/keys"
    apiKeyLinkText="Brave Search API Key"
  />
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
    │  brave-search
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
    │  brave-search
    │
    // highlight-start
    ◆  What command should be run?
    │  npx -y @modelcontextprotocol/server-brave-search
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
    │  brave-search
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-brave-search
    │
    // highlight-start
    ◆  Please set the timeout for this tool (in secs):
    │  300
    // highlight-end
    │
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
    │  brave-search
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-brave-search
    │
    ◆  Please set the timeout for this tool (in secs):
    │  300
    │
    // highlight-start
    ◇  Would you like to add a description?
    │  No
    // highlight-end
    │
    └
  ```

  7. [Brave Search API Key](https://api-dashboard.search.brave.com/app/keys)를 얻어서 붙여넣으세요.
  :::info
  [Brave Search API 계정](https://brave.com/search/api/)에 가입하고, 플랜을 선택한 후(무료 티어는 월 2000개 쿼리 제공), [개발자 대시보드](https://api-dashboard.search.brave.com/app/keys)에서 API 키를 생성하세요.
  :::

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
    │  brave-search
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-brave-search
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
    │  BRAVE_API_KEY
    │
    ◇  Environment variable value:
    │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪
    │
    ◇  Add another environment variable?
    │  No
    // highlight-end
    └  Added brave-search extension
  ```

  </TabItem>
</Tabs>

## 사용 예제

Brave Search MCP 서버는 Brave Search API에 대한 액세스를 제공하며, 웹 검색과 로컬 검색 기능을 모두 제공합니다:

- **웹 검색**: 일반 쿼리, 뉴스, 기사, 페이지네이션 및 신선도 제어 기능
- **로컬 검색**: 비즈니스, 레스토랑, 서비스에 대한 상세 정보 찾기 (Pro API 키 필요)
- **유연한 필터링**: 결과 유형, 안전 수준, 콘텐츠 신선도 제어
- **스마트 폴백**: 로컬 검색 결과가 없으면 자동으로 웹 검색으로 전환


#### goose 프롬프트

```
( O)> Organize a Italian dinner party with Ian, Rizel, Ace, and Ebony. Find 3 or 4 YouTube cooking channels (with URLs) that focus on Italian cuisine for beginners, and then find one recipe (with URL) for each person to make: one salad, two entrees, and one dessert.
```

#### goose 출력

```

I'll help you organize the Italian dinner party and search for appropriate cooking resources and recipes. Let me break this down into parts.

First, let's search for some beginner-friendly Italian cooking YouTube channels:

Brave Web Search
query: best youtube channels italian cooking for beginners authenti...

Let me search for some specific recipes:

Brave Web Search
query: authentic italian salad recipes panzanella caprese insalata

Brave Web Search
query: authentic italian main course pasta recipes carbonara osso b...

Brave Web Search
query: authentic italian dessert recipes tiramisu panna cotta


Based on my research, I'll organize the dinner party plan with recommended YouTube channels and specific recipes for each person. Here's the plan:

Recommended Italian Cooking YouTube Channels for Beginners

1. Vincenzo's Plate (https://www.youtube.com/channel/UCcsSowAamCLJv-xeF9geXoA)
  - Authentic Italian recipes with clear instructions
  - Run by an Italian native who explains techniques well for beginners

2. Italia Squisita (https://www.youtube.com/c/ItaliaSquisita)
  - Features traditional Italian recipes
  - Often shows multiple ways to make classic dishes

3. Giallo Zafferano (https://www.youtube.com/user/GialloZafferano)
  - Italy's most popular cooking channel
  - Comprehensive collection of authentic Italian recipes


Recipe Assignments

1. Rizel - Salad Course
  - Classic Panzanella (Tuscan Bread and Tomato Salad)
  - Recipe: https://www.themediterraneandish.com/tomato-panzanella-salad-recipe/
  - Perfect summer salad with crusty bread, tomatoes, and fresh basil

2. Ian - First Entrée
  - Authentic Spaghetti Carbonara
  - Recipe: https://anitalianinmykitchen.com/spaghetti-carbonara/
  - Classic Roman pasta dish with eggs, pecorino, and guanciale

3. Ace - Second Entrée
  - Osso Buco alla Milanese
  - Recipe: https://www.pastagrammar.com/post/osso-buco-authentic-italian-veal-recipe
  - Traditional Milanese braised veal shanks with gremolata

4. Ebony - Dessert
  - Classic Vanilla Panna Cotta
  - Recipe: https://www.recipesfromitaly.com/panna-cotta-recipe/
  - Elegant, creamy dessert that can be made ahead


Tips for Success

- Watch the YouTube channels beforehand to familiarize yourself with Italian cooking techniques
- Make sure to read through the entire recipe before starting
- The Panna Cotta should be made at least 4 hours ahead (or the night before) to allow proper setting time
- The Panzanella can be assembled just before serving for the best texture
- The Osso Buco can be made slightly ahead and reheated, as it often tastes better the next day
- The Carbonara should be made just before serving and served immediately while hot

```
