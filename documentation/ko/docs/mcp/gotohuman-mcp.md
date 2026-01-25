---
title: gotoHuman 확장 프로그램
description: gotoHuman MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';


<!-- <YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/6aV8pinnUS8" />  -->

이 튜토리얼에서는 AI 워크플로우에 **휴먼 인 더 루프 승인**을 가져오기 위해 [gotoHuman MCP 서버](https://github.com/gotohuman/gotohuman-mcp-server)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다. gotoHuman을 사용하면 goose가 일시 중지하고 계속하기 전에 검토를 요청할 수 있어, 블로그 초안, 코드 리뷰, 규정 준수 확인 등에 완벽합니다.


:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40gotohuman%2Fmcp-server&id=gotoHuman&name=gotoHuman&description=gotoHuman%20MCP%20server%20for%20human-in-the-loop%20approvals&env=GOTOHUMAN_API_KEY%3DgotoHuman%20API%20Key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
   npx -y @gotohuman/mcp-server
  ```
  **환경 변수**
  ```sh
  GOTOHUMAN_API_KEY: <YOUR_API_KEY>
  ```
  </TabItem>
</Tabs>
:::

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="gotoHuman"
    extensionName="gotoHuman"
    description="휴먼 인 더 루프 승인을 위한 gotoHuman MCP 서버"
    command="npx"
    args={["-y", "@gotohuman/mcp-server"]}
    envVars={[
      { name: "GOTOHUMAN_API_KEY", label: "gotoHuman API Key" }
    ]}
    apiKeyLink="https://app.gotohuman.com/accounts/3kiXJvmf4UDBPKPi9ZNY/api-keys"
    apiKeyLinkText="GOTOHUMAN_API_KEY"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
      <CLIExtensionInstructions
        name="gotoHuman MCP"
        description="휴먼 인 더 루프 승인을 위한 gotoHuman MCP 서버"
        command="npx -y @gotohuman/mcp-server"
        timeout={300}
        envVars={[
            { key: "GOTOHUMAN_API_KEY", value: "<YOUR_API_KEY>" }
        ]}
        infoNote={
            <>
            <a href="https://app.gotohuman.com/accounts/3kiXJvmf4UDBPKPi9ZNY/api-keys" target="_blank" rel="noopener noreferrer">gotoHuman API KEY</a>를 얻어서 붙여넣으세요
            </>
        }
      />
  </TabItem>
</Tabs>

## 사용 예제

:::tip 시작하기 전에
[app.gotohuman.com](https://app.gotohuman.com)에 로그인하고 **Review Templates → Create Template**로 이동하세요.
템플릿을 만드는 단계를 따르고 **Webhook Endpoint**를 설정하세요 (테스트용으로 [webhook.site](https://webhook.site)를 사용할 수 있습니다).
:::


이 예제에서 goose는 생성된 `n8n news to post` 템플릿을 사용하여 LinkedIn 게시물 초안을 gotoHuman에 승인을 위해 보냅니다.

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

### goose 프롬프트
```
내 `n8n news to post` 양식을 사용하여 이 goose에 관한 블로그 초안을 gotoHuman에 검토를 위해 보내줘.

타임스탬프로 오늘 날짜를 포함하고, 다음 링크들:
[goose Docs: https://block.github.io/goose/, gotoHuman: https://gotohuman.com/],
'인간 승인을 위한 goose와 gotoHuman의 통합 소개'로 요약하고,
여기 초안이야:

goose는 터미널이나 IDE에서 로컬 머신에서 실행되는 오픈 소스 AI 에이전트입니다.
gotoHuman과 같은 MCP 서버에 연결하여 기능을 확장합니다. goose가 작업을 자동화할 수 있지만,
때때로 휴먼 인 더 루프가 필요합니다. gotoHuman MCP를 사용하면 모든 워크플로우에 승인 단계를 추가할 수 있습니다;
LinkedIn 게시물 초안 검토부터 코드 변경 승인까지.
승인되면 goose가 중단된 곳에서 계속할 수 있습니다.
이는 에이전트 워크플로우를 더 안전하고, 더 협력적이며, 실제 팀에 더 적합하게 만듭니다.
```
### goose 출력

:::note Desktop


`─── list-forms ──────────────────────────`

이제 특정 설정 및 사용 정보를 검색하겠습니다:

`─── Get-Form-Schema`

더 많은 기술적 세부 사항과 예제를 검색하겠습니다:

`─── Request-human-review-with-form ────────────`

완료! "n8n news to post" 양식을 사용하여 goose 블로그 초안을 gotoHuman에 검토를 위해 성공적으로 보냈습니다. 제출된 내용은 다음과 같습니다:
검토 요청 세부 사항

- 사용된 양식: n8n news to post
- 검토 ID: 6Tp7Z0lqH7lu52MfuHZq
- 검토 링크: https://app.gotohuman.com/accounts/3kiXJvmf4UDBPKPi9ZNY/reviews/6Tp7Z0lqH7lu52MfuHZq

제출된 콘텐츠

- 타임스탬프: 2025-09-23
- 요약: 인간 승인을 위한 goose와 gotoHuman의 통합 소개
- 링크:
    - goose Docs
    - gotoHuman
- 초안: goose와 gotoHuman의 통합에 관한 전체 블로그 게시물

검토 요청이 생성되었으며 이제 인간 승인을 기다리고 있습니다. 위에 제공된 검토 링크를 통해 액세스할 수 있습니다. 검토가 완료되면 블로그 게시물을 게시하거나 요청된 변경 사항을 적용할 수 있습니다.
:::

## gotoHuman에서 검토

이제 goose 출력의 **검토 링크**를 열면 gotoHuman 대시보드에서 대기 중인 요청을 볼 수 있습니다:

![](/img/gotohuman.png)
