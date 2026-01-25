---
title: VS Code 확장 기능
description: 파일 작업 및 VS Code 통합을 위해 VS Code MCP 서버를 goose 확장 기능으로 사용
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/gddEgvCLrgU" />

이 튜토리얼은 [VS Code MCP 서버](https://github.com/block/vscode-mcp)를 goose 확장 기능으로 추가하여 VS Code 통합, 파일 작업 및 개발 워크플로우 관리를 활성화하는 방법을 다룹니다.

:::tip 요약

**명령**
```sh
npx vscode-mcp-server
```

**필수 설정**

Visual Studio Marketplace에서 [VS Code MCP Extension](https://marketplace.visualstudio.com/items?itemName=block.vscode-mcp-extension)을 설치하세요.
:::

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용).
:::

1. VS Code에 [VS Code MCP Extension](https://marketplace.visualstudio.com/items?itemName=block.vscode-mcp-extension)을 추가하세요. VS Code에서 추가 설정은 필요하지 않습니다.

<Tabs groupId="interface">
  <TabItem value="cli" label="goose CLI" default>
  1. `configure` 명령을 실행합니다:
  ```sh
  goose configure
  ```

  2. `Command-line Extension`을 추가하도록 선택합니다
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension
    │
    ◆  What type of extension would you like to add?
    │  ○ Built-in Extension
    // highlight-start
    │  ● Command-line Extension (Run a local command or script)
    // highlight-end
    │  ○ Remote Extension
    └
  ```

  3. 확장 기능에 이름을 지정합니다
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    // highlight-start
    ◆  What would you like to call this extension?
    │  vscode-mcp
    // highlight-end
    └
  ```

  4. 명령을 입력합니다
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  vscode-mcp
    │
    // highlight-start
    ◆  What command should be run?
    │  npx vscode-mcp-server
    // highlight-end
    └
  ```

  5. 시간 초과 값을 입력합니다 (기본값 300초 권장)
    ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  vscode-mcp
    │
    ◇  What command should be run?
    │  npx vscode-mcp-server install
    │
    // highlight-start
    ◆  Please set the timeout for this tool (in secs):
    │  300
    // highlight-end
    │
    └
  ```

  6. 기본 설정에 추가 환경 변수는 필요하지 않습니다

  </TabItem>
  <TabItem value="ui" label="goose 데스크톱">
  1. [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=vscode-mcp-server&id=vscode-mcp&name=VS%20Code%20MCP&description=VS%20Code%20integration%20and%20file%20operations)
  2. `Yes`를 눌러 설치를 확인합니다
  3. `Save Configuration`을 클릭합니다
  4. 왼쪽 상단에서 `Exit`을 클릭합니다
  </TabItem>
</Tabs>



## 사용 예시

VS Code MCP 확장 기능을 사용하면 goose가 VS Code 환경과 상호작용하여 파일, 프로젝트 및 개발 워크플로우를 관리할 수 있습니다.

VS Code MCP 서버의 주요 강점은 다음과 같습니다:

- 변경하기 전에 diff 표시
- VS Code 인터페이스와 파일 작업 통합
- 프로젝트 워크스페이스 관리
- 에디터에서 즉각적인 시각적 피드백 제공


:::note
VS Code MCP 서버가 활성화된 goose 세션을 시작할 때마다 VS Code에서 일치하는 프로젝트가 열려 있는지 확인합니다. 그렇지 않으면 진행하기 전에 프로젝트를 열라는 프롬프트가 표시됩니다.
:::

### goose 프롬프트

```
docusaurus 개발 서버를 시작하는 방법에 대한 지침으로 기여 가이드를 업데이트해줘
```

## 결과

![goose가 업데이트를 수행하고 VS Code가 변경 사항을 수락하거나 거부하라는 프롬프트를 표시하는 세션](../assets/guides/vscode-mcp.png)
