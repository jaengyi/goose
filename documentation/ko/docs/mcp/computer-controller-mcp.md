---
title: Computer Controller 확장 프로그램
description: Computer Controller MCP 서버를 goose 확장 프로그램으로 사용하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/EuMzToNOQtw" />

Computer Controller 확장 프로그램은 웹 검색, 시스템 설정 제어, 데이터 파일 처리, 코딩 없이 애플리케이션 제어와 같은 일상적인 컴퓨터 작업과 웹 상호 작용을 자동화하는 데 도움이 됩니다.

이 튜토리얼에서는 goose에 내장된 확장 프로그램인 Computer Controller MCP 서버를 활성화하고 사용하는 방법을 다룹니다.

:::tip
goose가 작업을 완료할 때까지 중단하지 마세요 - 완료될 때까지 마우스나 키보드 사용을 피하세요.
:::

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Computer Controller"
    description="일상적인 컴퓨터 작업 및 웹 상호 작용 자동화"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Toggle Extensions` 선택
  ```sh
  ┌   goose-configure
  │
  ◇  What would you like to configure?
  │  Toggle Extensions
  │
  ◆  Enable extensions: (use "space" to toggle and "enter" to submit)
  // highlight-start
  │  ● computercontroller
  // highlight-end
  |
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose가 시스템 제어, 음악 재생부터 웹 조사, 데이터 정리까지 모든 것을 처리하며 멀티태스킹하는 방법을 보여드리겠습니다.

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   1. goose Desktop에서 새 세션 열기
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. 터미널을 열고 새 goose 세션 시작:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

### goose 프롬프트
```
goose, I need to decompress while researching AI models for tomorrow's meeting. Can you create a relaxing environment and help me research?

1. Play classical music in Safari
2. Dim screen brightness (2 levels)
3. Research current flagship AI models (Anthropic, OpenAI, Google):
    * Verify availability
    * Collect: release date, features, pricing, context window
    * Use official sources only
4. Create "ai-models-comparison.csv" on desktop with collected data
5. Launch Numbers and open the CSV file
6. Reset brightness and close Safari
```

### goose 출력
```md
I'll help you create a relaxing research environment and gather information about AI models. I'll break this down into steps:

1. First, let's set up the relaxing environment with classical music and dimmed screen:

─── computer_control | computercontroller ──────────────────────────
script: ...



2. Now, let's research the AI models. I'll use web search and scraping to gather
 information from official sources:

─── web_search | computercontroller ──────────────────────────
query: Claude




─── web_search | computercontroller ──────────────────────────
query: GPT4




─── web_search | computercontroller ──────────────────────────
query: Gemini



3. Let's create the CSV file with the collected information:

─── automation_script | computercontroller ──────────────────────────
language: shell
script: ...



4. Now, let's open the CSV file in Numbers and reset the environment:

─── computer_control | computercontroller ──────────────────────────
script: ...

I've completed all the requested tasks:
1. ✅ Played classical music in Safari
2. ✅ Dimmed screen brightness
3. ✅ Researched current flagship AI models
4. ✅ Created the CSV file on desktop
5. ✅ Launched Numbers and opened the CSV file
6. ✅ Reset brightness and closed Safari

The CSV file has been created on your desktop with the following information for each model:
- Model name
- Company
- Release date
- Key features
- Context window size
- Pricing information

The file is now open in Numbers for your review. The environment has been reset to normal settings. Is there anything specific about any of the models you'd like me to elaborate on?
```

### 결과
![Computer Controller MCP 출력](../assets/guides/computer-controller-csv-result.png)
