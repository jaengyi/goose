---
title: Dev.to 확장 기능
description: Dev.to MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import { PanelLeft } from 'lucide-react';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/nkdksdxHxaQ" />

이 튜토리얼은 [Dev.to MCP 서버](https://github.com/nickytonline/dev-to-mcp)를 goose 확장 기능으로 추가하여 Dev.to 공개 API에 액세스하는 방법을 다룹니다. 이 확장 기능을 사용하면 goose가 인증 없이 기사, 태그, 사용자 정보, 댓글 등을 가져올 수 있습니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?type=streamable_http&url=http%3A%2F%2Flocalhost%3A3000%2Fmcp&id=dev-to&name=Dev.to&description=Access%20Dev.to%20articles%20and%20content)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  `goose configure`를 사용하여 `Remote Extension (Streaming HTTP)` 확장 기능 유형을 추가하세요:

  **엔드포인트 URL**

  ```
  http://localhost:3000/mcp
  ```

</TabItem>
</Tabs>

**필수 설정**

확장 기능을 추가하기 전에 Dev.to MCP 서버가 실행 중인지 확인하세요
:::

## 구성

:::info
Node.js와 npm이 설치되어 있는지 확인하세요. 서버를 사용하기 전에 `npm install`과 `npm run build`를 한 번 실행해야 합니다.
:::

1. 먼저 `Dev.to MCP 서버`를 클론하고 설정합니다:
```bash
git clone https://github.com/nickytonline/dev-to-mcp.git
cd dev-to-mcp
npm install
npm run build # 일회성 설정
```
```bash
npm start
```
서버가 다음 주소에서 실행됩니다:
`http://localhost:3000/mcp`


<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    <GooseDesktopInstaller
      extensionId="dev-to"
      extensionName="Dev.to"
      description="Dev.to 기사 및 콘텐츠에 액세스"
      type="http"
      url="http://localhost:3000/mcp"
    />

  </TabItem>
  <TabItem value="cli" label="goose CLI" default>
      <CLIExtensionInstructions
        name="dev.to"
        description="Dev.to 기사 및 콘텐츠에 액세스"
        type="http"
        url="http://localhost:3000/mcp"
        timeout={300}
      />
  </TabItem>
</Tabs>

## 사용 예시

이 예시에서 goose는 `Dev.to MCP` 확장 기능을 사용하여 최신 JavaScript 기사 5개를 가져와 즉시 개인화된 학습 허브로 만듭니다.

### goose 프롬프트
```
goose야, Dev.to에서 최신 JavaScript 기사 5개를 가져와줘
다음을 포함하는 간단한 HTML 랜딩 페이지를 만들어줘:

1. 각 기사 제목을 클릭 가능한 링크로 나열

2. 각 제목 아래에 저자와 한 문장 요약 포함

3. 각 기사에서 배운 내용을 적용할 수 있는 작은 프로젝트나 코드 스니펫 제안 (해당되는 경우)

4. 시간이 하나밖에 없다면 어떤 기사를 먼저 읽어야 하는지 강조하고 이유 설명

목표는 무엇을 읽을 가치가 있는지 빠르게 보여주고 실행 가능한 연습 아이디어를 제공하여 시간을 절약하고 더 빨리 배울 수 있도록 하는 것입니다.
```

### goose 출력

:::note 데스크톱

Dev.to에서 최신 JavaScript 기사를 가져와 멋진 HTML 학습 페이지를 만들어 드리겠습니다. 먼저 기사를 가져오겠습니다.

`─── per_page state tag ──────────────────────────`

이제 요청하신 모든 기능이 포함된 아름다운 HTML 학습 페이지를 만들겠습니다:

`─── writing /Users/ebonyl/... ──────────────────────────`

완벽합니다! Dev.to에서 최신 JavaScript 기사 5개를 담은 아름다운 HTML 랜딩 페이지를 만들었습니다. 포함된 내용은 다음과 같습니다:

🎯 주요 기능
    - 📱 현대적인 그라데이션 배경과 카드 기반 레이아웃을 갖춘 반응형 디자인
    - 🔗 새 탭에서 열리는 클릭 가능한 기사 제목
    - 👤 각 기사의 저자 이름과 읽기 시간
    - 📝 각 기사가 다루는 내용을 설명하는 한 문장 요약
    - 🏷️ 관련 기술을 보여주는 태그 표시
    - ⭐ 시각적 강조가 있는 우선순위 추천
    - 학습을 심화하기 위한 실행 가능한 프로젝트
🎨 디자인 요소
    - 그라데이션 배경과 부드러운 호버 효과
    - 시스템 글꼴을 사용한 깔끔한 타이포그래피
    - 색상으로 구분된 추천 시스템
    - 모든 기기에서 작동하는 반응형 레이아웃
    - 그림자와 애니메이션이 있는 전문적인 카드 기반 디자인
📖 콘텐츠 하이라이트

최우선 추천: Oluwabusayo Jacobs의 Signal Clone 튜토리얼(파트 1)이 필독 기사로 강조됩니다:

29분짜리 포괄적인 심층 분석
실제 메시징 앱 개발을 다룸
인증과 실시간 기능 포함
React Native와 Stream 사용 (웹 개발에 높은 전이 가능성)
실용적이고 실습 중심의 학습 경험 제공

랜딩 페이지는 `javascript_articles_landing.html`로 저장되어 브라우저에서 열 준비가 되었습니다! 각 기사 링크를 클릭하면 전체 Dev.to 기사로 바로 이동하여 더 깊이 읽을 수 있습니다.

![Dev.to 프로젝트 예시](/img/dev.to-project.png)
:::
