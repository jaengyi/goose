---
title: Code Execution 확장 프로그램
description: 여러 MCP 도구와 상호 작용하기 위해 JavaScript 코드 실행
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PlatformExtensionNote } from '@site/src/components/PlatformExtensionNote';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

Code Execution 확장 프로그램은 MCP 도구와 상호 작용하기 위한 프로그래밍 방식인 [Code Mode](/docs/guides/managing-tools/code-mode)를 활성화합니다.

Code Mode에서 LLM은 활성화된 확장 프로그램에서 사용 가능한 도구를 검색하고 도구를 직접 하나씩 호출하는 대신 goose가 한 번에 실행하는 JavaScript 코드를 작성합니다. 이는 여러 확장 프로그램이 활성화되어 있고 여러 도구 호출이 있는 워크플로우를 수행할 때 컨텍스트 창 사용량을 더 효율적으로 관리하는 데 도움이 됩니다.

## 구성

<PlatformExtensionNote defaultEnabled={false} />

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Code Execution"
    description="MCP 도구와 효율적으로 상호 작용하기 위해 JavaScript 코드 실행"
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
  │  ● code_execution
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 여러 도구 호출이 필요한 보고서를 컴파일하도록 goose에게 요청합니다.

### goose 프롬프트

```
현재 git 브랜치, 최근 3개의 커밋, package.json의 버전이 포함된 LOG.md 파일을 만들어줘
```

### goose 출력

:::note Desktop
git 브랜치, 최근 3개의 커밋, package.json의 버전이 포함된 LOG.md 파일을 만드는 것을 도와드리겠습니다. 이 모든 정보를 한 번의 작업으로 수집하겠습니다.

`Execute Code code: import { shell, text_editor } from "developer" ...`

먼저 package.json 경로를 확인하겠습니다:

`Execute Code code: import { shell, text_editor } from "developer" ...`

완료! 다음을 포함하는 **LOG.md** 파일을 생성했습니다:

- ✅ **현재 git 브랜치**
- ✅ **최근 3개의 커밋** (해시, 메시지, 작성자, 상대 시간 포함)
- ✅ ui/desktop/package.json의 **버전**

파일이 루트 디렉토리에 `LOG.md`로 저장되었습니다.
:::
