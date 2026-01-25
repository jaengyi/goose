---
title: Figma 확장 프로그램
description: Figma Dev Mode MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import { PanelLeft } from 'lucide-react';

이 튜토리얼에서는 Figma 파일, 디자인, 컴포넌트와 상호 작용할 수 있도록 [Figma Dev Mode MCP 서버](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-Mode-MCP-Server)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::info
MCP 서버는 Professional, Organization 또는 Enterprise Figma 플랜에서 Dev 또는 Full 시트가 필요합니다.
:::

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    [설치 프로그램 실행](goose://extension?type=streamable_http&url=http%3A%2F%2F127.0.0.1%3A3845%2Fmcp&id=figma&name=Figma&description=Convert%20Figma%20designs%20into%20code%20and%20extract%20design%20context)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    `goose configure`를 사용하여 `Remote Extension (Streaming HTTP)` 확장 프로그램 유형을 추가합니다:

    **Endpoint URL**

    ```
    http://127.0.0.1:3845/mcp
    ```
  </TabItem>
</Tabs>

**필수 설정**

Dev Mode MCP 서버는 [Figma 데스크톱 앱](https://www.figma.com/downloads/)에서 활성화되어야 합니다
:::

## 구성

1. Figma의 [Dev Mode MCP 서버 가이드](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-Mode-MCP-Server)의 지침을 따라 MCP 서버를 활성화하세요.

   이제 서버가 `http://127.0.0.1:3845/mcp`에서 실행됩니다

   :::info 대체 설정
   지침에 설명된 대로 데스크톱 앱에서 `Preferences` 메뉴가 보이지 않으면 다음을 시도하세요:
   1. 디자인 파일 하단의 `</>` 토글을 클릭
   2. 오른쪽 패널에서 `Enable MCP server` 토글 켜기
   :::

2. goose에 Figma 확장 프로그램 추가:
   <Tabs groupId="interface">
     <TabItem value="ui" label="goose Desktop" default>
       <GooseDesktopInstaller
         extensionId="figma"
         extensionName="Figma"
         description="Figma 디자인을 코드로 변환하고 디자인 컨텍스트 추출"
         type="http"
         url="http://127.0.0.1:3845/mcp"
       />
     </TabItem>
     <TabItem value="cli" label="goose CLI">
       <CLIExtensionInstructions
         name="figma"
         description="Figma 디자인을 코드로 변환하고 디자인 컨텍스트 추출"
         type="http"
         url="http://127.0.0.1:3845/mcp"
         timeout={300}
       />
     </TabItem>
   </Tabs>

## 사용 예제

goose의 Figma 확장 프로그램은 Figma 데스크톱 앱과 함께 작동합니다. 확장 프로그램을 사용하여 다음을 수행할 수 있습니다:
- Figma 디자인을 프로덕션 준비 코드(React, Vue, HTML/CSS 등)로 변환
- 디자인 토큰, 변수 추출 및 디자인 시스템 규칙 생성
- 디자인의 시각적 참조 캡처

Figma 디자인을 React 코드로 변환해 보겠습니다. 먼저 링크를 복사하세요:
  1. MCP 서버가 활성화된 [Figma 데스크톱 앱](https://www.figma.com/downloads/)에서 디자인 파일을 엽니다
  2. 레이어 패널에서 프레임이나 컴포넌트를 마우스 오른쪽 버튼으로 클릭하고 `Copy link to selection`을 선택합니다

:::tip
goose에 Figma 링크를 제공하거나 Figma 데스크톱 앱에서 요소를 선택하기만 하면 됩니다.
:::

### goose 프롬프트

```
Convert this to React: https://www.figma.com/design/B6IFM1qjwxJw5fOYJYKRl5/Buttons-?node-id=351-8&m=dev
```

### goose 출력

:::note Desktop

I'll help you convert that Figma design to React. Let me first examine the Figma file to understand the design structure and components.

```
manage_extensions action, extension_name
```

I'll help you convert that Figma design to React. Let me first extract the node ID from the URL and then generate the React code for you.

```
get_code nodeId, clientLanguages, clientFrameworks
```

```
get_image nodeId, clientLanguages, clientFrameworks
```

I've successfully converted your Figma design to React! Here's the generated component:

```jsx
const imgImage1 = "http://localhost:3845/assets/e0e0f6accb741cb6cacec2697cef047a7eea7e30.png";

function Component1() {
  return (
    <div className="relative size-full" data-name="Component 1" data-node-id="351:8">
      <div className="absolute bg-[#d9d9d9] inset-0 rounded-lg" data-node-id="351:2" />
      <div
        className="absolute font-['Inter:Black',_sans-serif] font-black inset-[23.33%_2.56%_22.22%_56.41%] leading-[normal] not-italic text-[#333b43] text-[20px]"
        data-node-id="351:3"
      >
        <p className="block mb-0">ask</p>
        <p className="block">goose</p>
      </div>
      <div
        className="absolute aspect-[92/90] bg-center bg-cover bg-no-repeat left-0 right-[52.82%] rounded-lg top-0"
        data-name="image 1"
        data-node-id="351:6"
        style={{ backgroundImage: `url('${imgImage1}')` }}
      />
    </div>
  );
}

export default function Component2() {
  return (
    <div className="relative size-full" data-name="Component 1" data-node-id="351:8">
      <Component1 />
    </div>
  );
}
```

#### Key Features of the Generated Component:

- **Tailwind CSS Styling**: Uses Tailwind classes for responsive design and styling
- **Image Asset**: The goose logo is loaded from a localhost server as a background image
- **Typography**: Uses Inter Black font for the "ask goose" text
- **Layout**: Positioned absolutely with precise inset values to match the Figma design
- **Responsive**: Uses relative positioning and percentage-based insets for scalability

#### Component Structure:
- **Background**: Light gray rounded container (#d9d9d9)
- **Logo**: goose silhouette image positioned on the left side
- **Text**: "ask goose" text positioned on the right in dark gray (#333b43)

The component maintains the exact visual hierarchy and styling from your Figma design while being fully responsive and ready to integrate into a React application with Tailwind CSS.

:::
