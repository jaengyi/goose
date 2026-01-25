---
title: Repomix 확장 기능
description: Repomix MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/69h4LLkIg_E" />


이 튜토리얼은 [Repomix MCP 서버](https://github.com/yamadashy/repomix)를 goose 확장 기능으로 추가하여 자동화된 저장소 패킹, 코드베이스 분석, 아키텍처 요약, 테스트 생성 및 코드 탐색을 활성화하면서 LLM의 컨텍스트 제한 내에서 유지하기 위해 토큰 사용을 최소화하도록 코드베이스를 압축하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=repomix&arg=--mcp&id=repomix&name=Repomix&description=Pack%20repositories%20into%20AI-friendly%20formats%20for%20goose)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx -y repomix --mcp
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용).
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseDesktopInstaller
    extensionId="repomix"
    extensionName="Repomix"
    description="goose를 위해 저장소를 AI 친화적 형식으로 패킹"
    command="npx"
    args={["-y", "repomix", "--mcp"]}
  />
</TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Repomix"
      description="goose를 위해 저장소를 AI 친화적 형식으로 패킹"
      command="npx -y repomix --mcp"
    />
  </TabItem>
</Tabs>

## 사용 예시

이 예시에서는 Repomix 확장 기능과 함께 goose를 사용하여 압축된 Axios 저장소를 분석하는 방법을 보여드리겠습니다. goose는 `lib/core/Axios.js` 파일에 집중하여 아키텍처에서의 역할을 설명하고, 누락된 테스트를 발견하고, 새로운 단위 테스트를 작성하고, 테스트 전략 및 코드 유지보수성에 대한 개선 사항을 제안합니다.

전체 코드베이스를 AI 친화적 형식으로 압축함으로써 Repomix는 goose가 토큰 제한 내에서 유지하면서 상세하고 파일별 인사이트를 제공하는 데 도움을 줍니다.

### goose 프롬프트

> Axios 저장소에서 lib/core/Axios.js 파일을 검토하고 핵심 함수와 클래스를 철저히 커버하는 데 필요한 누락된 단위 테스트를 식별해줘. 이 파일이 전체 Axios 아키텍처에서 어떤 역할을 하는지, 다른 모듈과 어떻게 통합되는지, 현재 얼마나 잘 테스트되어 있는지 설명해줘. 커버리지를 개선하기 위한 새로운 단위 테스트를 작성하고, 코드를 더 유지보수 가능하게 만들기 위한 테스트 전략 또는 리팩토링 기회에 대한 개선 사항을 권장해줘.


### goose 출력

:::note CLI

Axios.js 핵심 파일과 테스트 커버리지를 분석하는 데 도움을 드리겠습니다. 단계별로 분해해 보겠습니다:

1. 먼저 repomix를 사용하여 Axios 저장소를 가져와 검사합니다
2. 그런 다음 특정 파일과 관련 테스트를 분석합니다
3. 마지막으로 권장 사항과 새 단위 테스트를 제공합니다

저장소를 가져오는 것부터 시작하겠습니다:

--- 원격 저장소 패킹 ---

이제 핵심 Axios.js 파일과 관련 테스트 파일을 읽겠습니다:

--- Repomix 출력 읽기 ---

Axios 코드베이스 분석을 기반으로 프로젝트 구조에 대한 포괄적인 개요를 제공하고 개선된 README를 만들겠습니다. 섹션으로 나누어 설명하겠습니다:

**Axios 아키텍처에서의 역할**

`Axios.js` 파일은 Axios 라이브러리의 가장 중요한 핵심 구성 요소 중 하나입니다. 여러 주요 목적을 수행합니다:

1. 요청 처리: HTTP 요청을 처리하고 발송하는 주요 `Axios` 클래스 포함
2. 구성 관리: 기본 구성과 요청별 구성의 병합 처리
3. 인터셉터 관리: 요청 및 응답 인터셉터 관리
4. HTTP 메서드 편의성: 다양한 HTTP 동사(get, post 등)에 대한 편의 메서드 제공

파일은 다음을 통해 다른 핵심 모듈과 통합됩니다:

- 요청/응답 인터셉터 처리를 위한 `InterceptorManager`
- 실제 HTTP 요청을 만들기 위한 `dispatchRequest`
- 구성 처리를 위한 `mergeConfig`
- URL 해결을 위한 `buildFullPath`
- 헤더 관리를 위한 `AxiosHeaders`

**현재 테스트 커버리지 분석**

Axios.js의 현재 테스트 커버리지는 상당히 좋지만 일부 공백이 있습니다. 테스트 파일을 보면:

1. `test/unit/core/Axios.js` - 주로 오류 처리 케이스, 특히 쓰기 불가능한 오류 스택에 초점
2. `test/specs/api.spec.js` - 공개 API 표면 테스트
3. `test/specs/interceptors.spec.js` - 인터셉터 기능 테스트
4. `test/specs/instance.spec.js` - 인스턴스 생성 및 구성 테스트

**누락된 테스트 커버리지**
1. 오류 처리
2. 구성 처리
3. 메서드 별칭
4. 인터셉터 체인

**테스트 전략 권장 사항**

1. 모듈식 테스트:
- 테스트를 더 작고 집중된 단위로 분해
- 각 HTTP 메서드 별칭을 별도로 테스트
- 구성 처리, 인터셉터 및 오류 처리를 위한 전용 테스트 스위트 생성

2. 통합 지점 모킹:
- 더 나은 격리를 위해 의존성 주입 사용
- 외부 의존성을 일관되게 모킹
- 재사용 가능한 모킹 팩토리 생성

이러한 개선 사항은 코드를 더 유지보수 가능하고 테스트하기 쉽게 만들면서 오류 처리와 관심사 분리를 개선합니다.
:::
