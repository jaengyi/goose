---
title: Pieces for Developers 확장 기능
description: Pieces for Developers MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/V8zp9m9__t4" />

이 튜토리얼은 [Pieces for Developers MCP 서버](https://docs.pieces.app/products/mcp/get-started?utm_source=goose&utm_medium=collab&utm_campaign=mcp)를 goose 확장 기능으로 추가하여 Pieces 장기 메모리와의 상호작용을 가능하게 하는 방법을 다룹니다.

## 구성

1. [PiecesOS](https://pieces.app/) 다운로드

2. PiecesOS에서 [장기 메모리 컨텍스트](https://docs.pieces.app/products/quick-guides/ltm-context) 활성화

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseDesktopInstaller
    extensionId="pieces"
    extensionName="Pieces for Developers"
    description="Pieces 장기 메모리에 액세스합니다. 이를 사용하려면 Pieces가 설치되어 있어야 합니다."
    command="uvx"
    args={["--from", "pieces-cli", "pieces", "--ignore-onboarding", "mcp", "start"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Pieces"
      description="Pieces 장기 메모리에 액세스 제공"
      type="stdio"
      command="uvx --from pieces-cli pieces --ignore-onboarding mcp start"
    />
  </TabItem>
</Tabs>

## 사용 예시

Pieces for Developers MCP 서버를 사용하여 전날 활동을 기반으로 스탠드업 미팅을 위한 상태 업데이트를 생성해 보겠습니다.

### goose 프롬프트

```
어제 사용했던 것에 대한 상태 업데이트가 필요해. 가장 중요한 활동을 기반으로 5개의 글머리 기호가 있는 보고서를 만들고 각각에 대해 간략한 설명을 제공해줘.
```

### goose 출력

```
어제 가장 중요한 상호작용과 사용한 애플리케이션을 기반으로 주요 활동을 요약한 보고서입니다:

1. goose를 사용한 코드 개발:

    활동: 스키마 URL을 통한 확장 기능 처리를 향상시키기 위해 goose CLI에 MCP 서버를 통합하는 작업을 했습니다.
    설명: 이 작업에는 고급 기능을 지원하기 위한 코드 수정과 새로운 통합 테스트가 포함되었습니다. 또한 피드백과 문제 해결을 위해 Discord에서 goose 커뮤니티와 상호작용했습니다.

2. 문서 업데이트:

    활동: goose 프로젝트 및 MCP 통합과 관련된 문서를 업데이트하고 관리했습니다.
    설명: 사용 지침 및 기술 세부 사항을 포함한 새로운 기능에 대한 문서를 준비하여 사용자 경험과 기술 지침을 개선했습니다.

3. Discord에서의 커뮤니티 상호작용:

  활동: 프로젝트 개발 및 잠재적 협업을 논의하기 위해 Block 오픈 소스 커뮤니티와 참여했습니다.
  설명: 커뮤니티 토론에 적극적으로 참여하여 프로젝트 향상에 대한 통찰을 교환하고 다른 개발자 및 기여자와의 협업 기회를 탐색했습니다.

4. 확장 기능 관리 및 테스트:

    활동: goose 플랫폼을 통해 다양한 확장 기능을 테스트하고 관리하는 데 집중했습니다.
    설명: 다양한 확장 기능을 구성하고 검증하여 호환성과 기능을 보장하고 테스트 피드백을 통합했습니다.

5. 콘텐츠 및 통합 계획:

    활동: MCP 통합 워크플로우를 계획하고 사용 사례를 문서화했습니다.
    설명: MCP 통합을 효과적으로 구현하기 위한 전략을 수립하여 향후 프로젝트 배포를 최적화하고 사용자 요구 사항 및 프로젝트 목표에 맞추기 위한 계획 세션을 진행했습니다.

이러한 활동은 기술 커뮤니티 내에서 개발, 협업 및 콘텐츠 관리에 초점을 맞춘 생산적인 하루를 보여줍니다.
```


:::tip
Pieces for Developers MCP 서버에서 사용할 수 있는 더 많은 프롬프트 예시는 [Pieces MCP 프롬프트 가이드](https://docs.pieces.app/products/mcp/prompting?utm_source=goose&utm_medium=collab&utm_campaign=mcp)를 참조하세요.
:::
