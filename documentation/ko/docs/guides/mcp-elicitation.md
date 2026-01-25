---
sidebar_position: 55
title: MCP 정보 요청
sidebar_label: MCP 정보 요청
description: 확장 기능이 작업 중 구조화된 정보를 요청하는 방법
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

MCP 정보 요청(Elicitation)을 사용하면 확장 기능에 정보가 필요할 때 goose가 일시 중지하고 특정 정보를 요청할 수 있습니다. 추측하거나 가정하는 대신 goose는 계속 진행하는 데 필요한 것을 정확히 요청하는 양식을 제시합니다.

이 기능은 goose에서 자동으로 활성화됩니다. 정보 요청을 지원하는 확장 기능이 정보가 필요하면 세션에 양식이 나타납니다.

:::info
[MCP 정보 요청](https://modelcontextprotocol.io/specification/draft/client/elicitation)은 Model Context Protocol의 기능입니다. goose는 양식 모드 요청을 지원합니다.
:::

## MCP 정보 요청 작동 방식

확장 기능에 정보가 필요하면 goose가 일시 중지하고 작성할 양식을 제시합니다. 응답을 제출하거나 요청을 취소할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

    채팅에 인라인으로 양식이 나타나며 다음을 포함합니다:
    - 요청된 데이터를 위한 필드
    - 별표(*)로 표시된 필수 필드
    - 수락하거나 변경할 수 있는 기본값
    - 응답을 보내는 **제출** 버튼

    제출 후 확인 메시지가 표시됩니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    터미널에 프롬프트가 나타나며 다음을 포함합니다:
    - 어떤 정보가 필요한지 설명하는 메시지(청록색)
    - 설명이 포함된 필드 이름(노란색)
    - 빨간색 별표(*)로 표시된 필수 필드
    - 대괄호 안에 표시된 기본값, 예: `[default]`

    각 필드에 응답을 입력하고 Enter를 누르세요. 예/아니오 질문의 경우 대화형 토글이 표시됩니다.

    요청을 취소하려면 `Ctrl+C`를 누르세요.

  </TabItem>
</Tabs>

:::info 시간 초과
정보 요청은 5분 후에 시간 초과됩니다. 시간 내에 응답하지 않으면 요청이 취소되고 goose는 해당 정보 없이 계속합니다.
:::

## 확장 기능 개발자를 위해

자체 확장 기능에 정보 요청을 추가하고 싶으신가요? MCP 서버가 사용자로부터 구조화된 입력을 요청하는 방법을 알아보려면 [MCP 정보 요청 사양](https://modelcontextprotocol.io/specification/draft/client/elicitation)을 참조하세요.
