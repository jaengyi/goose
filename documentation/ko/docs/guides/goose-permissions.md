---
sidebar_position: 20
title: goose 권한 모드
sidebar_label: goose 권한
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Tornado } from 'lucide-react';

goose의 권한은 파일 수정, 확장 기능 사용 및 자동화된 작업 수행 시 얼마나 많은 자율성을 가지는지 결정합니다. 권한 모드를 선택하면 goose가 개발 환경과 상호작용하는 방식을 완전히 제어할 수 있습니다.

<details>
  <summary>권한 모드 비디오 안내</summary>
  <iframe
  class="aspect-ratio"
  src="https://www.youtube.com/embed/bMVFFnPS_Uk"
  title="goose Permission Modes Explained"
  frameBorder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
  ></iframe>
</details>

## 권한 모드

| 모드 | 설명 | 적합한 사용자 |
|------|-------------|----------|
| **완전 자율** | goose가 **승인 없이** 파일 수정, 확장 기능 사용, 파일 삭제 가능 | **완전한 자동화**와 워크플로우에 원활한 통합을 원하는 사용자 |
| **수동 승인** | goose가 도구나 확장 기능을 사용하기 전에 **확인 요청**(세분화된 [도구 권한](/docs/guides/managing-tools/tool-permissions) 지원) | 모든 변경 사항과 도구 사용을 **검토하고 승인**하려는 사용자 |
| **스마트 승인** | goose가 위험 기반 접근 방식을 사용하여 **저위험 작업은 자동 승인**하고 **다른 작업은 승인 요청**(세분화된 [도구 권한](/docs/guides/managing-tools/tool-permissions) 지원) | 작업의 영향에 따라 **자율성과 감독의 균형 잡힌 조합**을 원하는 사용자 |
| **채팅 전용** | goose가 **채팅에만 참여**하며 확장 기능 사용이나 파일 수정 없음 | 자동화 없이 분석, 작문 및 추론 작업을 위한 **대화형 AI 경험**을 선호하는 사용자 |

:::warning
`자율 모드`가 기본적으로 적용됩니다.
:::

## goose 모드 구성

구성 방법은 다음과 같습니다:

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

    세션 전이나 도중에 모드를 변경할 수 있으며 즉시 적용됩니다.

     <Tabs groupId="method">
      <TabItem value="session" label="세션 중" default>

      하단 메뉴에서 <Tornado className="inline" size={16} /> 모드 버튼을 클릭하세요.
      </TabItem>
      <TabItem value="settings" label="설정에서">
        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
        2. 사이드바에서 `Settings` 버튼을 클릭합니다.
        3. `Chat`을 클릭합니다.
        4. `Mode`에서 원하는 모드를 선택합니다.
      </TabItem>
    </Tabs>
  </TabItem>
  <TabItem value="cli" label="goose CLI">

    <Tabs groupId="method">
      <TabItem value="session" label="세션 중" default>
        세션 도중 모드를 변경하려면 `/mode` 명령을 사용하세요.

        * 자율: `/mode auto`
        * 스마트 승인: `/mode smart_approve`
        * 승인: `/mode approve`
        * 채팅: `/mode chat`
      </TabItem>
      <TabItem value="settings" label="설정에서">
        1. 다음 명령을 실행하세요:

        ```sh
        goose configure
        ```

        2. 메뉴에서 `goose settings`를 선택하고 Enter를 누르세요.

        ```sh
        ┌ goose-configure
        │
        ◆ What would you like to configure?
        | ○ Configure Providers
        | ○ Add Extension
        | ○ Toggle Extensions
        | ○ Remove Extension
        // highlight-start
        | ● goose settings (Set the goose mode, Tool Output, Tool Permissions, Experiment, goose recipe github repo and more)
        // highlight-end
        └
        ```

        3. 메뉴에서 `goose mode`를 선택하고 Enter를 누르세요.

        ```sh
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  goose settings
        │
        ◆  What setting would you like to configure?
        // highlight-start
        │  ● goose mode (Configure goose mode)
        // highlight-end
        │  ○ Router Tool Selection Strategy
        │  ○ Tool Permission
        │  ○ Tool Output
        │  ○ Max Turns
        │  ○ Toggle Experiment
        │  ○ goose recipe github repo
        │  ○ Scheduler Type
        └
        ```

        4. 구성하려는 goose 모드를 선택하세요.

        ```sh
        ┌   goose-configure
        │
        ◇  What would you like to configure?
        │  goose settings
        │
        ◇  What setting would you like to configure?
        │  goose mode
        │
        ◆  Which goose mode would you like to configure?
        // highlight-start
        │  ● Auto Mode (Full file modification, extension usage, edit, create and delete files freely)
        // highlight-end
        |  ○ Approve Mode
        |  ○ Smart Approve Mode
        |  ○ Chat Mode
        |
        └  Set to Auto Mode - full file modification enabled
        ```
      </TabItem>
    </Tabs>
  </TabItem>
</Tabs>

  :::info
  수동 및 스마트 승인 모드에서는 도구 호출 중에 세션 창에 "Allow" 및 "Deny" 버튼이 표시됩니다.
  goose는 '쓰기' 도구로 간주되는 도구에 대해서만 권한을 요청합니다. 예: 모든 '텍스트 편집기 쓰기', '텍스트 편집기 편집', 'bash - rm, cp, mv' 명령.

  읽기/쓰기 승인은 읽기 또는 쓰기 도구를 분류하기 위해 최선의 시도를 합니다. 이는 LLM 공급자에 의해 해석됩니다.
  :::
