---
title: Cognee 확장 프로그램
description: Cognee MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

이 튜토리얼에서는 지식 그래프 메모리 기능을 활성화하여 30개 이상의 데이터 소스에 연결하여 향상된 컨텍스트 및 검색을 위해 [Cognee MCP 서버](https://github.com/topoteretes/cognee/tree/main/cognee-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
**Command**
```sh
uv --directory /path/to/cognee-mcp run python src/server.py
```
**환경 변수**
```
LLM_API_KEY: <YOUR_OPENAI_API_KEY>
```
:::

## 구성

:::info
이 명령은 `uv`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="cli" label="goose CLI" default>

**Cognee 설치:**

```bash
# Cognee 클론 및 설치
git clone https://github.com/topoteretes/cognee
cd cognee/cognee-mcp
uv sync --dev --all-extras --reinstall

# Linux에서는 추가 종속성 설치
sudo apt install -y libpq-dev python3-dev
```

**확장 프로그램 구성:**

<CLIExtensionInstructions
  name="Cognee"
  description="30개 이상의 데이터 소스 연결이 가능한 지식 그래프 메모리 기능"
  command="uv --directory /path/to/cognee-mcp run python src/server.py"
  envVars={[
    { key: "LLM_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
  ]}
  commandNote={
    <>
      <code>/path/to/cognee-mcp</code>를 클론한 cognee-mcp 디렉토리의 실제 경로로 바꾸세요.
    </>
  }
  infoNote={
    <>
      LLM 공급자의 API 키를 얻어서 붙여넣으세요. 기본적으로 이것은 <a href="https://platform.openai.com/api-keys" target="_blank" rel="noopener noreferrer">OpenAI API 키</a>입니다.
    </>
  }
/>
  </TabItem>
</Tabs>

:::info
지원되는 구성 옵션은 [Cognee MCP 문서](https://docs.cognee.ai/how-to-guides/deployment/mcp)를 참조하세요.
:::

## 사용 예제

Cognee는 goose에 지식 그래프 메모리 기능을 제공하여 대화 및 문서 전반에 걸쳐 정보를 기억하고 연결할 수 있게 합니다.

:::info LLM
이 작업에는 OpenAI의 GPT-4o가 사용되었습니다.
:::

### goose 프롬프트

> _goose, 이 정보를 cognify해줘: "데이터 분석에는 Python을 선호하고 pandas를 광범위하게 사용합니다. 현재 프로젝트는 고객 행동 데이터 분석과 관련이 있습니다." 그런 다음 내 프로그래밍 선호도에 대한 정보를 검색해줘._

### goose 출력

:::note CLI

<details>
    <summary>Tool Calls</summary>

    ─── cognee_cognify | abcdefgh ──────────────────────────

    data: I prefer Python for data analysis and use pandas extensively. My current project involves analyzing customer behavior data.


    ─── cognee_search | abcdefgh ──────────────────────────

    search_query: programming preferences Python data analysis

    search_type: GRAPH_COMPLETION

</details>

정보를 지식 그래프에 저장한 다음 프로그래밍 선호도를 검색하는 것을 도와드리겠습니다.

먼저, 선호도에 대한 정보를 cognify하겠습니다:

**Cognify 상태:**
MCP 타임아웃 제한으로 인해 백그라운드 프로세스가 시작되었습니다. Python과 pandas에 대한 데이터 분석 선호도 정보가 지식 그래프에 추가되었습니다.

이제 프로그래밍 선호도에 대한 정보를 검색하겠습니다:

**검색 결과:**
지식 그래프에서 프로그래밍 선호도에 대한 정보를 찾았습니다:

- **언어 선호도**: 데이터 분석에 Python을 선호합니다
- **주요 라이브러리**: 데이터 조작에 pandas를 광범위하게 사용합니다
- **현재 관심사**: 현재 프로젝트는 고객 행동 데이터 분석과 관련이 있습니다
- **도메인**: 데이터 과학 및 분석 워크플로우
- **기술 스택**: 데이터 분석 작업을 위한 Python + pandas

지식 그래프가 프로그래밍 선호도와 프로젝트 컨텍스트를 성공적으로 저장했으며 이제 향후 대화에서 검색할 수 있습니다.

:::
