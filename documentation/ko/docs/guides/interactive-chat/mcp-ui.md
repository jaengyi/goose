---
sidebar_position: 1
title: MCP 앱 및 MCP-UI 사용
sidebar_label: MCP 앱 및 MCP-UI 사용
description: goose가 MCP 앱 및 MCP-UI 확장 기능에서 인터랙티브 UI 구성 요소를 렌더링하는 방법 알아보기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import { PanelLeft } from 'lucide-react';

# MCP 앱 및 MCP-UI 사용

MCP 앱 또는 MCP-UI로 구축된 확장 기능을 사용하면 goose 데스크톱이 인터랙티브하고 매력적인 사용자 경험을 제공할 수 있습니다. 텍스트 응답을 읽고 모든 프롬프트를 입력하는 대신 그래픽적이고 클릭 가능한 UI를 사용한다고 상상해 보세요. 이러한 확장 기능은 goose가 풍부하고 동적이며 간소화된 상호 작용을 위해 삽입된 UI 요소로 렌더링할 수 있는 콘텐츠를 반환합니다.

:::info MCP 앱이 공식 사양입니다
[MCP 앱](/docs/tutorials/building-mcp-apps)은 이제 인터랙티브 UI를 위한 공식 MCP 사양입니다. MCP-UI 확장 기능은 여전히 goose에서 작동하지만, 새로운 확장 기능에는 MCP 앱이 권장됩니다.
:::

<div style={{ width: '100%', maxWidth: '800px', margin: '0 auto' }}>
  <video
    controls
    playsInline
    style={{
      width: '100%',
      aspectRatio: '2876/2160',
      borderRadius: '8px'
    }}
  >
    <source src={require('@site/static/videos/plan-trip-demo.mp4').default} type="video/mp4" />
    브라우저가 video 태그를 지원하지 않습니다.
  </video>
</div>

## 사용해 보기

goose에서 인터랙티브 응답이 어떻게 작동하는지 확인해 보세요. 이 연습에서는 Andrew Harvard가 제공하는 [MCP-UI 데모](https://mcp-aharvard.netlify.app/)에 연결하는 확장 기능을 추가합니다.

  <Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
      <GooseDesktopInstaller
        extensionId="richdemo"
        extensionName="Rich Demo"
        description="데모 인터랙티브 확장 기능"
        type="http"
        url="https://mcp-aharvard.netlify.app/mcp"
      />
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        <CLIExtensionInstructions
          name="rich_demo"
          description="데모 인터랙티브 확장 기능"
          type="http"
          url="https://mcp-aharvard.netlify.app/mcp"
          timeout={300}
        />
    </TabItem>
  </Tabs>

goose 데스크톱에서 물어보세요:

- `항공편 좌석 선택을 도와주세요`

단순한 텍스트 대신 다음이 포함된 인터랙티브 응답이 표시됩니다:
- 이용 가능한 좌석과 점유된 좌석이 있는 시각적 좌석 지도
- 실시간 클릭 가능한 선택 기능
- 항공편 세부 정보가 포함된 예약 확인

다른 데모를 시도해 보세요:

- `내 기분에 따라 다음 여행을 계획해 주세요`
- `필라델피아 날씨는 어때?`

## 확장 기능 개발자를 위해

자신만의 확장 기능에 인터랙티비티를 추가하세요:

- [MCP 앱 구축](/docs/tutorials/building-mcp-apps) - 단계별 튜토리얼 (권장)
- [MCP-UI 문서](https://mcpui.dev/guide/introduction) - MCP-UI 사양
