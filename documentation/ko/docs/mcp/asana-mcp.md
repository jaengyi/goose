---
title: Asana 확장 프로그램
description: Asana MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<!--<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/VIDEO_ID" />-->


이 튜토리얼에서는 작업 자동화, 프로젝트 추적, 팀 협업을 활성화하기 위해 [Asana MCP 서버](https://github.com/roychri/mcp-server-asana)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40roychri%2Fmcp-server-asana&id=asana&name=Asana&description=enable%20task%20automation%2C%20project%20tracking%2C%20and%20team%20collaboration&env=ASANA_ACCESS_TOKEN%3DAsana%20Access%20Token)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @roychri/mcp-server-asana
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  ASANA_ACCESS_TOKEN: <YOUR_TOKEN>
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::


<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="asana"
    extensionName="Asana"
    description="작업 자동화, 프로젝트 추적, 팀 협업 활성화"
    command="npx"
    args={["-y", "@roychri/mcp-server-asana"]}
    envVars={[
      { name: "ASANA_ACCESS_TOKEN", label: "Asana Access Token" }
    ]}
    apiKeyLink="https://app.asana.com/0/my-apps"
    apiKeyLinkText="Asana Access Token"
  />
  :::info
  액세스 토큰 생성에 대한 자세한 지침이 필요하면 [Asana 개발자 문서](https://developers.asana.com/docs/personal-access-token)를 참조하세요.
  :::
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Asana"
      description="작업 자동화, 프로젝트 추적, 팀 협업 활성화"
      command="npx -y @roychri/mcp-server-asana"
      envVars={[
        { key: "ASANA_ACCESS_TOKEN", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          <a href="https://app.asana.com/0/my-apps" target="_blank" rel="noopener noreferrer">Asana Access Token</a>을 얻어서 붙여넣으세요. 액세스 토큰 생성에 대한 자세한 지침이 필요하면 <a href="https://developers.asana.com/docs/personal-access-token" target="_blank" rel="noopener noreferrer">Asana 개발자 문서</a>를 참조하세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예제

:::info LLM
이 작업에는 OpenAI의 GPT-4o가 사용되었습니다. Amazon Bedrock 모델에는 [알려진 버그](https://github.com/block/goose/issues/1804)가 있습니다.
:::

### goose 프롬프트

> _goose, 한 시간이 있어. Asana에서 나에게 할당된 미완료 작업을 살펴보고 한 시간 이내에 완료할 수 있을 것으로 예상되는 작업을 보여줘. 마감일 순으로 정렬해 줘._


### goose 출력

ID는 일반적인 것으로 변경되었습니다

:::note CLI

<details>
    <summary>Tool Calls</summary>

    ─── asana_search_tasks | abcdefgh ──────────────────────────

    assignee.any: me

    completed: false

    workspace: 1234567890123



    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000001


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000002


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000003


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000004


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000005


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000006


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000007


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000008


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000009


    ─── asana_get_task | abcdefgh ──────────────────────────

    opt_fields: due_on,estimated_hours

    task_id: 1100000000000010
</details>



마감일 순으로 정렬된, 한 시간 이내에 완료할 수 있을 것으로 예상되는 Asana의 미완료 작업입니다:

1. **작업:** 팀 회의 노트 검토
   - **마감일:** 2025-03-21

2. **작업:** 간단한 상태 업데이트 작성
   - **마감일:** 2025-03-21

3. **작업:** 블로그 게시물 교정
   - **마감일:** 2025-03-21

4. **작업:** 파트너에게 리마인더 보내기
   - **마감일:** 2025-03-27

더 많은 정보나 특정 작업에 대한 도움이 필요하시면 알려주세요!
:::
