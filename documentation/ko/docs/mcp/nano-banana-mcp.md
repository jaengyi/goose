---
title: Nano Banana 확장 기능
description: Nano Banana MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';


<!--<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/VIDEO_ID" />-->


이 튜토리얼은 [Nano Banana MCP 서버](https://github.com/ConechoAI/Nano-Banana-MCP)를 goose 확장 기능으로 추가하여 Google의 Gemini Image API를 사용한 AI 이미지 생성 및 편집 기능을 제공하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=nano-banana-mcp&id=nano-banana&name=Nano%20Banana&description=Image%20generation%20with%20Gemini)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx nano-banana-mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  GEMINI_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용).
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    <GooseDesktopInstaller
      extensionId="nano-banana"
      extensionName="Nano Banana"
      description="Gemini를 사용한 이미지 생성"
      type="stdio"
      command="npx"
      args={["nano-banana-mcp"]}
      envVars={[
        { name: "GEMINI_API_KEY", label: "Gemini API Key" }
      ]}
      apiKeyLink="https://aistudio.google.com/api-keys"
      apiKeyLinkText="Gemini API Key"
    />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Nano Banana"
      description="Gemini를 사용한 이미지 생성"
      type="stdio"
      command="npx nano-banana-mcp"
      timeout={300}
      envVars={[
        { key: "GEMINI_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          <a href="https://aistudio.google.com/api-keys" target="_blank" rel="noopener noreferrer">
            Google AI Studio
          </a>에서 API 키를 받아 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예시

Nano Banana를 사용하면 자연어로 새 이미지를 생성하고 기존 이미지를 편집할 수 있습니다. 만들거나 수정하고 싶은 것을 설명하기만 하면 goose가 Nano Banana에 제공할 상세한 프롬프트로 변환합니다.

### 이미지 생성 예시
원하는 것을 설명하여 처음부터 이미지를 생성합니다:

- "산 위로 지는 석양 이미지를 만들어줘"
- "Bean There라는 커피숍 로고를 생성해줘"
- "보물더미를 지키는 판타지 드래곤을 만들어줘"

### 이미지 반복 예시
이미지를 생성하거나 편집한 후 계속 다듬을 수 있습니다:

- "하늘을 더 보라색으로 만들어줘"
- "구석에 고양이를 추가해줘"
- "텍스트를 제거해줘"
- "더 만화 스타일로 만들어줘"

### goose 프롬프트

> nano banana를 사용해서 얼어붙은 작업장에서 얼음 조각품을 만드는 판타지 얼음 마법사 이미지를 만들어줘

### goose 출력

:::note 데스크톱

🎨 nano-banana로 이미지 생성됨 (Gemini 2.5 Flash Image)!


프롬프트: "얼어붙은 작업장에서 얼음 조각품을 만드는 판타지 얼음 마법사. 마법사는 서리 무늬가 있는 흐르는 로브를 입고, 정교한 얼음 조각품을 만들면서 손에서 파란 마법 에너지가 빛납니다. 작업장은 아름다운 얼어붙은 창작물로 가득합니다 - 얼음 동물, 수정 꽃, 정교한 조각상. 천장에서 고드름이 매달려 있고, 돌벽에 서리가 덮여 있으며, 마법의 파란 빛이 공간을 비춥니다. 눈이 공기 중에 부드럽게 떠다닙니다. 마법의 분위기가 있는 상세한 판타지 아트 스타일."

✨ **완료!** 판타지 얼음 마법사 이미지입니다!

이미지가 아름답게 나왔습니다 — 다음을 특징으로 합니다:

- **얼음 마법사**: 정교한 서리와 눈송이 자수가 있는 우아한 짙은 파란색 로브를 입은 후드 쓴 인물, 손에서 파란 마법 에너지가 빛남
- **얼음 조각품**: 만들어지고 있는 멋진 유니콘, 배경의 페가수스, 수정 장미 부케, 작은 기사 조각상
- **얼어붙은 작업장**: 아치형 천장에서 드라마틱한 고드름이 매달린 웅장한 얼음 성당 같은 공간, 서리로 덮인 기둥, 기하학적 얼음 패턴
- **마법의 분위기**: 무지개 빛 굴절, 떠다니는 눈 입자, 전체적으로 영묘한 파란 빛

이미지가 저장된 위치:
`generated_imgs/generated-2025-12-04T04-15-47-907Z-3grhec.png`

![](/img/nano-banana-generated-img.png)

이 이미지를 수정하시겠습니까? `continue_editing` 기능을 사용하여 마법사의 외모를 조정하거나, 더 많은 조각품을 추가하거나, 조명을 변경하거나, 다른 세부 사항을 조정할 수 있습니다!

:::
