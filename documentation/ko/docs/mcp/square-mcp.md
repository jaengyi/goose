---
title: Square MCP 확장 프로그램
description: Square API를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<details>
  <summary> 🎥 Square MCP 서버 비디오 안내</summary>
  <iframe
  class="aspect-ratio"
  src="https://www.youtube.com/embed/y6pklrzhzNg"
  title="Run your Business with AI | Square MCP Server"
  frameBorder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  ></iframe>
</details>


이 튜토리얼에서는 Square 판매자 계정의 대화형 및 자동화된 작업을 활성화하기 위해 [Square MCP 서버](https://developer.squareup.com/docs/mcp)를 시작하는 방법을 안내합니다. Square MCP 서버는 goose를 사용하여 Square API와 상호 작용할 수 있게 해주는 오픈 소스 프로젝트입니다.

Square는 두 가지 버전의 MCP 서버를 제공합니다:

1. Square가 호스팅하는 **원격 MCP 서버**로, OAuth를 사용하여 인증하고 API 사용에 대한 세분화된 권한을 허용합니다.
2. 자체 머신에서 실행할 수 있는 **로컬 MCP 서버**로, 액세스 토큰을 사용하여 인증하고 전체 API 액세스를 허용합니다.

:::info
설치 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

## 구성

<Tabs groupId="remote-or-local">
  <TabItem value="remote" label="Square Remote MCP" default>
  :::tip 요약
  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
    [설치 프로그램 실행](https://mcp.squareup.com/goose)
    </TabItem>
    <TabItem value="cli" label="goose CLI">
    **Command**
    ```sh
    npx mcp-remote https://mcp.squareup.com/sse
    ```
    </TabItem>
  </Tabs>
  :::

  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
     1. [설치 프로그램 실행](https://mcp.squareup.com/goose)
     2. `OK`를 클릭하여 설치 확인
     3. goose가 OAuth 권한 페이지로 브라우저 탭을 열어야 합니다. 허용할 권한을 다시 확인하고 `Grant Access`를 클릭합니다.
     4. Square에 로그인하거나 재인증하라는 메시지가 표시될 수 있으며, 허용할 권한을 확인하도록 요청할 수 있습니다.
     5. goose에서 채팅으로 이동합니다

    </TabItem>
    <TabItem value="cli" label="goose CLI">
      <CLIExtensionInstructions
        name="square-mcp-remote"
        type="stdio"
        command="npx mcp-remote https://mcp.squareup.com/sse"
        timeout={300}
      />

      다음 세션을 시작하면 goose가 브라우저를 열어 Square 계정에 권한을 부여하고 로그인할 수 있습니다.

    </TabItem>
  </Tabs>
</TabItem>

  <TabItem value="local" label="Square Local MCP">
  :::tip 요약
  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
    [설치 프로그램 실행](goose://extension?cmd=npx&arg=square-mcp-server&arg=start&id=mcp_square_api&name=Square%20MCP%20Server&description=Square%20API%20MCP%20Server&env=ACCESS_TOKEN%3DYour%20Access%20Token&env=SANDBOX%3Dtrue)

    </TabItem>
    <TabItem value="cli" label="goose CLI">
    **Command**
    ```sh
    npx square-mcp-server start
    ```
    </TabItem>
  </Tabs>
    **환경 변수**
    ```
    ACCESS_TOKEN: <YOUR_API_KEY>
    SANDBOX: <true/false>
    PRODUCTION: <true/false>
    ```

    `SANDBOX` 또는 `PRODUCTION` 중 하나만 사용하며 둘 다 사용하지 않습니다. `ACCESS_TOKEN`은 선택한 환경에 따라 샌드박스 또는 프로덕션 토큰이 됩니다.
  :::

  <Tabs groupId="interface">
    <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="mcp_square_api"
      extensionName="Square MCP Server"
      description="Square API MCP 서버"
      command="npx"
      args={["square-mcp-server", "start"]}
      envVars={[
        { name: "ACCESS_TOKEN", label: "Your Access Token" },
        { name: "SANDBOX", label: "true" }
      ]}
      appendToStep3="SANDBOX 또는 PRODUCTION 중 하나를 true로 설정하세요 (액세스 토큰은 환경과 일치해야 함)"
      apiKeyLink="https://developer.squareup.com/apps"
      apiKeyLinkText="Square Access Token"
    />
    </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="square-mcp"
      type="stdio"
      command="npx square-mcp-server start"
      timeout={300}
      envVars={[
        { key: "ACCESS_TOKEN", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" },
        { key: "SANDBOX", value: "true" }
      ]}
      infoNote={
        <>
          <a href="https://developer.squareup.com/apps" target="_blank" rel="noopener noreferrer">Square Access Token</a>을 얻어서 붙여넣으세요. <code>SANDBOX</code> 또는 <code>PRODUCTION</code> 중 하나를 true로 설정하세요 (액세스 토큰은 환경과 일치해야 함)
        </>
      }
    />
    </TabItem>
  </Tabs>

  </TabItem>
</Tabs>

## 사용 예제

Square MCP 서버를 사용하면 Square API 생태계에 액세스하기 위한 광범위한 [서비스 카탈로그](https://github.com/square/square-mcp-server?tab=readme-ov-file#service-catalog)로 Square의 연결 API와 상호 작용할 수 있습니다.

#### goose 프롬프트

```
( O)> 다음 이미지에서 내 음식 메뉴를 찾아 Square 판매자 계정에 가져와 주세요. "my-burger-menu.png"
```

[데모에 사용된 메뉴 이미지입니다.](../assets/guides/square-mcp-goosin-menu.png)

#### goose 출력

```
이 메뉴 항목을 Square 카탈로그에 가져오는 것을 도와드리겠습니다. Square API를 사용하여 각 버거에 대한 카탈로그 항목을 생성하겠습니다. 단계별로 나누어 보겠습니다:

1. 먼저, 카탈로그 API 서비스 정보와 유형을 확인하겠습니다
2. 그런 다음 각 버거에 대한 카탈로그 항목을 생성하겠습니다

이제 카탈로그 객체 생성을 위한 유형 정보를 확인하겠습니다

이제 메뉴의 각 버거에 대한 카탈로그 항목을 생성하기 시작하겠습니다. 카탈로그 API를 사용하여 하나씩 생성하겠습니다. 각 버거를 처리해 보겠습니다:

(출력 생략)

메뉴의 모든 버거 항목을 Square 카탈로그에 성공적으로 생성했습니다. 각 버거는 다음과 함께 추가되었습니다:

- 메뉴의 정확한 이름
- 전체 설명
- 정확한 가격
- 주문을 위한 표준 변형

생성된 항목 요약:
- The Classic - $10.00
- Cheeseburger - $11.00
- Primetime Burger - $12.00
- Double Bacon - $14.00
- Texas Meatlover - $12.50
- Buffalo Burger - $12.00
- The Atomic - $12.50
- Little Bird - $11.00
- Mushroom Burger - $10.00

이제 모든 항목이 Square 카탈로그에서 사용 가능하며 POS 시스템에서 사용할 준비가 되었습니다. 각 버거는 전체 설명을 포함하며 지정된 가격의 "Regular" 변형이 함께 제공됩니다. 항목은 기본적으로 판매 가능 및 재고 관리 가능으로 설정되며 모든 위치에서 사용할 수 있습니다.
```

