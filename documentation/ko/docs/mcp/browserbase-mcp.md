---
title: Browserbase 확장 프로그램
description: Browserbase MCP 서버를 goose 확장 프로그램으로 추가하여 웹 자동화하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

이 튜토리얼에서는 브라우저 자동화를 위해 Browserbase MCP 서버를 goose 확장 프로그램으로 추가하는 방법을 다룹니다. 탐색, 페이지 상호 작용, 콘텐츠 캡처에 대한 프로그래밍 방식의 제어가 가능해집니다.

:::tip 요약

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=@browserbasehq/mcp&id=browserbase&name=Browserbase&description=Automate%20web%20browsing%20and%20data%20extraction&env=BROWSERBASE_PROJECT_ID%3DBrowserbase%20Project%20ID&env=BROWSERBASE_API_KEY%3DBrowserbase%20API%20Key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx @browserbasehq/mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  BROWSERBASE_PROJECT_ID: <YOUR_PROJECT_ID>
  BROWSERBASE_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="browserbase"
    extensionName="Browserbase"
    description="웹 브라우징 및 데이터 추출 자동화"
    command="npx"
    args={["@browserbasehq/mcp"]}
    envVars={[
      { name: "BROWSERBASE_PROJECT_ID", label: "Browserbase Project ID" },
      { name: "BROWSERBASE_API_KEY", label: "Browserbase API Key" }
    ]}
    apiKeyLink="https://browserbase.io/dashboard"
    apiKeyLinkText="Browserbase credentials"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Browserbase"
      description="웹 브라우징 및 데이터 추출 자동화"
      command="npx @browserbasehq/mcp"
      envVars={[
        { key: "BROWSERBASE_PROJECT_ID", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" },
        { key: "BROWSERBASE_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          <a href="https://browserbase.io/dashboard" target="_blank" rel="noopener noreferrer">Browserbase 자격 증명</a>을 얻어서 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예제

Browserbase 확장 프로그램을 사용하여 GitHub에서 트렌딩 MCP 관련 저장소에 대한 정보를 수집해 보겠습니다.

:::info LLM
이 작업에는 Claude 4 Sonnet이 사용되었습니다.
:::

### goose 프롬프트

```
1. Go to https://github.com/trending

2. Search for "MCP server" or scroll through and find repos with "MCP" or "Model Context Protocol" in the title or description

3. For the top 3 relevant repositories:
    • Click into the repo
    • Take a screenshot of the repo page
    • Extract:
        - Repo name
        - Description
        - Stars today
        - First paragraph of README
    • Navigate back

- Close all tabs

- Output a Markdown report showing:
    • Repo name (link)
    • Screenshot
    • Description + Stars
    • README excerpt
```

### goose 출력

```
# MCP Repositories Report

## 1. [FastMCP](https://github.com/jlowin/fastmcp)

![FastMCP Repository Screenshot]

**Description**: The fast, Pythonic way to build MCP servers and clients.
**Stars Today**: 10.2k stars

**README Excerpt**: The Model Context Protocol (MCP) is a new, standardized way to provide context and tools to your LLMs, and FastMCP makes building MCP servers and clients simple and intuitive. Create tools, expose resources, define prompts, and connect components with clean, Pythonic code.

## 2. [GitHub MCP Server](https://github.com/github/github-mcp-server)

![GitHub MCP Server Screenshot]

**Description**: GitHub's official MCP Server that provides seamless integration with GitHub APIs, enabling advanced automation and interaction capabilities for developers and tools.
**Stars Today**: 13.9k stars

**README Excerpt**: The GitHub MCP Server is a Model Context Protocol (MCP) server that provides seamless integration with GitHub APIs, enabling advanced automation and interaction capabilities for developers and tools.

## 3. [Playwright MCP](https://github.com/microsoft/playwright-mcp)

![Playwright MCP Screenshot]

**Description**: A Model Context Protocol (MCP) server that provides browser automation capabilities using Playwright.
**Stars Today**: 10.2k stars

**README Excerpt**: A Model Context Protocol (MCP) server that provides browser automation capabilities using Playwright. This server enables LLMs to interact with web pages through structured accessibility snapshots, bypassing the need for screenshots or visually-tuned models.
```
