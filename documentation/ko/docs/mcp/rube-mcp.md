---
title: Rube 확장 기능
description: 500개 이상의 앱과 연결하기 위해 Rube를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import { PanelLeft } from 'lucide-react';

이 튜토리얼은 [Rube](https://rube.app)를 goose 확장 기능으로 추가하여 Slack, Gmail, Notion, Google Workspace, Microsoft Office, GitHub 등 500개 이상의 앱과의 원활한 통합을 가능하게 하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    [설치 프로그램 실행](goose://extension?type=streamable_http&url=https%3A%2F%2Frube.app%2Fmcp&id=rube&name=Rube&description=Seamlessly%20connect%20across%20500%2B%20applications%20including%20Slack%2C%20Gmail%2C%20Notion%2C%20Google%20Workspace%2C%20Microsoft%20Office%2C%20GitHub%2C%20and%20many%20more)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    `goose configure`를 사용하여 `Remote Extension (Streaming HTTP)` 확장 기능 유형을 추가하세요:

    **엔드포인트 URL**
    ```
    https://rube.app/mcp
    ```
  </TabItem>
</Tabs>

:::info OAUTH 흐름
브라우저에서 OAuth 창이 열립니다. 프롬프트를 따라 Rube 계정에 대한 액세스를 승인하세요.
:::

## Rube란?

Rube는 Composio가 지원하는 플랫폼으로 단일 통합을 통해 500개 이상의 앱과 서비스에 대한 통합 액세스를 제공합니다. 추가 설정 없이 다양한 애플리케이션 간의 원활한 연결을 가능하게 하여 개인 사용자와 팀 모두에게 완벽합니다.

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    <GooseDesktopInstaller
      extensionId="rube"
      extensionName="Rube"
      description="Slack, Gmail, Notion, Google Workspace, Microsoft Office, GitHub 등 500개 이상의 애플리케이션과 원활하게 연결"
      type="http"
      url="https://rube.app/mcp"
    />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="rube"
      description="Slack, Gmail, Notion, Google Workspace, Microsoft Office, GitHub 등 500개 이상의 애플리케이션과 원활하게 연결"
      type="http"
      url="https://rube.app/mcp"
      timeout={300}
      infoNote="Rube 도구를 처음 사용할 때 브라우저에서 OAuth 인증이 자동으로 발생합니다"
    />

  </TabItem>
</Tabs>

## 인증

Rube는 OAuth 브라우저 인증을 사용합니다. 이는 다음을 의미합니다:
- 수동으로 관리할 API 키 없음
- 보안 인증이 자동으로 처리됨
- Rube 도구를 처음 사용할 때 브라우저가 열려 관련 서비스로 인증
- 인증 토큰은 Rube에 의해 안전하게 저장되고 관리됨

## 사용 예시

Rube가 구성되면 goose를 통해 500개 이상의 앱과 직접 상호작용할 수 있습니다. 다음은 몇 가지 예시입니다:

**이메일 관리**
```
john@example.com에게 제목 "회의 업데이트"와 본문 "회의가 오후 3시로 변경되었습니다"로 이메일을 보내줘
```

**Slack 통신**
```
#general 채널에 "배포가 성공적으로 완료되었습니다"라는 메시지를 게시해줘
```

**GitHub 통합**
```
내 저장소에 제목 "버그 보고서"와 설명 "로그인 흐름에서 문제를 발견했습니다"로 새 이슈를 만들어줘
```

**Google Sheets**
```
내 "판매 데이터" 스프레드시트에 값이 있는 새 행을 추가해줘: 날짜: 오늘, 금액: 1500, 고객: Acme Corp
```

**캘린더 관리**
```
내일 오후 2시에 "프로젝트 리뷰" 제목으로 회의를 예약하고 sarah@company.com을 초대해줘
```

특정 서비스의 도구를 처음 사용할 때 브라우저에서 OAuth를 통해 인증하라는 프롬프트가 표시됩니다. 그 후에는 해당 서비스와의 모든 후속 상호작용이 원활하게 작동합니다.

## 문제 해결

- **확장 기능이 연결되지 않음**: 안정적인 인터넷 연결이 있고 네트워크에서 `https://rube.app`에 액세스할 수 있는지 확인하세요.
- **인증 문제**: OAuth 흐름이 작동하지 않으면 브라우저 캐시를 지우거나 다른 브라우저를 사용해 보세요.
- **도구 시간 초과**: 대규모 데이터셋을 사용하는 일부 작업은 더 오래 걸릴 수 있습니다. 필요한 경우 확장 기능 설정에서 시간 초과 값을 늘릴 수 있습니다.
- **속도 제한**: Rube는 개별 서비스의 속도 제한을 준수합니다. 제한에 도달하면 다시 시도하기 전에 몇 분 기다리세요.

## 도움 받기

- 통합별 질문에 대한 문서 및 지원은 [rube.app](https://rube.app)을 방문하세요
- 서비스 중단에 대해서는 Rube 상태 페이지를 확인하세요
