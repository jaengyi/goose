---
title: Extension Manager
description: goose 세션 중에 확장 프로그램을 동적으로 검색, 활성화, 비활성화
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PlatformExtensionNote } from '@site/src/components/PlatformExtensionNote';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

항상 수동으로 확장 프로그램을 관리할 필요는 없습니다. Extension Manager 확장 프로그램을 사용하면 goose가 활성 세션 중에 확장 프로그램을 동적으로 검색, 활성화, 비활성화할 수 있습니다. 주어진 작업에 따라 goose는 특정 확장 프로그램이 필요한 시점을 인식하고, 필요할 때 활성화하며, 사용하지 않는 확장 프로그램이 컨텍스트 창을 차지하는 경우 비활성화를 제안합니다.

작업을 설명하기만 하면 goose가 확장 프로그램 관리를 자동으로 처리합니다.

## 구성

<PlatformExtensionNote/>

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Extension Manager"
    description="세션 중에 확장 프로그램을 동적으로 검색, 활성화, 비활성화"
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
  │  ● extensionmanager
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## Extension Manager를 사용하는 이유

goose는 많은 확장 프로그램과 함께 작동할 수 있지만, 한 번에 너무 많은 확장 프로그램이 활성화되면:
- LLM에 너무 많은 도구 선택지를 제공하여 압도할 수 있음
- 도구 선택의 품질이 저하될 수 있음
- 응답 시간이 느려질 수 있음
- 권장 제한(5개의 확장 프로그램 또는 50개의 도구)을 초과할 수 있음

Extension Manager는 goose가 다음을 수행할 수 있게 하여 이를 해결합니다:
- 사용 가능한 확장 프로그램 **검색**
- 특정 작업에 필요할 때만 확장 프로그램 **활성화**
- 더 이상 필요하지 않을 때 확장 프로그램 **비활성화**

이렇게 하면 goose가 필요할 때 정확히 필요한 도구에 액세스할 수 있는 더 집중적이고 효율적인 경험을 만들 수 있습니다.

:::tip 권장 제한
최적의 성능을 위해 **5개 이하의 활성 확장 프로그램**과 총 **50개 이하의 도구**를 목표로 하세요. Extension Manager는 필요할 때만 작업별 확장 프로그램을 활성화하여 이러한 제한 내에 유지하도록 도와줍니다.
:::

## 사용 가능한 도구

Extension Manager는 다음 도구를 제공합니다:

| 도구 | 설명 | 사용 사례 |
|------|-------------|----------|
| `search_available_extensions` | 활성화하거나 비활성화할 수 있는 확장 프로그램 검색 | 작업에 적합한 확장 프로그램 찾기 |
| `manage_extensions` | 이름으로 확장 프로그램 활성화 또는 비활성화 | 동적으로 확장 프로그램 로드/언로드 |
| `list_resources` | 확장 프로그램의 리소스 목록 (지원되는 경우) | 사용 가능한 데이터 소스 검색 |
| `read_resource` | 특정 리소스 콘텐츠 읽기 (지원되는 경우) | 확장 프로그램이 제공하는 데이터 액세스 |

:::tip
리소스 도구(`list_resources` 및 `read_resource`)는 리소스를 지원하는 확장 프로그램이 하나 이상 활성화된 경우에만 사용할 수 있습니다.
:::

## 사용 예제

필요할 때 확장 프로그램을 활성화해 보겠습니다. 이 예제에서는 리포지토리 작업을 위해 GitHub 확장 프로그램을 활성화합니다.

### goose 프롬프트

```
내 GitHub 리포지토리 전부 나열해줘
```

### goose 출력

:::note Desktop

```
리포지토리 작업을 위해 GitHub 확장 프로그램을 활성화해 드리겠습니다.

MANAGE_EXTENSIONS
action: enable
extension_name: github

✅ 확장 프로그램 'github'이 성공적으로 설치되었습니다

GitHub 확장 프로그램이 이제 활성화되었습니다.

GitHub 확장 프로그램을 사용하여 GitHub 리포지토리를 나열하겠습니다.

LIST_REPOSITORIES

리포지토리 목록입니다:

...

이 리포지토리 중 작업하고 싶은 것이 있나요?
```

:::
