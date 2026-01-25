---
title: YouTube Transcript 확장 기능
description: YouTube 비디오 자막에 액세스하기 위해 YouTube Transcript MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/N38u7hZqZJg" />

이 튜토리얼은 [YouTube Transcript MCP 서버](https://github.com/jkawamoto/mcp-youtube-transcript)를 goose 확장 기능으로 추가하여 YouTube 비디오 자막을 가져오고 작업할 수 있게 하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=--from&arg=git%2Bhttps%3A%2F%2Fgithub.com%2Fjkawamoto%2Fmcp-youtube-transcript&arg=mcp-youtube-transcript&id=youtube-transcript&name=YouTube%20Transcript&description=Access%20YouTube%20video%20transcripts)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  uvx --from git+https://github.com/jkawamoto/mcp-youtube-transcript mcp-youtube-transcript
  ```
  </TabItem>
</Tabs>
:::

## 구성


:::info
이 명령을 실행하려면 [uv](https://docs.astral.sh/uv/#installation)가 시스템에 설치되어 있어야 합니다(`uvx` 사용).
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseDesktopInstaller
    extensionId="youtube-transcript"
    extensionName="YouTube Transcript"
    description="YouTube 비디오 자막에 액세스"
    command="uvx"
    args={["--from", "git+https://github.com/jkawamoto/mcp-youtube-transcript", "mcp-youtube-transcript"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="YouTube Transcript"
      description="YouTube 비디오 자막에 액세스"
      command="uvx --from git+https://github.com/jkawamoto/mcp-youtube-transcript mcp-youtube-transcript"
    />
  </TabItem>
</Tabs>

## 사용 예시

YouTube Transcript 확장 기능을 사용하면 YouTube 비디오에서 자막을 가져와 작업할 수 있습니다. 자막을 가져오려는 YouTube URL에서 비디오 ID가 필요합니다.

### goose 프롬프트

```
이 YouTube 비디오의 자막을 가져와줘: https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

### goose 출력

:::note CLI
해당 비디오의 자막을 가져오는 것을 도와드리겠습니다. 비디오 ID는 "dQw4w9WgXcQ"입니다. 자막을 가져오겠습니다.

자막입니다:

[타임스탬프와 텍스트가 포함된 자막 내용이 여기에 표시됩니다]

Rick Astley의 "Never Gonna Give You Up" 뮤직 비디오의 자막을 가져왔습니다. 자막에는 노래 가사가 표시되지만 자동화된 시스템의 특성상 일부 사소한 전사 오류가 있습니다. 자막에는 "릭롤링"에 자주 사용되는 인터넷 밈으로 가장 잘 알려진 이 유명한 1987년 노래의 상징적인 코러스와 절이 포함되어 있습니다.

비디오나 자막에 대해 다른 도움이 필요하신가요?
:::
