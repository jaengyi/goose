---
sidebar_position: 3
title: 확장 기능 사용하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Settings, Puzzle } from 'lucide-react';

확장 기능은 워크플로우에서 이미 사용하고 있는 애플리케이션 및 도구와 연결하여 goose의 기능을 확장하는 방법을 제공하는 추가 기능입니다. 이러한 확장 기능을 사용하여 새로운 기능을 추가하거나, 데이터와 리소스에 액세스하거나, 다른 시스템과 통합할 수 있습니다.

확장 기능은 [Model Context Protocol (MCP)](https://github.com/modelcontextprotocol)을 기반으로 하므로 goose를 광범위한 기능 생태계에 연결할 수 있습니다.

goose는 활성화 전에 외부 확장 기능을 알려진 맬웨어에 대해 자동으로 검사합니다. 악성 패키지가 감지되면 [확장 기능이 차단](/docs/troubleshooting/known-issues#malicious-package-detected)되고 명확한 오류 메시지가 표시됩니다.

:::tip 튜토리얼
다양한 goose 확장 기능을 추가하고 사용하는 [단계별 튜토리얼](/docs/category/mcp-servers)을 확인하세요
:::

## 내장 확장 기능
goose에는 바로 사용할 수 있는 여러 내장 확장 기능이 포함되어 있습니다:

- [Developer](/docs/mcp/developer-mcp): 소프트웨어 개발에 유용한 일반 개발 도구 세트를 제공합니다. Developer 확장 기능은 **기본적으로 활성화**되어 있습니다.
- [Computer Controller](/docs/mcp/computer-controller-mcp): 웹 스크래핑, 파일 캐싱 및 자동화를 위한 일반 컴퓨터 제어 도구를 제공합니다.
- [Memory](/docs/mcp/memory-mcp): goose가 사용하면서 사용자의 선호도를 기억하도록 가르칩니다.
- [Tutorial](/docs/mcp/tutorial-mcp): goose에 대해 배우기 위한 대화형 튜토리얼을 제공합니다.
- [Auto Visualiser](/docs/mcp/autovisualiser-mcp): 대화에서 그래픽 데이터 시각화를 자동으로 생성합니다.

:::warning 액세스 제어
goose는 기본적으로 자율적으로 작동합니다. Developer 확장 기능의 도구와 결합하면 goose가 사용자의 승인 없이 명령을 실행하고 파일을 수정할 수 있습니다. 이 동작을 더 많이 제어하려면 [goose 권한 모드](/docs/guides/goose-permissions), [도구 권한](/docs/guides/managing-tools/tool-permissions) 및 [.gooseignore 파일](/docs/guides/using-gooseignore)을 구성할 수 있습니다. 간략한 개요는 [액세스 제어 구성](/docs/mcp/developer-mcp#configuring-access-controls)을 참조하세요.
:::

### 내장 플랫폼 확장 기능

플랫폼 확장 기능은 대화 검색, 작업 추적 및 확장 관리와 같은 전역 기능을 제공하는 내장 확장 기능입니다. 이러한 확장 기능은 항상 사용할 수 있으며 필요에 따라 켜거나 끌 수 있습니다.

- [Chat Recall](/docs/mcp/chatrecall-mcp): 모든 세션 기록에서 대화 내용 검색
- [Code Execution](/docs/mcp/code-execution-mcp): 도구 검색 및 도구 호출을 위한 JavaScript 코드 실행
- [Extension Manager](/docs/mcp/extension-manager-mcp): 세션 중에 확장 기능을 동적으로 검색, 활성화 및 비활성화 (기본적으로 활성화됨)
- [Skills](/docs/mcp/skills-mcp): 다양한 프로젝트 및 전역 스킬 디렉토리에서 에이전트 스킬 로드 및 사용 (기본적으로 활성화됨)
- [Todo](/docs/mcp/todo-mcp): 세션 간에 작업 목록 관리 및 진행 상황 추적 (기본적으로 활성화됨)

### 내장 확장 기능 토글

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
  2. 사이드바에서 `Extensions` 버튼을 클릭합니다.
  3. `Extensions` 아래에서 내장 확장 기능을 켜거나 끌 수 있습니다.
  </TabItem>

  <TabItem value="cli" label="goose CLI">

    추가하려는 확장 기능의 정확한 이름을 알고 있다면 다음을 실행합니다:

    ```sh
    goose mcp {name}
    ```

    사용 가능한 확장 기능을 탐색하려면:

    1. 다음 명령어를 실행합니다:
        ```sh
        goose configure
        ```
    2. 메뉴에서 `Add Extension`을 선택합니다. 위아래 키를 사용하여 선택을 강조 표시한 다음 `Enter`를 누릅니다.
    3. `Built-In Extension`을 선택합니다.
    4. 활성화할 확장 기능을 선택합니다.
    5. 확장 기능의 시간 제한을 제공합니다 (초 단위).
    6. `Enter`를 누릅니다.

    **예시: 내장 확장 기능 추가**

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
    │  Auto Visualiser
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    └  Enabled Auto Visualiser extension
    ```
  </TabItem>
</Tabs>


:::info
goose의 내장 확장 기능은 자체적으로 MCP 서버입니다. goose에 포함된 MCP 서버를 다른 에이전트와 함께 사용하려면 자유롭게 사용할 수 있습니다.
:::

## 확장 기능 검색

goose는 설치하고 사용할 수 있는 [확장 기능 중앙 디렉토리][extensions-directory]를 제공합니다.

디렉토리에 나열되지 않은 다른 [MCP 서버](#mcp-servers)도 goose 확장 기능으로 추가할 수 있습니다.


## 확장 기능 추가

확장 기능은 [확장 기능 디렉토리][extensions-directory], CLI 또는 UI를 통해 직접 설치할 수 있습니다.

:::warning 에어갭 환경
기업 또는 에어갭 환경에 있고 확장 기능이 활성화되지 않으면 [에어갭/오프라인 환경](/docs/troubleshooting/known-issues#airgappedoffline-environment-issues)에서 해결 방법을 참조하세요.
:::

### MCP 서버

모든 MCP 서버를 goose 확장 기능으로 설치할 수 있습니다.

:::tip MCP 서버 디렉토리
**[MCP 서버 디렉토리](https://www.pulsemcp.com/servers)**에서 사용 가능한 서버를 확인하세요.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
  2. 사이드바에서 `Extensions` 버튼을 클릭합니다.
  3. `Extensions` 아래에서 `Add custom extension`을 클릭합니다.
  4. `Add custom extension` 모달에서 필요한 세부 정보를 입력합니다
     - 환경 변수를 추가하는 경우 변수 오른쪽의 `Add` 버튼을 클릭합니다
     - `Timeout` 필드를 사용하여 이 확장 기능의 도구 호출이 완료되기를 goose가 기다리는 시간을 설정할 수 있습니다
  5. `Add` 버튼을 클릭합니다

  #### [Knowledge Graph Memory MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/memory) 추가 예시:
    * **Type**: `Standard IO`
    * **ID**: `kgm-mcp` (_원하는 대로 설정_)
    * **Name**: `Knowledge Graph Memory` (_원하는 대로 설정_)
    * **Description**: `개념 간 복잡한 관계를 매핑하고 저장` (_원하는 대로 설정_)
    * **Command**: `npx -y @modelcontextprotocol/server-memory`
  </TabItem>

  <TabItem value="cli" label="goose CLI">

  1. 다음 명령어를 실행합니다:

    ```sh
    goose configure
    ```

  2. 메뉴에서 `Add Extension`을 선택합니다.

  3. 추가하려는 확장 기능 유형을 선택합니다:
      - `Built-In Extension`: goose와 함께 사전 설치된 확장 기능을 사용합니다.
      - `Command-Line Extension`: 확장 기능으로 실행할 로컬 명령 또는 스크립트를 추가합니다.
      - `Remote Extension (Streamable HTTP)`: Streamable HTTP를 통해 원격 시스템에 연결합니다

  4. 선택한 확장 기능 유형에 따라 프롬프트를 따릅니다.

  </TabItem>
</Tabs>

## 확장 기능 활성화/비활성화

설치된 확장 기능을 언제든지 활성화하거나 비활성화할 수 있습니다. 새 세션의 기본값으로 설정하거나 현재 세션에서 사용 중인 확장 기능을 변경할 수 있습니다.

### 새 세션의 기본 확장 기능 설정

기본 확장 기능에 대한 변경 사항은 향후 세션에 적용됩니다. 이러한 설정에 대한 업데이트는 현재 활성 세션에 영향을 주지 않습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
  2. 사이드바에서 `Extensions` 버튼을 클릭합니다.
  3. 확장 기능 옆의 토글 스위치를 사용하여 활성화하거나 비활성화합니다.

  </TabItem>

  <TabItem value="cli" label="goose CLI">

  1. goose 구성을 열려면 다음 명령어를 실행합니다:
      ```sh
      goose configure
      ```
  2. 메뉴에서 `Toggle Extensions`를 선택합니다.
  3. 이미 설치된 확장 기능 목록이 표시됩니다.
  4. `스페이스바`를 눌러 확장 기능을 토글합니다. 채워진 것은 활성화를 의미합니다.

  **예시:**

  ```
  ┌   goose-configure
  │
  ◇  What would you like to configure?
  │  Toggle Extensions
  │
  ◆  enable extensions: (use "space" to toggle and "enter" to submit)
  │  ◼ developer
  │  ◻ fetch
  └
  ```
  </TabItem>
</Tabs>

### 세션 중 확장 기능 변경

세션 중에 변경하면 다시 시작하지 않고 현재 대화가 유지됩니다. 세션 중 변경 사항은 현재 채팅 세션에만 적용되며 새 세션의 기본 확장 기능은 변경되지 않습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 앱 하단의 <Puzzle className="inline" size={16} /> 버튼을 클릭합니다.
  2. 확장 기능 옆의 토글 스위치를 사용하여 활성화하거나 비활성화합니다.

  </TabItem>

  <TabItem value="cli" label="goose CLI">

  슬래시 명령으로 대화형 세션 중에 확장 기능을 추가합니다:

  **stdio 확장 기능 추가:**
  ```bash
  /extension npx -y @modelcontextprotocol/server-memory
  ```

  **내장 확장 기능 추가:**
  ```bash
  /builtin developer
  ```
  </TabItem>
</Tabs>

## 자동으로 활성화되는 확장 기능

goose의 스마트 확장 기능 추천 시스템은 작업과 필요에 따라 관련 확장 기능을 자동으로 식별하고 제안합니다. 이 섹션에서는 이 기능을 효과적으로 사용하는 방법과 그 기능과 제한 사항을 이해하는 방법을 설명합니다.

작업을 요청하면 goose는 활성화된 확장 기능과 해당 도구를 확인하여 요청을 수행할 수 있는지 확인합니다. 그렇지 않으면 필요에 따라 추가 확장 기능을 제안하거나 활성화합니다. 이름으로 특정 확장 기능을 요청할 수도 있습니다.


:::warning
동적으로 활성화된 확장 기능은 현재 세션에 대해서만 활성화됩니다. 세션 간에 확장 기능을 계속 활성화하려면 [확장 기능 활성화/비활성화](#enablingdisabling-extensions)를 참조하세요.
:::

## 확장 기능 속성 업데이트

goose는 확장 기능 속성에 의존하여 확장 기능을 처리하는 방법을 결정합니다. 이름, 시간 제한 또는 환경 변수와 같은 확장 기능의 표시 설정과 동작을 변경하려면 이러한 속성을 편집할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
  2. 사이드바에서 `Extensions` 버튼을 클릭합니다.
  3. `Extensions` 아래에서 편집하려는 확장 기능의 <Settings className="inline" size={16} /> 버튼을 클릭합니다.
  4. 표시되는 대화 상자에서 필요에 따라 확장 기능의 속성을 편집합니다.
  5. `Save Changes`를 클릭합니다.

  </TabItem>

  <TabItem value="cli" label="구성 파일">

  1. goose [구성 파일](/docs/guides/config-files)로 이동합니다. 예를 들어 macOS에서 `~/.config/goose/config.yaml`로 이동합니다.
  2. 필요에 따라 확장 기능 속성을 편집하고 변경 사항을 저장합니다.

  </TabItem>
</Tabs>

## 확장 기능 제거

설치된 확장 기능을 제거할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
  2. 사이드바에서 `Extensions` 버튼을 클릭합니다.
  3. `Extensions` 아래에서 제거하려는 확장 기능의 <Settings className="inline" size={16} /> 버튼을 클릭합니다.
  4. 표시되는 대화 상자에서 `Remove Extension`을 클릭합니다.

  </TabItem>

  <TabItem value="cli" label="구성 파일">
  :::info
  확장 기능을 제거하려면 먼저 [비활성화](#enablingdisabling-extensions)해야 합니다.
  :::

    1. goose 구성을 열려면 다음 명령어를 실행합니다:
    ```sh
    goose configure
    ```
    2. 메뉴에서 `Remove`를 선택합니다. 비활성화된 확장 기능이 나열됩니다.
    3. 제거하려는 확장 기능으로 화살표를 내립니다.
    4. `스페이스바`를 눌러 확장 기능을 선택합니다. 채워진 것은 선택됨을 의미합니다.
    ```
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Remove Extension
    │
    ◆  Select extensions to remove (note: you can only remove disabled extensions - use "space" to toggle and "enter" to submit)
    │  ◼ fetch
    └
    ```
    5. Enter를 눌러 저장합니다
  </TabItem>
</Tabs>


## 확장 기능으로 세션 시작

CLI에서 특정 확장 기능으로 맞춤형 goose 세션을 직접 시작할 수 있습니다.

:::info 참고
* 확장 기능은 설치되지 않습니다. 현재 세션에 대해서만 활성화됩니다.
* 확장 기능이 이미 활성화되어 있으면 이 작업이 필요 없습니다.
:::

### 내장 확장 기능

세션을 시작하면서 내장 확장 기능을 활성화하려면 다음 명령어를 실행합니다:

```bash
goose session --with-builtin "{extension_id}"
```

예를 들어 Developer와 Computer Controller 확장 기능을 활성화하고 세션을 시작하려면 다음을 실행합니다:

```bash
goose session --with-builtin "developer,computercontroller"
```

또는:

```bash
goose session --with-builtin developer --with-builtin computercontroller
```


### 외부 확장 기능

세션을 시작하면서 확장 기능을 활성화하려면 다음 명령어를 실행합니다:

```bash
goose session --with-extension "{extension command}" --with-extension "{another extension command}"
```

예를 들어 [Fetch 확장 기능](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)으로 세션을 시작하려면 다음을 실행합니다:

```bash
goose session --with-extension "uvx mcp-server-fetch"
```


#### 환경 변수

일부 확장 기능에는 환경 변수가 필요합니다. 명령에 이를 포함할 수 있습니다:

```bash
goose session --with-extension "VAR=value command arg1 arg2"
```

예를 들어 [GitHub 확장 기능](https://github.com/github/github-mcp-server)으로 세션을 시작하려면 다음을 실행합니다:

```bash
goose session --with-extension "GITHUB_PERSONAL_ACCESS_TOKEN=<YOUR_TOKEN> npx -y @modelcontextprotocol/server-github"
```

:::info
이 명령을 실행하려면 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다. `npx`를 사용하기 때문입니다.
:::


### Streamable HTTP를 통한 원격 확장 기능

세션을 시작하면서 Streamable HTTP를 통해 원격 확장 기능을 활성화하려면 다음 명령어를 실행합니다:

```bash
goose session --with-streamable-http-extension "{extension URL}" --with-streamable-http-extension "{another extension URL}"
```

예를 들어 Streamable HTTP 확장 기능으로 세션을 시작하려면 다음을 실행합니다:

```bash
goose session --with-streamable-http-extension "https://example.com/streamable"
```

## 확장 기능 개발

goose 확장 기능은 AI 모델과 에이전트가 로컬 또는 원격 리소스에 안전하게 연결할 수 있도록 하는 표준 프로토콜인 MCP로 구현됩니다. [MCP 서버로 확장 기능](https://modelcontextprotocol.io/quickstart/server)을 빌드하는 방법을 알아보세요.

**튜토리얼:**
- [사용자 지정 확장 기능 빌드](/docs/tutorials/custom-extensions) - Python 기반 MCP 확장 기능 만들기
- [MCP 앱 빌드](/docs/tutorials/building-mcp-apps) - 대화형 UI 앱 만들기

[extensions-directory]: /extensions
