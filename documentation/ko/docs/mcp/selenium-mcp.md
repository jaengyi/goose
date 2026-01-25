---
title: Selenium 확장 프로그램
description: Selenium MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/PLqPOEeGPLc" />


이 튜토리얼에서는 웹 페이지 탐색과 양식 작성과 같은 브라우저 상호 작용을 자동화하기 위해 [Selenium MCP 서버](https://github.com/angiejones/mcp-selenium)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40angiejones%2Fmcp-selenium&id=selenium-mcp&name=Selenium%20MCP&description=automates%20browser%20interactions)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @angiejones/mcp-selenium
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::


<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="selenium-mcp"
    extensionName="Selenium MCP"
    description="브라우저 상호 작용 자동화"
    command="npx"
    args={["-y", "@angiejones/mcp-selenium"]}
  />
</TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Selenium"
      description="브라우저 상호 작용 자동화"
      command="npx -y @angiejones/mcp-selenium"
    />
  </TabItem>
</Tabs>

## 사용 예제

goose를 사용하여 테스트 자동화 프로젝트를 처음부터 만들어 보겠습니다! Selenium MCP를 사용하여 웹 양식 작성을 자동화한 다음, goose가 코드가 포함된 Selenium 프로젝트를 생성하여 필요할 때 이러한 테스트를 다시 실행할 수 있도록 합니다.


### goose 프롬프트

> Use selenium to go to the heroku formy site and fill out the form page with generic data. then can you turn what you've done into an automation script for me? I would like it in Java. Also use the Page Object Model pattern.


### goose 출력

<iframe class="aspect-ratio" src="https://www.youtube.com/embed/mRV0N8hcgYA?start=28&end=152" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
