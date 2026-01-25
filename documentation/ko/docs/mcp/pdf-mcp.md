---
title: PDF Reader 확장 프로그램
description: PDF Reader MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/EJf2_iZfaWk" />

이 튜토리얼에서는 goose가 보호된 PDF와 보호되지 않은 PDF에서 텍스트를 읽고 추출할 수 있도록 [PDF Reader MCP 서버](https://github.com/michaelneale/mcp-read-pdf)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=mcp-read-pdf&id=pdf_read&name=PDF%20Reader&description=Read%20large%20and%20complex%20PDF%20documents)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  uvx mcp-read-pdf
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="pdf_read"
    extensionName="PDF Reader"
    description="대용량 및 복잡한 PDF 문서 읽기"
    command="uvx"
    args={["mcp-read-pdf"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="PDF Reader"
      description="대용량 및 복잡한 PDF 문서 읽기"
      command="uvx mcp-read-pdf"
    />
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 PDF Reader 확장 프로그램을 사용하여 특정 속성에 대한 지원자의 이력서를 분석하는 방법을 보여줍니다. PDF Reader 확장 프로그램을 사용하면 goose가 특정 페이지 또는 전체 문서를 읽고 대용량 및 복잡한 PDF에서 텍스트를 추출할 수 있습니다.

### goose 프롬프트

```
~/Downloads/resume.pdf에 있는 이력서를 읽고 다음 역할 요구 사항에 이 후보자가 얼마나 적합한지 평가해 주세요:

  - 5년 이상의 백엔드 개발 경험
  - 강력한 시스템 설계 및 분산 시스템 지식
  - 클라우드 인프라 경험 (AWS 선호)
  - 기술 프로젝트 또는 팀 리딩 경험
  - 보너스: LLM 또는 AI/ML 도구에 대한 친숙도

각 요구 사항에 대해 5점 만점의 점수, 근거 증거, 그리고 마지막에 장점, 단점, 결정(Strong Fit / Moderate Fit / Weak Fit)을 포함한 요약을 제공해 주세요.
```

### goose 출력

:::note CLI

이력서를 해당 요구 사항에 대해 분석하는 것을 도와드리겠습니다. 먼저 PDF를 읽겠습니다.

─── read_pdf | pdf ──────────────────

file_path: ~/Downloads/resume.pdf

이제 이력서 내용을 기반으로 각 요구 사항에 대해 후보자를 평가하겠습니다:

**1. 백엔드 개발 경험 (5년 이상)**

**점수: 5/5**

- 다양한 회사에서 7년 이상의 소프트웨어 엔지니어 경험
- Java, Python, Go를 사용한 광범위한 백엔드 개발 작업
- 여러 백엔드 서비스 및 API 구축 및 유지 관리
- 마이크로서비스 아키텍처 및 RESTful API에 대한 깊은 경험

**2. 시스템 설계 및 분산 시스템 지식**

**점수: 4.5/5**

- 분산 캐싱 시스템 설계 및 구현
- 높은 처리량의 메시지 큐잉 시스템 경험
- 마이크로서비스용 서비스 메시 아키텍처 구현
- 확장성 및 안정성 패턴에 대한 강력한 이해
- 매일 수백만 건의 트랜잭션을 처리하는 시스템 작업 경험

**3. 클라우드 인프라 경험 (AWS 선호)**

**점수: 4/5**

- 다음을 포함한 상당한 AWS 경험:
  - EC2, ECS 및 Lambda
  - S3 및 DynamoDB
  - CloudWatch 및 CloudFormation
- 코드형 인프라 경험
- Kubernetes 배포 및 관리
- 클라우드 환경에서의 CI/CD 파이프라인 구현

**4. 기술 프로젝트 또는 팀 리딩 경험**

**점수: 4.5/5**

- 5명의 엔지니어 팀을 이끌고 새로운 결제 처리 시스템 개발
- 주니어 개발자 멘토링 및 기술 면접 수행
- 시작부터 배포까지 여러 중요 프로젝트 관리
- 교차 기능 팀 성공적으로 조율
- 아키텍처 결정에서 기술 리더십 시연

**5. 보너스: LLM 또는 AI/ML 도구에 대한 친숙도**

**점수: 3.5/5**

- ML 모델 배포 파이프라인 구현 경험
- 자연어 처리 시스템 작업 경험
- ML 개념 및 프레임워크에 대한 기본 이해
- LLM 통합에 대한 일부 노출, 광범위하지는 않음

**요약**

**장점:**

- 필요한 백엔드 개발 경험 연수 초과
- 여러 도메인에 걸친 강력한 기술적 깊이
- 입증된 리더십 및 프로젝트 관리 능력
- 포괄적인 클라우드 인프라 경험
- 현대 AI/ML 기술에 대한 좋은 기반

**단점:**

- AWS 경험은 있지만 일부 최신 클라우드 서비스 경험이 도움이 될 수 있음
- AI/ML 경험은 좋지만 LLM에 깊이 전문화되지 않음
- 특정 클라우드 네이티브 패턴에 대한 더 많은 경험이 도움이 될 수 있음

**결정: Strong Fit**

후보자는 핵심 요구 사항, 특히 백엔드 개발 및 시스템 설계에서 탁월한 강점을 보여줍니다. 리더십 경험과 클라우드 인프라 지식이 매우 견고합니다. AI/ML 경험이 광범위하지는 않지만 보너스 자격으로 나열된 역할에는 충분합니다. 기술적 경험의 폭과 깊이, 입증된 팀 리딩 능력이 결합되어 이 포지션에 강력한 적합자가 됩니다.

전반적인 프로필은 즉각적인 기여가 가능하면서 추가 전문 지식이 필요한 영역에서 성장할 능력도 있음을 시사합니다. 분산 시스템에 대한 강력한 기반과 입증된 팀 리딩 경험은 기술 리더십 역할에 특히 가치가 있습니다.

**전체 점수 요약:**

1. 백엔드 개발: 5/5
2. 시스템 설계: 4.5/5
3. 클라우드 인프라: 4/5
4. 리더십 경험: 4.5/5
5. AI/ML (보너스): 3.5/5

**총점: 21.5/25** (보너스 제외)

:::
