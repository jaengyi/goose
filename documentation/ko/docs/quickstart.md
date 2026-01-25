---
sidebar_position: 1
title: 빠른 시작
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Link from "@docusaurus/Link";
import { IconDownload } from "@site/src/components/icons/download";
import { RateLimits } from '@site/src/components/RateLimits';
import { ModelSelectionTip } from '@site/src/components/ModelSelectionTip';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import MacDesktopInstallButtons from '@site/src/components/MacDesktopInstallButtons';
import WindowsDesktopInstallButtons from '@site/src/components/WindowsDesktopInstallButtons';
import LinuxDesktopInstallButtons from '@site/src/components/LinuxDesktopInstallButtons';
import { PanelLeft } from 'lucide-react';

# 5분 만에 시작하는 goose

goose는 코딩 작업을 자동화하여 소프트웨어 개발을 향상시키는 확장 가능한 오픈 소스 AI 에이전트입니다.

이 빠른 튜토리얼에서는 다음 내용을 안내합니다:

- ✅ goose 설치하기
- ✅ LLM 구성하기
- ✅ 간단한 앱 만들기
- ✅ MCP 서버 추가하기

시작해봅시다 🚀

## goose 설치

<Tabs>
  <TabItem value="mac" label="macOS" default>
    goose 데스크톱 및/또는 CLI 버전 중 설치할 버전을 선택하세요:

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        <MacDesktopInstallButtons/>
        <div style={{ marginTop: '1rem' }}>
          1. 다운로드한 zip 파일의 압축을 해제합니다.
          2. 실행 파일을 실행하여 goose 데스크톱 애플리케이션을 시작합니다.
        </div>
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        다음 명령어를 실행하여 goose를 설치하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
        ```
      </TabItem>
    </Tabs>
  </TabItem>

  <TabItem value="linux" label="Linux">
    goose 데스크톱 및/또는 CLI 버전 중 설치할 버전을 선택하세요:

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        <LinuxDesktopInstallButtons/>
        <div style={{ marginTop: '1rem' }}>
          **Debian/Ubuntu 기반 배포판의 경우:**
          1. DEB 파일을 다운로드합니다
          2. 터미널에서 저장된 디렉토리로 이동합니다
          3. `sudo dpkg -i (파일명).deb`를 실행합니다
          4. 앱 메뉴에서 goose를 실행합니다

        </div>
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        다음 명령어를 실행하여 Linux에 goose CLI를 설치하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
        ```
      </TabItem>
    </Tabs>
  </TabItem>

  <TabItem value="windows" label="Windows">
    goose 데스크톱 및/또는 CLI 버전 중 설치할 버전을 선택하세요:

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        <WindowsDesktopInstallButtons/>
        <div style={{ marginTop: '1rem' }}>
          1. 다운로드한 zip 파일의 압축을 해제합니다.
          2. 실행 파일을 실행하여 goose 데스크톱 애플리케이션을 시작합니다.
        </div>
      </TabItem>
      <TabItem value="cli" label="goose CLI">

        **Git Bash**, **MSYS2** 또는 **PowerShell**에서 다음 명령어를 실행하여 Windows에 goose CLI를 네이티브로 설치하세요:

        ```bash
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
        ```

        필수 요구사항에 대해서는 [설치 가이드](/docs/getting-started/installation)를 참조하세요.

        :::info PATH 경고 및 키링
        설치 후 PATH 경고가 표시되면 `goose configure`를 실행하기 전에 goose를 PATH에 추가해야 합니다. 자세한 단계는 [Windows CLI 설치 지침](/docs/getting-started/installation)을 참조하세요.

        구성 중에 메시지가 표시되면 키링에 저장하지 않도록 선택하세요. 키링 오류가 발생하면 [Windows 설정 지침](/docs/getting-started/installation#set-llm-provider)에서 자세한 정보를 확인하세요.
        :::

      </TabItem>
    </Tabs>
  </TabItem>
</Tabs>

## 공급자 구성

goose는 요청을 이해하는 데 필요한 AI 지능을 제공하는 [지원되는 LLM 공급자](/docs/getting-started/providers)와 함께 작동합니다. 처음 사용 시 선호하는 공급자를 구성하라는 메시지가 표시됩니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  시작 화면에서 세 가지 옵션이 있습니다:
  - **[Tetrate Agent Router](https://tetrate.io/products/tetrate-agent-router-service)로 자동 설정**
  - **[OpenRouter](https://openrouter.ai/)로 자동 설정**
  - **기타 공급자**

  이 빠른 시작에서는 `Tetrate Agent Router로 자동 설정`을 선택하세요. Tetrate는 내장된 속도 제한 및 자동 장애 조치 기능과 함께 여러 AI 모델에 대한 액세스를 제공합니다. OpenRouter 또는 기타 공급자에 대한 자세한 정보는 [LLM 공급자 구성](/docs/getting-started/providers)을 참조하세요.

  goose가 브라우저를 열어 Tetrate로 인증하거나, 계정이 없는 경우 새 계정을 만들 수 있습니다. goose 데스크톱 앱으로 돌아오면 첫 번째 세션을 시작할 준비가 된 것입니다.

  :::info 무료 크레딧 제공
  goose를 통해 Tetrate로 처음 자동 인증하면 $10의 무료 크레딧을 받게 됩니다. 이 혜택은 신규 및 기존 Tetrate 사용자 모두에게 제공됩니다.
  :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">
  1. 터미널에서 다음 명령어를 실행하세요:

    ```sh
    goose configure
    ```

  2. 메뉴에서 `Configure Providers`를 선택하고 Enter를 누르세요.

    ```
   ┌   goose-configure
   │
   ◆  What would you like to configure?
   │  ● Configure Providers (Change provider or update credentials)
   │  ○ Add Extension
   │  ○ Toggle Extensions
   │  ○ Remove Extension
   │  ○ goose settings
   └
   ```
   3. 모델 공급자를 선택하세요. 이 빠른 시작에서는 `Tetrate Agent Router Service`를 선택하고 Enter를 누르세요. Tetrate는 내장된 속도 제한 및 자동 장애 조치 기능과 함께 여러 AI 모델에 대한 액세스를 제공합니다. 다른 공급자에 대한 정보는 [LLM 공급자 구성](/docs/getting-started/providers)을 참조하세요.

   ```
   ┌   goose-configure
   │
   ◇  What would you like to configure?
   │  Configure Providers
   │
   ◆  Which model provider should we use?
   │  ○ Anthropic
   │  ○ Azure OpenAI
   │  ○ Amazon Bedrock
   │  ○ Claude Code
   │  ○ Codex CLI
   │  ○ Databricks
   │  ○ Gemini CLI
   |  ● Tetrate Agent Router Service (Enterprise router for AI models)
   │  ○ ...
   └
   ```
    :::info 무료 크레딧 제공
    goose를 통해 Tetrate로 처음 자동 인증하면 $10의 무료 크레딧을 받게 됩니다. 이 혜택은 신규 및 기존 Tetrate 사용자 모두에게 제공됩니다.
    :::

   4. 메시지가 표시되면 API 키(및 기타 구성 세부정보)를 입력하세요.

   ```
   ┌   goose-configure
   │
   ◇  What would you like to configure?
   │  Configure Providers
   │
   ◇  Which model provider should we use?
   │  Tetrate Agent Router Service
   │
   ◆  Provider Tetrate Agent Router Service requires TETRATE_API_KEY, please enter a value
   │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪
   └
   ```
    :::tip GitHub Copilot 인증
    GitHub Copilot은 API 키를 사용하지 않습니다. 대신 구성 중에 인증 코드가 생성됩니다. 코드를 생성하려면 공급자로 `GitHub Copilot`을 선택하세요. 인증 코드가 클립보드에 복사되고 브라우저 창이 열리면 붙여넣기하여 인증을 완료할 수 있습니다.

    자세한 내용은 [GitHub Copilot 인증](/docs/getting-started/providers#github-copilot-authentication)을 참조하세요.
    :::

   5. 사용하려는 모델을 선택하거나 검색하세요.
   ```
   │
   ◇  Model fetch complete
   │
   ◆  Select a model:
   │  ○ Search all models...
   │  ○ gemini-2.5-pro
   │  ○ gemini-2.0-flash
   |  ○ gemini-2.0-flash-lite
   │  ● gpt-5 (Recommended)
   |  ○ gpt-5-mini
   |  ○ gpt-5-nano
   |  ○ gpt-4.1
   │
   ◓  Checking your configuration...
   └  Configuration saved successfully
   ```
  </TabItem>
</Tabs>

## 세션 시작
세션은 사용자와 goose 간의 단일 연속 대화입니다. 하나를 시작해봅시다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        LLM 공급자를 선택한 후 사이드바에서 `Home` 버튼을 클릭하세요.

        입력 필드에 질문, 작업 또는 지침을 직접 입력하면 goose가 즉시 작업을 시작합니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        1. 빈 디렉토리(예: `goose-demo`)를 만들고 터미널에서 해당 디렉토리로 이동하세요.
        2. 새 세션을 시작하려면 다음을 실행하세요:
        ```sh
        goose session
        ```

        :::tip goose 웹
        CLI 사용자는 웹 기반 채팅 인터페이스인 [goose 웹](/docs/guides/goose-cli-commands#web)에서도 세션을 시작할 수 있습니다:
        ```sh
        goose web --open
        ```
        :::

    </TabItem>
</Tabs>

## 프롬프트 작성

프롬프트에서 개발자에게 말하듯이 정확하게 지침을 입력하여 goose와 상호작용할 수 있습니다.

goose에게 틱택토 게임을 만들어달라고 요청해봅시다!

```
create an interactive browser-based tic-tac-toe game in javascript where a player competes against a bot
```

goose가 계획을 세우고 바로 구현 작업을 시작합니다. 완료되면 디렉토리에 JavaScript 파일과 플레이용 HTML 페이지가 포함되어 있어야 합니다.


## 확장 기능 활성화

작업 디렉토리로 수동으로 이동하여 브라우저에서 HTML 파일을 열 수 있지만, goose가 대신 해주면 더 좋지 않을까요? [`Computer Controller` 확장 기능](/docs/mcp/computer-controller-mcp)을 활성화하여 goose에게 웹 브라우저를 열 수 있는 기능을 부여해봅시다.

<Tabs groupId="interface">

    <TabItem value="ui" label="goose 데스크톱" default>
        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
        2. 사이드바 메뉴에서 `Extensions`를 클릭합니다.
        3. `Computer Controller` 확장 기능을 토글하여 활성화합니다. 이 확장 기능은 웹 스크래핑, 파일 캐싱 및 자동화를 가능하게 합니다.
        4. 세션으로 돌아가서 계속합니다.
        5. 이제 goose가 브라우저 기능을 갖추었으니 브라우저에서 게임을 실행해달라고 요청해봅시다:
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        1. `Ctrl+C`를 입력하여 현재 세션을 종료하고 터미널의 명령 프롬프트로 돌아갑니다.
        2. 구성 명령어를 실행합니다
        ```sh
        goose configure
        ```
        3. `Add Extension` > `Built-in Extension` > `Computer Controller`를 선택하고 시간 제한을 300초로 설정합니다. 이 확장 기능은 웹 스크래핑, 파일 캐싱 및 자동화를 가능하게 합니다.
        ```
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  Add Extension
        │
        ◇  What type of extension would you like to add?
        │  Built-in Extension
        │
        ◇  Which built-in extension would you like to enable?
        │  Computer Controller
        │
        ◇  Please set the timeout for this tool (in secs):
        │  300
        │
        └  Enabled computercontroller extension
        ```
        4. 이제 goose가 브라우저 기능을 갖추었으니 마지막 세션을 재개해봅시다:
        ```sh
         goose session -r
        ```
        5. goose에게 브라우저에서 게임을 실행해달라고 요청하세요:
    </TabItem>
</Tabs>

```
open the tic-tac-toe game in a browser
```

이제 게임을 플레이해보세요, 하고 싶은 거 알아요 😂 ... 행운을 빕니다!


## 다음 단계
축하합니다, goose를 사용하여 웹 앱을 성공적으로 개발했습니다! 🎉

다음 단계를 위한 몇 가지 아이디어입니다:
* goose와 세션을 계속하여 게임을 개선하세요 (스타일링, 기능 등).
* 사용 가능한 다른 [확장 기능](/extensions)을 탐색하고 더 설치하여 goose의 기능을 더욱 향상시키세요.
* 세션 내에서 사용할 [힌트 세트](/docs/guides/context-engineering/using-goosehints)를 goose에게 제공하세요.
* goose가 자율적으로 작업하지 않길 원하는 경우 [액세스 제어](/docs/mcp/developer-mcp#configuring-access-controls)를 설정하는 방법을 확인하세요.
