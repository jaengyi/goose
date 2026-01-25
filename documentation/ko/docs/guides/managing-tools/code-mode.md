---
sidebar_position: 3
title: 코드 모드
sidebar_label: 코드 모드
description: 코드 모드가 도구 검색 및 도구 호출을 처리하는 방법 이해
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

코드 모드는 MCP 도구를 직접 호출하는 대신 프로그래밍 방식으로 상호 작용하는 방법입니다. 코드 모드는 많은 확장 기능이 활성화된 상태에서 작업할 때 컨텍스트 윈도우 사용량을 더 효율적으로 관리하는 데 도움이 될 수 있어 특히 유용합니다.

:::info
이 기능을 사용하려면 내장된 [Code Execution 확장 기능](/docs/mcp/code-execution-mcp)이 활성화되어 있어야 합니다.
:::

코드 모드는 도구가 검색되고 호출되는 방식을 제어합니다:
- 활성화된 확장 기능의 도구가 필요에 따라 검색되어 컨텍스트에 로드됩니다
- 여러 도구 호출이 하나의 실행으로 일괄 처리됩니다
- 중간 결과가 연결됩니다 (한 도구의 출력이 다음 도구의 입력으로)

## 코드 모드 작동 방식

[Code Execution 확장 기능](/docs/mcp/code-execution-mcp)은 MCP 프로토콜을 사용하여 세 가지 기본 메타 도구를 노출하는 MCP 서버입니다. Code Execution이 활성화되면 goose는 코드 모드로 전환합니다. 모든 요청에 대해 LLM은 goose가 샌드박스 환경에서 실행하는 JavaScript 코드를 작성하여:
- 활성화된 확장 기능에서 사용 가능한 도구를 검색합니다 (필요한 경우)
- 현재 작업에 필요한 도구와 함께 작업하는 방법을 학습합니다
- 해당 도구를 프로그래밍 방식으로 호출하여 작업을 완료합니다

### 전통적인 방식 vs. 코드 모드 도구 호출

전통적인 MCP 도구 호출과 코드 모드는 동일한 목표에 대한 두 가지 다른 접근 방식입니다: goose에게 도구에 대한 액세스를 제공하는 것.

| 측면 | 전통적 방식 | 코드 모드 |
|--------|------------------|-----------|
| **도구 검색** | 활성화된 확장 기능의 모든 도구, 예:<br/>• `developer.shell`<br/>• `developer.text_editor`<br/>• `github.list_issues`<br/>• `github.get_pull_request`<br/>• `slack.send_message`<br/>• ... *잠재적으로 더 많음* | Code Execution 확장 기능의 메타 도구:<br/>• `search_modules`<br/>• `read_module`<br/>• `execute_code`<br/><br/>LLM은 이러한 도구를 사용하여 필요에 따라 다른 활성화된 확장 기능의 도구를 검색합니다 |
| **도구 호출** | • 순차적 도구 호출<br/>• 각 결과가 다음 호출 전에 LLM으로 전송됨 | • 도구 검색 호출이 필요할 수 있음<br/>• 여러 도구 호출이 하나의 실행으로 일괄 처리됨<br/>• 중간 결과가 연결되어 로컬에서 처리됨 |
| **컨텍스트 윈도우** | 모든 LLM 호출에 활성화된 확장 기능의 모든 도구 정의가 포함됨 | 모든 LLM 호출에 3개의 메타 도구 정의와 세션에서 이전에 검색된 도구 정의가 포함됨 |
| **적합한 용도** | • 1-3개의 활성화된 확장 기능<br/>• 1-2개 도구를 사용하는 간단한 작업 | • 5개 이상의 확장 기능<br/>• 잘 정의된 다단계 워크플로우 |

:::info 텍스트 전용 결과
코드 모드는 도구 결과에서 텍스트 콘텐츠만 지원합니다. 이미지, 바이너리 데이터 및 기타 콘텐츠 유형은 무시됩니다.
:::

## 추가 리소스

import ContentCardCarousel from '@site/src/components/ContentCardCarousel';
import gooseCodeMode from '@site/blog/2025-12-15-code-mode-mcp/header-image.jpg';
import notMcpReplacement from '@site/blog/2025-12-21-code-mode-doesnt-replace-mcp/header-image.png';

<ContentCardCarousel
  items={[
    {
      type: 'blog',
      title: 'MCP를 위한 코드 모드',
      description: 'MCP 도구 호출에 대한 코드 실행 접근 방식을 알아보세요.',
      thumbnailUrl: gooseCodeMode,
      linkUrl: '/goose/blog/2025/12/15/code-mode-mcp',
      date: '2025-12-15',
      duration: '5분 읽기'
    },
    {
      type: 'blog',
      title: '코드 모드는 MCP를 대체하지 않습니다',
      description: '코드 모드와 MCP가 함께 작동하는 방식 이해.',
      thumbnailUrl: notMcpReplacement,
      linkUrl: '/goose/blog/2025/12/21/code-mode-doesnt-replace-mcp',
      date: '2025-12-21',
      duration: '8분 읽기'
    }
  ]}
/>
