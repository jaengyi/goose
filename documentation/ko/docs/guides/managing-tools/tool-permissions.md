---
title: 도구 권한 관리
sidebar_position: 1
sidebar_label: 도구 권한
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Tornado, Settings } from 'lucide-react';

도구 권한은 goose가 확장 기능 내에서 다양한 도구를 사용하는 방식에 대한 세분화된 제어를 제공합니다. 이 가이드는 이러한 권한을 효과적으로 이해하고 구성하는 데 도움이 됩니다.

## 도구와 확장 기능 이해

권한에 대해 자세히 알아보기 전에 주요 구성 요소를 명확히 하겠습니다:

- **확장 기능**은 goose에 기능을 추가하는 패키지입니다 (Developer, Google Drive 등)
- **도구**는 goose가 사용할 수 있는 각 확장 기능 내의 특정 함수입니다

예를 들어, Developer 확장 기능에는 다음과 같은 여러 도구가 포함됩니다:

- 파일 편집을 위한 텍스트 편집기 도구
- 명령 실행을 위한 셸 도구
- 스크린샷 캡처를 위한 화면 캡처 도구
:::warning 성능 최적화
goose는 모든 확장 기능에서 활성화된 총 도구 수가 25개 미만일 때 가장 잘 작동합니다. 현재 작업에 필요한 확장 기능만 활성화하는 것을 고려하세요.
:::

## 권한 수준

도구 권한은 [goose 권한 모드](/docs/guides/goose-permissions)와 함께 작동합니다. 모드는 기본 동작을 설정하고, 도구 권한은 특정 도구의 동작을 재정의할 수 있게 합니다.

각 도구는 세 가지 권한 수준 중 하나로 설정할 수 있습니다:

| 권한 수준 | 설명 | 적합한 용도 | 예시 |
|-----------------|-------------|-----------|----------|
| **항상 허용** | 승인 없이 도구 실행 | 안전한 읽기 전용 작업 | • 파일 읽기<br></br>• 디렉토리 목록<br></br>• 정보 검색 |
| **사전 확인** | 확인 필요 | 상태 변경 작업 | • 파일 쓰기/편집<br></br>• 시스템 명령<br></br>• 리소스 생성 |
| **절대 금지** | 도구 사용 불가 | 민감한 작업 | • 자격 증명 액세스<br></br>• 시스템 중요 파일<br></br>• 리소스 삭제 |

## 도구 권한 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    `Manual` 또는 `Smart` 승인 모드를 사용할 때 활성화된 확장 기능에 대한 세분화된 도구 권한을 구성할 수 있습니다. 이러한 규칙은 모드 토글 또는 `Settings` 페이지에서 액세스할 수 있습니다.

    <Tabs>
      <TabItem value="toggle" label="모드 토글" default>
        1. 앱 하단의 <Tornado className="inline" size={16} /> 버튼을 클릭합니다
        2. 선택한 `Manual` 또는 `Smart` 모드 옆의 <Settings className="inline" size={16} /> 버튼을 클릭합니다
        3. 도구를 구성하려는 확장 기능을 클릭합니다
        4. 각 도구 옆의 드롭다운을 사용하여 권한 수준을 설정합니다
        5. `Save Changes`를 클릭합니다
      </TabItem>
      <TabItem value="settings" label="설정 페이지" default>
        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
        2. 사이드바에서 `Settings` 버튼을 클릭합니다
        3. `Chat`을 클릭합니다
        4. `Mode` 아래에서 선택한 `Manual` 또는 `Smart` 모드 옆의 <Settings className="inline" size={16} /> 버튼을 클릭합니다
        5. 도구를 구성하려는 확장 기능을 클릭합니다
        6. 각 도구 옆의 드롭다운을 사용하여 권한 수준을 설정합니다
        7. `Save Changes`를 클릭합니다
      </TabItem>
    </Tabs>

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    1. configure 명령을 실행합니다:
    ```sh
    goose configure
    ```

    2. 메뉴에서 `goose settings`를 선택합니다
    ```sh
    ┌ goose-configure
    │
    ◆ What would you like to configure?
    | ○ Configure Providers
    | ○ Add Extension
    | ○ Toggle Extensions
    | ○ Remove Extension
    // highlight-start
    | ● goose settings
    // highlight-end
    └
    ```

    3. `Tool Permission`을 선택합니다
    ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  goose settings
    │
    ◆  What setting would you like to configure?
    │  ○ goose mode
    // highlight-start
    │  ● Tool Permission
    // highlight-end
    |  ○ Tool Output
    └
    ```

    4. 확장 기능을 선택하고 해당 도구의 권한을 구성합니다:
    ```sh
    ┌   goose-configure
    │
    ◇  What setting would you like to configure?
    │  Tool Permission
    │
    ◇  Choose an extension to configure tools
    │  developer
    │
    ◇  Choose a tool to update permission
    │  developer__image_processor
    │
    ◆  Set permission level for tool developer__image_processor, current permission level: Not Set
    │  ○ Always Allow
     // highlight-start
    │  ● Ask Before (Prompt before executing this tool)
    // highlight-end
    │  ○ Never Allow
    └
    ```
  </TabItem>
</Tabs>

## 권한 관리의 이점

:::tip
작업이 변경됨에 따라 도구 권한을 검토하고 업데이트하세요. 세션 중에 언제든지 권한을 수정할 수 있습니다.
:::

도구 권한을 구성하는 데는 여러 가지 이유가 있습니다:

1. **성능 최적화**
   - 최상의 성능을 위해 활성화된 총 도구 수를 25개 미만으로 유지
   - 현재 작업에 필요하지 않은 도구 비활성화
   - 컨텍스트 윈도우 사용량 감소 및 응답 품질 향상
   - 도구 결정 마비 방지

2. **보안 제어**
   - 민감한 작업에 대한 액세스 제한
   - 실수로 파일 수정 방지
   - 시스템 리소스 사용 제어

3. **작업 집중**
   - 현재 작업에 필요한 도구만 활성화
   - goose가 더 나은 도구 선택을 할 수 있도록 지원
   - 응답의 노이즈 감소

## 권한 구성 예시

### 작업 기반 구성

현재 작업에 따라 권한을 구성합니다:

```
개발 작업:
✓ 파일 읽기 → 항상 허용
✓ 코드 편집 → 사전 확인
✓ 테스트 실행 → 항상 허용
✗ 시스템 명령 → 사전 확인

문서화 작업:
✓ 파일 읽기 → 항상 허용
✓ 마크다운 편집 → 항상 허용
✗ 코드 편집 → 절대 금지
✗ 시스템 명령 → 절대 금지
```
