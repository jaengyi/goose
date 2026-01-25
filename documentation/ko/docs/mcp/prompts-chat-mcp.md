---
title: prompts.chat 확장 기능
description: prompts.chat MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

이 튜토리얼은 [prompts.chat MCP 서버](https://prompts.chat)를 goose 확장 기능으로 추가하여 AI 어시스턴트에서 직접 수천 개의 AI 프롬프트에 액세스할 수 있게 하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=@fkadev/prompts.chat-mcp@latest&id=prompts-chat-mcp&name=prompts.chat&description=Access%20thousands%20of%20AI%20prompts%20directly%20in%20your%20AI%20assistant&env=PROMPTS_API_KEY%3DAPI%20Key%20to%20save%20and%20list%20private%20prompts)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx -y @fkadev/prompts.chat-mcp@latest
  ```
  </TabItem>
</Tabs>
  **환경 변수 (선택 사항)**
  ```
  PROMPTS_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용).
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseDesktopInstaller
    extensionId="prompts-chat-mcp"
    extensionName="prompts.chat"
    description="AI 어시스턴트에서 직접 수천 개의 AI 프롬프트에 액세스"
    command="npx"
    args={["-y", "@fkadev/prompts.chat-mcp@latest"]}
    envVars={[
      { name: "PROMPTS_API_KEY", label: "prompts.chat에서 비공개 프롬프트를 저장하고 나열하기 위한 API 키 (선택 사항)" }
    ]}
    apiKeyLink="https://prompts.chat"
    apiKeyLinkText="prompts.chat API 키"
  />
</TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="prompts.chat"
      description="AI 어시스턴트에서 직접 수천 개의 AI 프롬프트에 액세스"
      command="npx -y @fkadev/prompts.chat-mcp@latest"
      envVars={[
        { key: "PROMPTS_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          PROMPTS_API_KEY는 선택 사항입니다. 비공개 프롬프트를 저장하고 나열하려면{" "}
          <a href="https://prompts.chat" target="_blank" rel="noopener noreferrer">
            prompts.chat
          </a>에서 API 키를 얻어 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예시

prompts.chat 확장 기능은 goose 내에서 직접 검색하고 사용할 수 있는 AI 프롬프트의 선별된 라이브러리에 대한 액세스를 제공합니다. 영감이 필요하거나 특정 작업에 대해 검증된 프롬프트 패턴을 활용하려는 경우에 유용합니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  1. goose 데스크톱에서 새 세션을 엽니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. 터미널을 열고 새 goose 세션을 시작합니다:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

### 프롬프트 검색

#### goose 프롬프트

```
코드 리뷰에 관한 프롬프트를 검색해줘
```

#### goose 출력

```
코드 리뷰와 관련된 여러 프롬프트를 찾았습니다:

1. **코드 리뷰 어시스턴트** - 코드 품질, 보안 및 모범 사례를 검토하기 위한 포괄적인 프롬프트
2. **풀 리퀘스트 리뷰어** - 풀 리퀘스트를 분석하고 개선 사항을 제안하는 데 도움
3. **보안 코드 감사** - 보안 취약점 식별에 초점

이러한 프롬프트 중 하나를 가져오시겠습니까?
```

### 프롬프트 사용

#### goose 프롬프트

```
코드 리뷰 어시스턴트 프롬프트를 가져와서 현재 파일을 검토하는 데 사용해줘
```

#### goose 출력

```
코드 리뷰 어시스턴트 프롬프트를 가져왔습니다. 코드를 분석하는 데 적용하겠습니다...

[프롬프트 지침을 적용하여 코드 검토]

프롬프트의 프레임워크를 기반으로 한 검토입니다:

✅ 코드 구조 및 조직
✅ 명명 규칙
⚠️ 엣지 케이스에 대한 오류 처리 추가 고려
⚠️ 공개 메서드에 대한 문서화 개선 필요

이러한 발견 사항 중 하나를 해결하는 데 도움을 드릴까요?
```
