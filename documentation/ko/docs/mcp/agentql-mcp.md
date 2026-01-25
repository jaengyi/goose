---
title: AgentQL 확장 프로그램
description: AgentQL MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<!-- <YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/VIDEO_ID" /> -->

이 튜토리얼에서는 비구조화된 웹 콘텐츠를 구조화된 데이터로 추출하고 변환하기 위해 [AgentQL MCP 서버](https://github.com/tinyfish-io/agentql-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=agentql-mcp&id=agentql&name=AgentQL&description=Transform%20unstructured%20web%20content%20into%20structured%20data&env=AGENTQL_API_KEY%3DAgentQL%20API%20Key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y agentql-mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  AGENTQL_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="agentql"
    extensionName="AgentQL"
    description="비구조화된 웹 콘텐츠를 구조화된 데이터로 변환"
    command="npx"
    args={["-y", "agentql-mcp"]}
    envVars={[
      { name: "AGENTQL_API_KEY", label: "AgentQL API Key" }
    ]}
    apiKeyLink="https://dev.agentql.com/api-keys"
    apiKeyLinkText="AGENTQL_API_KEY"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="agentql"
      description="비구조화된 웹 콘텐츠를 구조화된 데이터로 변환"
      command="npx -y agentql-mcp"
      envVars={[
        { key: "AGENTQL_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          <a href="https://dev.agentql.com/api-keys" target="_blank" rel="noopener noreferrer">AgentQL API Key</a>를 얻어서 붙여넣으세요.
        </>
      }
    />

  </TabItem>
</Tabs>

## 사용 예제

AgentQL 확장 프로그램을 사용하여 발표 일정 계획에 도움이 되는 기술 컨퍼런스 데이터를 수집하고 구조화해 보겠습니다.

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::

### goose 프롬프트

```
저는 2025-2026 제출을 계획 중인 기술 컨퍼런스 발표자입니다.

2022-2024년에 개최된 개발자 컨퍼런스(참석자 500명 이상)에서 다음을 추출해 주세요:
- 컨퍼런스 이름
- 컨퍼런스 날짜
- CFP 일정

다음을 식별해 주세요:
- 일관된 월별 패턴
- 컨퍼런스가 매년 같은 달에 개최되는지
- CFP 기간이 매년 일관된지
- 전통적인 시기의 변화

결과를 JSON으로 구조화해 주세요
```

### goose 출력

```
웹 데이터 추출 도구를 사용하여 컨퍼런스 패턴을 분석하겠습니다. 포괄적인 데이터를 얻기 위해 몇 가지 쿼리로 나누겠습니다.

─── extract-web-data | agentql ──────────────────────────
prompt: ...
url: ...

패턴을 비교하기 위해 과거 데이터를 가져오겠습니다:

─── extract-web-data | agentql ──────────────────────────
prompt: ....
url: ....

조사와 사용 가능한 데이터를 기반으로 주요 기술 컨퍼런스 패턴 분석을 제공하겠습니다. 다음은 발견 사항이 포함된 구조화된 JSON입니다:

{
  "analysis": {
    "major_conference_patterns": {
      "timing_consistency": {
        "spring_events": {
          "typical_months": ["March", "April", "May"],
          "notable_conferences": [
            "Google I/O",
            "Microsoft Build",
            "DevOpsDays (various locations)"
          ],
          "cfp_patterns": "일반적으로 이벤트 4-6개월 전에 열리고 2-3개월 전에 마감"
        },
        "fall_events": {
          "typical_months": ["September", "October", "November"],
          "notable_conferences": [
            "AWS re:Invent",
            "KubeCon + CloudNativeCon"
          ],
          "cfp_patterns": "일반적으로 이벤트 6-8개월 전에 열리고 4-5개월 전에 마감"
        }
      },
      "observed_trends": {
        "timing_consistency": "높음 - 대부분의 주요 컨퍼런스가 매년 일관된 달 유지",
        "cfp_windows": {
          "typical_duration": "6-12주",
          "advance_notice": "이벤트 4-8개월 전",
          "variation": "지역 이벤트는 더 짧은 CFP 기간(4-6주) 경향"
        },
        "recent_shifts": [
          "2022년 이후 하이브리드/가상 옵션 증가",
          "주요 컨퍼런스의 더 많은 지역 에디션",
          "대형 이벤트의 더 빠른 CFP 마감"
        ]
      }
    },
    "recommendations_for_2025_2026": {
      "spring_submissions": {
        "prepare_by": "전년도 9-10월",
        "submit_during": "전년도 11-12월"
      },
      "fall_submissions": {
        "prepare_by": "2-3월",
        "submit_during": "3-4월"
      }
    }
  }
}


계획을 위한 주요 인사이트:

1. **월별 패턴**:
   - 봄 컨퍼런스 시즌: 3-5월
   - 가을 컨퍼런스 시즌: 9-11월
   - 여름은 주요 기술 컨퍼런스가 적은 경향
   - 12월-1월은 지역 이벤트를 제외하고 일반적으로 조용함

2. **연간 일관성**:
   - 주요 컨퍼런스(AWS re:Invent, KubeCon, Google I/O)는 시기가 매우 일관적
   - 주요 컨퍼런스의 지역 버전(DevOpsDays 등)은 날짜 유연성이 더 큼
   - 대부분 매년 같은 분기/시즌 유지

3. **CFP 기간**:
   - 대형 컨퍼런스: 이벤트 6-8개월 전
   - 지역 컨퍼런스: 이벤트 3-4개월 전
   - 주요 컨퍼런스에 대한 더 긴 검토 기간 추세
   - 많은 컨퍼런스가 롤링 또는 다단계 CFP 프로세스 도입

4. **최근 변화**:
   - 2022년 이전 대비 더 빠른 CFP 마감
   - 더 많은 하이브리드/가상 구성 요소
   - 주요 컨퍼런스의 지역 에디션 증가
   - 제출물에 대한 더 긴 검토 기간

2025-2026 계획을 위한 권장 사항:
- 2025년 2-3월까지 2025년 가을 제출 준비 시작
- 2025년 9-10월까지 2026년 봄 제출 준비 시작
- 주요 컨퍼런스의 지역 버전은 마감이 늦은 경우가 많으므로 고려
- 다중 트랙 컨퍼런스는 트랙별로 다른 CFP 마감일이 있을 수 있으므로 추적

이러한 패턴의 특정 측면에 집중하거나 특정 컨퍼런스에 대한 더 자세한 정보를 원하시면 알려주세요.
```
