---
title: Memory 확장 프로그램
description: Memory MCP 서버를 goose 확장 프로그램으로 사용하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

<YouTubeShortEmbed videoUrl="https://youtube.com/embed/BZ0yrSLXQwk" />

Memory 확장 프로그램은 개인화된 핵심 정보(예: 명령, 코드 스니펫, 환경 설정, 구성)를 가르쳐서 나중에 회상하고 적용할 수 있게 하여 goose를 지식이 풍부한 어시스턴트로 만들어 줍니다. 프로젝트별(로컬) 지식이든 범용(글로벌) 지식이든 goose는 당신에게 가장 중요한 것을 배우고 기억합니다.

이 튜토리얼에서는 goose에 내장된 확장 프로그램인 Memory MCP 서버를 활성화하고 사용하는 방법을 다룹니다.

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Memory"
    description="일관된 지원을 위해 개인화된 정보를 저장하고 회상"
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
  │  ● memory
  // highlight-end
  |
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## Memory를 사용하는 이유
Memory 확장 프로그램을 사용하면 정적인 메모를 저장하는 것이 아니라 goose가 당신을 더 잘 도울 수 있도록 가르치는 것입니다. goose에게 다음과 같이 말한다고 상상해 보세요:

> _MCP 서버에 대한 모든 것을 배우고 메모리에 저장해._

나중에 다음과 같이 요청할 수 있습니다:
> _우리의 MCP 서버 지식을 활용해서 MCP 서버를 만드는 것을 도와줘._

goose에게 기억하라고 지시하는 한 저장한 모든 것을 회상합니다. 이를 통해 goose와 작업할 때 일관된 결과를 더 쉽게 얻을 수 있습니다.

goose는 세션 시작 시 저장된 모든 메모리를 로드하고 LLM에 보내는 모든 프롬프트에 포함합니다. 크거나 상세한 지침의 경우 파일에 저장하고 goose에게 해당 파일을 참조하도록 지시하세요:

> _JavaScript 작성 도움을 요청하면 "/path/to/javascript_notes.txt"를 참조하고 해당 파일의 지침을 따르라고 기억해._


## 트리거 단어와 사용 시기
goose는 메모리를 저장, 검색 또는 제거할 때를 알리는 특정 트리거 단어도 인식합니다.

| **트리거 단어**   | **사용 시기** |
|---------------------|----------------|
| remember            | 나중에 사용할 유용한 정보 저장 |
| forget           | 저장된 메모리 제거 |
| memory           | 일반적인 메모리 관련 작업 |
| save             | 명령, 구성 또는 환경 설정 저장 |
| remove memory    | 특정 저장된 데이터 삭제 |
| clear memory     | 모든 저장된 메모리 지우기 |
| search memory    | 이전에 저장된 데이터 찾기 |
| find memory      | 특정 저장된 정보 찾기 |

## 사용 예제

이 예제에서는 프로젝트의 API 표준에 대해 가르쳐서 goose를 지식이 풍부한 개발 어시스턴트로 만드는 방법을 보여드리겠습니다. Memory 확장 프로그램을 사용하면 goose가 구조화된 정보를 저장하고 필요할 때 회상하여 작업의 일관성을 유지하는 데 도움이 됩니다.

즉, 더 이상 반복할 필요가 없습니다. goose가 프로젝트의 요구 사항을 기억하고 새 작업에 자동으로 적용합니다.

:::tip
API 표준이나 기타 구조화된 지식으로 자주 작업하는 경우 goose가 워크플로우에서 패턴을 감지할 때 자동으로 메모리에 저장하도록 제안할 수 있습니다.
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

### 1단계: goose에게 API 표준 가르치기
    #### goose 프롬프트 #1
    ```
    goose, learn our API development standards:

    - All endpoints must use TypeScript interfaces
    - Error responses follow RFC 7807 Problem Details spec
    - Rate limiting headers are required
    - Authentication uses JWT in Authorization header
    - All endpoints need OpenAPI documentation

    Remember this so you can help me follow these practices.
    ```

    #### goose 출력

    ```
    I've learned your API standards. I'll help enforce these requirements when working on API endpoints.

    Storing in:
        - Category: development_standards
        - Tags: #api #typescript #security #documentation
        - Scope: global (these are general standards)

    I can now help with:
        - API endpoint implementation
        - Error handling patterns
        - Authentication flows
        - Documentation requirements
    ```
### 2단계: 저장된 지식을 사용하여 새 API 엔드포인트 생성
    #### goose 프롬프트 #2

    ```
    Help me create a new endpoint for user preferences
    ```
    #### goose 출력
    ```
    I see you need a new endpoint. Based on our API development standards that I have stored in memory, I'll ensure it meets all our requirements.

    [.. Checking stored API standards ...]

    I've included all our required elements:

    ✅ TypeScript interfaces
    ✅ RFC 7807 error response format
    ✅ Rate limiting
    ✅ JWT auth header
    ✅ OpenAPI documentation

    Here's your compliant endpoint:

    interface UserPreferences {
        userId: string;
        theme: 'light' | 'dark';
        notifications: boolean;
        timezone: string;
    }

    interface ErrorResponse {
        type: string;
        title: string;
        status: number;
        detail: string;
        instance: string;
    }

    @Route('/api/users/:userId/preferences')
    class UserPreferencesController {
        @Get()
        @RateLimit(100)  // Adding required rate limiting
        @OpenAPI({ ... }) // Including mandatory OpenAPI docs
        async getUserPreferences(
            @Header('Authorization') token: string,  // JWT auth as per our standard
            @Param('userId') userId: string
        ): Promise<UserPreferences | ErrorResponse> {
            // Implementation following our error handling pattern
        }
    }

    Would you like me to implement the full endpoint logic?
    ```
