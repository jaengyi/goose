---
title: JetBrains 확장 프로그램
description: JetBrains MCP 서버를 goose 확장 프로그램으로 사용하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import { PanelLeft } from 'lucide-react';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/1fP5elf9qQM" />

이 튜토리얼에서는 모든 JetBrains IDE와 통합하기 위해 JetBrains 확장 프로그램을 추가하는 방법을 다룹니다. goose는 개발자 중심 작업에 [Developer 확장 프로그램](/docs/mcp/developer-mcp)을 사용할 수 있지만, JetBrains 확장 프로그램은 코드 작업에 더 통합되고 프로젝트 인식적인 방법을 제공합니다.

## 구성

**중요**: 구성 단계는 IDE 버전에 따라 다릅니다. `[IDE 이름] > About` (macOS) 또는 `Help > About` (Windows/Linux)에서 버전을 확인할 수 있습니다.

<Tabs groupId="ideVersion">
  <TabItem value="later" label="2025.2 이상" default>

    2025.2 이상 버전은 내장된 MCP 서버 지원이 있으며 IDE 인스턴스에 특정한 동적 구성을 생성합니다. 자세한 내용은 IDE 문서를 참조하세요 (예: IntelliJ IDEA의 경우 [MCP Server](https://www.jetbrains.com/help/idea/mcp-server.html)).

    <br/>
    IDE의 내장 MCP 서버 지원을 사용하여 확장 프로그램을 구성하세요:

    1. IDE 특정 구성 가져오기:

       1. IDE에서 `Settings > Tools > MCP Server`로 이동
       2. 필요한 경우 `Enable MCP Server`를 클릭하여 MCP 서버 활성화
       3. `Copy Stdio Config` 클릭
       4. `OK`를 클릭하여 변경 사항을 저장하고 서버 시작
       5. 구성에서 `command`, `args`, `env` 값 복사

    2. 구성의 명령을 사용하여 goose에 JetBrains 확장 프로그램 추가:

       :::info
       goose Desktop 또는 goose CLI 구성 단계가 성공하지 못하면 `Config File` 단계를 따르세요.
       :::

       <Tabs groupId="interface">
         <TabItem value="ui" label="goose Desktop" default>
           1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바 열기
           2. 사이드바에서 `Extensions` 클릭
           3. `Add custom extension` 클릭
           4. `Add custom extension` 모달에서 다음을 입력:
              - **Extension Name**: JetBrains
              - **Type**: STDIO
              - **Description**: goose를 모든 JetBrains IDE와 통합
              - **Command**: IDE의 Stdio 구성에서 `command`와 `args`를 단일 명령 문자열로 결합
              - **Environment Variables**: 복사한 Stdio 구성의 `env` 섹션에서 포트 값으로 `IJ_MCP_SERVER_PORT` 추가
           5. `Add Extension`을 클릭하여 확장 프로그램 저장
           6. 채팅으로 이동
         </TabItem>
         <TabItem value="cli" label="goose CLI">
           <CLIExtensionInstructions
             name="jetbrains"
             description="goose를 모든 JetBrains IDE와 통합"
             type="stdio"
             command="YOUR_COMMAND_AND_ARGS_FROM_IDE"
             timeout={300}
             envVars={[{ key: "IJ_MCP_SERVER_PORT", value: "YOUR_PORT_FROM_IDE" }]}
             commandNote={
               <>
                 <code>command</code>와 <code>args</code>를 단일 문자열로 결합하세요. 포트 값은 복사한 Stdio 구성의 <code>env</code> 섹션에서 가져옵니다.
               </>
             }
           />
         </TabItem>
         <TabItem value="config" label="Config File">
           1. goose [`config.yaml`](/docs/guides/config-files) 파일 열기
           2. `extensions` 섹션에 IDE의 Stdio 구성을 사용하는 항목 추가, 예:

              ```yaml
              extensions:
                jetbrains:
                  enabled: true
                  type: stdio
                  name: JetBrains
                  description: goose를 모든 JetBrains IDE와 통합
                  cmd: /Applications/IntelliJ IDEA.app/Contents/jbr/Contents/Home/bin/java
                  args:
                  - -classpath
                  - /Applications/IntelliJ IDEA.app/Contents/plugins/mcpserver/lib/mcpserver-frontend.jar:/Applications/IntelliJ IDEA.app/Contents/lib/util-8.jar
                  - com.intellij.mcpserver.stdio.McpStdioRunnerKt
                  envs:
                    IJ_MCP_SERVER_PORT: "63342"
                  env_keys:
                  - IJ_MCP_SERVER_PORT
                  timeout: 300
                  bundled: null
                  available_tools: []
              ```

              다음을 확인하세요:
              - 예제의 `cmd` 및 `args` 값을 JetBrains IDE 설치에 맞게 교체
              - IDE의 MCP 서버 포트에 맞게 `IJ_MCP_SERVER_PORT` 업데이트
         </TabItem>
       </Tabs>
  </TabItem>
  <TabItem value="earlier" label="2025.1 이하">

    2025.1 이하 버전은 MCP Server 플러그인을 설치하고 [JetBrains MCP Proxy Server](https://github.com/JetBrains/mcp-jetbrains)를 사용해야 합니다.

    :::tip 요약
    <Tabs groupId="interface">
      <TabItem value="ui" label="goose Desktop" default>
      [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40jetbrains%2Fmcp-proxy&id=jetbrains&name=JetBrains&description=Integrate%20goose%20with%20any%20JetBrains%20IDE)
      </TabItem>
      <TabItem value="cli" label="goose CLI">
      **Command**
      ```sh
      npx -y @jetbrains/mcp-proxy
      ```
      </TabItem>
    </Tabs>

    **필수 설정**

    IDE에 [MCP Server 플러그인](https://plugins.jetbrains.com/plugin/26071-mcp-server)을 추가하세요.
    :::

    :::info
    이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
    :::

    <br/>
    MCP Server 플러그인과 프록시 서버를 사용하여 확장 프로그램을 구성하세요:

    1. IDE에 [MCP Server 플러그인](https://plugins.jetbrains.com/plugin/26071-mcp-server) 추가

    2. goose에 JetBrains 확장 프로그램 추가:

       <Tabs groupId="interface">
         <TabItem value="ui" label="goose Desktop" default>
           <GooseDesktopInstaller
             extensionId="jetbrains"
             extensionName="JetBrains"
             description="goose를 모든 JetBrains IDE와 통합"
             command="npx"
             args={["-y", "@jetbrains/mcp-proxy"]}
             timeout={300}
           />
         </TabItem>
         <TabItem value="cli" label="goose CLI">
             <CLIExtensionInstructions
               name="jetbrains"
               description="goose를 모든 JetBrains IDE와 통합"
               command="npx -y @jetbrains/mcp-proxy"
               timeout={300}
             />
         </TabItem>
       </Tabs>

  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 Java 프로젝트를 최신 LTS 버전으로 업그레이드하겠습니다.

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::


<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   1. [IntelliJ](https://www.jetbrains.com/idea/download) (JetBrains의 Java 및 Kotlin IDE) 열기
   2. goose Desktop에서 새 세션 열기
   :::note
   goose Desktop 앱과 IntelliJ IDE 두 개의 별도 앱과 상호 작용하게 됩니다.
   :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. [IntelliJ](https://www.jetbrains.com/idea/download) (JetBrains의 Java 및 Kotlin IDE) 열기
  2. IDE 내에서 터미널을 열고 새 goose 세션 시작:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

:::tip
goose를 사용하여 코드를 리팩토링할 때는 변경하기 _전에_ 코드를 버전 관리에 커밋하는 것이 좋습니다. 이렇게 하면 비교할 깔끔한 스냅샷이 있어 goose의 수정 사항을 검토하고, 필요한 경우 되돌리고, 개선 사항을 추적하기 쉬워집니다.
:::

### goose 프롬프트
```
using the JetBrains IDE tools, upgrade this project to use the latest Java LTS version and refactor the codebase to use newer Java features where beneficial.
```

2분 후, goose가 업그레이드를 완료했습니다! 🎉

### goose 출력

```md
I'll make several improvements to modernize this codebase to use Java 21 (latest LTS) features. Here are the changes I'll implement:

1. Update the `pom.xml` to use Java 21
2. Use newer Java features

[... code changes ...]

Here's a summary of the modernization changes made:

1. `pom.xml`:
   - Set Java version to 21
   - Enabled preview features
   - Updated all dependencies to their latest stable versions

2. `AccountsOverviewPage.java`:
   - Made the class `final` since it extends the sealed `Page` class
   - Updated stream collection to use `.toList()` instead of `collect(Collectors.toList())`
   - Made fields final for better immutability

3. `Page.java`:
   - Converted to a sealed class
   - Restricted which classes can extend it using `permits`
   - Made fields protected and final where appropriate
   - Added pattern matching for exception handling
   - Implemented Optional for null safety
   - Used String Templates for error messages
   - Added functional programming concepts with Supplier
   - Enhanced error handling with specific exception types

4. `LoginComponent.java`:
   - Added a private record for credentials
   - Implemented pattern matching with record patterns
   - Made fields final for better immutability

5. `AccountTests.java`:
   - Implemented structured concurrency using `StructuredTaskScope`
   - Used parallel execution for UI and API tasks
   - Improved error handling with structured task management
   - Used pattern matching and type inference with `var`

6. `BaseTests.java`:
   - Implemented structured concurrency for setup tasks
   - Improved resource management
   - Better exception handling
   - Used modern Duration API

7. `APIUtil.java`:
   - Implemented String Templates (Java 21 preview feature)
   - Used modern text blocks

8. `Customers.java`:
   - Added sealed interface for customer types
   - Implemented pattern matching for switch expressions
   - Added pattern matching for instanceof
   - Enhanced type safety with records

Key modern Java features used:
- String Templates (Java 21 Preview)
- Pattern Matching for switch (Java 21)
- Sealed Classes and Interfaces (Java 17)
- Records (Java 16)
- Pattern Matching for instanceof (Java 16)
- Text Blocks (Java 15)
- Switch Expressions (Java 14)

The codebase is now using the latest Java LTS version (21) and incorporates many modern Java features that improve:
- Type safety
- Null safety
- Code readability
- Error handling
- Performance
```




JetBrains 확장 프로그램을 사용하면 goose가 다음을 더 쉽게 수행할 수 있었습니다:
- 코드베이스 탐색
- 변경 사항 추적
- 파일 간에 일관된 수정 사항 적용
- 변경 전후의 파일 내용 확인
