---
title: Reddit 확장 프로그램

description: Reddit MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/VBtFinHieHA" />


이 튜토리얼에서는 인기 스레드 가져오기, Reddit 게시물 콘텐츠 분석, 서브레딧 토론 탐색을 위해 [Reddit MCP 서버](https://github.com/adhikasp/mcp-reddit)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=--from&arg=git%2Bhttps%3A%2F%2Fgithub.com%2Fadhikasp%2Fmcp-reddit.git&arg=mcp-reddit&id=reddit&name=Reddit&description=Fetch%20and%20analyze%20Reddit%20content)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  uvx --from git+https://github.com/adhikasp/mcp-reddit.git mcp-reddit
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="reddit"
      extensionName="Reddit"
      description="Reddit 콘텐츠 가져오기 및 분석"
      command="uvx"
      args={["--from", "git+https://github.com/adhikasp/mcp-reddit.git", "mcp-reddit"]}
    />
  </TabItem>

  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Reddit MCP"
      description="Reddit 콘텐츠 가져오기 및 분석"
      command="uvx --from git+https://github.com/adhikasp/mcp-reddit.git mcp-reddit"
      timeout={300}
    />
  </TabItem>

</Tabs>

## 사용 예제

이 예제에서는 goose와 Reddit MCP 서버를 사용하여 AI 주제에 대한 맞춤형 뉴스 피드를 만들겠습니다. goose가 Reddit에서 상위 AI 스레드를 가져와 이미지와 하이라이트가 포함된 스크롤 가능한 소셜 미디어 스타일 피드로 변환합니다.

Reddit과 같지만, 당신만을 위해 큐레이션된 것입니다.

### goose 프롬프트

> goose reddit MCP 서버를 사용하여 최신 AI 스레드의 개인화된 소셜 피드를 만들어 주세요. 이미지를 포함하고 실제 소셜 미디어 앱처럼 포맷해 주세요.



### goose 출력

:::note Desktop

Reddit 스레드와 서브스레드를 사용하여 개인화된 소셜 미디어 피드를 만드는 것을 도와드리겠습니다. 먼저 작업할 콘텐츠를 확인한 다음 웹 앱을 만들겠습니다.

두 서브레딧에서 샘플 게시물을 가져와 데이터 구조를 확인해 보겠습니다:


──────────────  Fetch Reddit Hot Threads ─────────────────

자바스크립트를 사용하여 웹 앱을 만들겠습니다:

──────────────  Text Editor ─────────────────

기본 브라우저에서 웹 앱을 열었습니다. 이제 개인화된 소셜 미디어 피드를 볼 수 있습니다.

페이지는 기본적으로 r/ProgrammerAIHumor에서 최신 게시물을 자동으로 가져오며, 상단의 버튼을 클릭하여 r/AI로 전환할 수 있습니다. 각 게시물은 클릭 가능하며 새 탭에서 원본 Reddit 스레드로 이동합니다.

조정하고 싶은 것이 있으면 알려주세요:

- 색상 구성표
- 레이아웃
- 새로고침 속도
- 표시되는 게시물 수

기타 기능도 가능합니다!

또한, 페이지 보기에 문제가 있으면 알려주시면 문제 해결을 도와드리겠습니다.
:::
