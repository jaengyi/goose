---
title: Tavily 웹 검색 확장 프로그램
description: Tavily MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://youtube.com/embed/mxS2G9afGxE" />

이 튜토리얼에서는 AI 기반 웹 검색 기능을 활성화하기 위해 [Tavily 웹 검색 MCP 서버](https://github.com/tavily-ai/tavily-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=tavily-mcp&id=tavily&name=Tavily%20Web%20Search&description=Search%20the%20web%20with%20Tavily%20MCP&env=TAVILY_API_KEY%3DTavily%20API%20Key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y tavily-mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  TAVILY_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="tavily"
    extensionName="Tavily Web Search"
    description="Tavily MCP로 웹 검색"
    command="npx"
    args={["-y", "tavily-mcp"]}
    envVars={[
      { name: "TAVILY_API_KEY", label: "Tavily API Key" }
    ]}
    apiKeyLink="https://tavily.com/"
    apiKeyLinkText="Tavily API Key"
  />
</TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Tavily Web Search"
      description="Tavily MCP로 웹 검색"
      command="npx -y tavily-mcp"
      envVars={[
        { key: "TAVILY_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          <a href="https://tavily.com/" target="_blank" rel="noopener noreferrer">Tavily API Key</a>를 얻어서 붙여넣으세요. <a href="https://app.tavily.com/" target="_blank" rel="noopener noreferrer">tavily.com</a>에서 가입하고 계정 설정으로 이동하여 API 키를 얻을 수 있습니다.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예제

Tavily MCP 서버는 goose 상호 작용에서 AI 기반 웹 검색 기능을 활성화합니다. API 키를 얻으면 다음과 같은 Tavily의 고급 검색 기능에 액세스할 수 있습니다:

1. 실시간 웹 검색
2. 구조화된 데이터 추출
3. 콘텐츠 분석
4. 주제별 검색

### goose 프롬프트

```
Search for recent news about artificial intelligence breakthroughs in medicine and summarize the key findings.
```

### goose 출력

```
I'll use the Tavily news search tool to find recent news about AI breakthroughs in medicine.

─── tavily_news_search | tavily ──────────────────────────
max_results: 5
query: ...



Let me summarize the key findings from the recent news about AI in medicine:

1. Personalized AI-Driven Healthcare:
- Development of AI systems that act as "personal doctors" that work alongside human clinicians rather than replacing them
- Focus on augmenting clinical capabilities while maintaining human oversight

2. Breakthrough in Medical Imaging:
- A significant advancement in CT scan analysis has been reported in Nature
- Development of a "Unified total-body CT image" (Uni-CT) algorithm that:
  - Automatically generates single series of total-body CT images
  - Concatenates multiple images of segmented organs using organ-specific window settings
  - Reduces diagnosis time by 35% without compromising accuracy
  - Improves efficiency in trauma cases

3. Future Trends:
- Movement toward democratized AI in healthcare, potentially reducing the gap between large and small healthcare providers
- Development of hyper-personalized medical experiences powered by AI
- Integration of AI systems that can perform human-like reasoning in medical contexts

The most concrete breakthrough appears to be in medical imaging, with the Uni-CT algorithm showing promising results in improving both the speed and accuracy of trauma diagnosis. This represents a practical application of AI that could have immediate benefits in emergency medicine.

Would you like me to search for more specific information about any of these developments or explore other areas of AI in medicine?
```

:::tip
다양한 유형의 쿼리와 정보 깊이에 맞게 [검색 매개변수](https://github.com/tavily-ai/tavily-mcp#tavily-search-examples)를 조정할 수 있습니다. 이 확장 프로그램은 빠른 검색과 포괄적인 리서치를 모두 지원합니다.
:::
