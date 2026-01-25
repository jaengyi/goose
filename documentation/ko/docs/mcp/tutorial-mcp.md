---
title: Tutorial 확장 기능
description: 가이드 학습을 위한 goose의 내장 Tutorial 확장 기능 사용 방법 알아보기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

Tutorial 확장 기능은 goose의 다양한 측면과 기능을 배우기 위한 대화형, 단계별 가이드를 제공하는 goose의 내장 기능입니다. 실습을 통해 사용자가 goose의 기능에 익숙해지도록 설계되었습니다.

Tutorial 확장 기능은 다음과 같은 대화형 학습 도구 역할을 합니다:
- 구조화된 단계별 튜토리얼 제공
- goose 기능을 사용한 실습 가능
- 즉각적인 피드백 및 가이드 제공

## 구성

1. Tutorial 확장 기능이 활성화되어 있는지 확인합니다:

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseBuiltinInstaller
    extensionName="Tutorial"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. `configure` 명령을 실행합니다:
  ```sh
  goose configure
  ```

  2. `Toggle Extensions`를 선택합니다
  ```sh
  ┌   goose-configure
  │
  ◇  What would you like to configure?
  │  Toggle Extensions
  │
  ◆  Enable extensions: (use "space" to toggle and "enter" to submit)
  // highlight-start
  │  ● tutorial
  // highlight-end
  |
  └  Extension settings updated successfully
  ```
</TabItem>
</Tabs>

## 사용 가능한 튜토리얼

현재 다음 튜토리얼을 사용할 수 있습니다:

- **build-mcp-extension**: Model Context Protocol(MCP)을 사용하여 확장 기능을 빌드하는 방법 배우기
- **first-game**: goose로 첫 게임 만들기

더 많은 기능과 사용 사례를 다루는 튜토리얼이 정기적으로 추가되고 있습니다.

## Tutorial 확장 기능 사용

### 튜토리얼 시작

튜토리얼과 상호작용하는 두 가지 주요 방법이 있습니다:

1. **goose에게 직접 물어보기**: 관심 있는 주제에 대해 간단히 물어보면 goose가 관련 튜토리얼을 제안합니다.

```
( O)> goose용 확장 기능을 빌드하는 방법을 배우고 싶어
```

2. **특정 튜토리얼 요청**: 이름으로 튜토리얼을 직접 요청할 수 있습니다.

```
( O)> first-game 튜토리얼을 안내해줄 수 있어?
```

### 튜토리얼 진행 중

튜토리얼을 진행하는 동안:

1. goose가 단계별로 프로세스를 안내합니다
2. 조치를 취하기 전에 명확한 지침을 받습니다
3. 언제든지 명확한 설명을 위해 질문할 수 있습니다
4. 휴식을 취하고 나중에 다시 시작할 수 있습니다

### 모범 사례

- **천천히 하세요**: 튜토리얼을 서두르지 마세요. 다음으로 넘어가기 전에 각 단계를 이해했는지 확인하세요.
- **질문하세요**: 명확하지 않은 것이 있으면 goose에게 더 설명해달라고 요청하세요.
- **실험하세요**: 배우고 있는 것의 변형을 자유롭게 시도하세요.
- **메모하세요**: 흥미로운 발견이나 배운 기술을 문서화하세요.

## 사용 예시

### 튜토리얼 세션 시작

확장 기능 빌드에 대해 배우기 시작하는 방법의 예시입니다:

### goose 프롬프트
```
goose용 확장 기능을 빌드하는 방법을 배우고 싶어
```

### goose 출력
```
확장 기능 빌드에 대해 배우는 것을 도와드리겠습니다! "build-mcp-extension" 튜토리얼이 이에 완벽합니다. 로드하고 안내해 드리겠습니다.

─── load_tutorial | tutorial ──────────────────────────
name: build-mcp-extension



확장 기능을 빌드하기 전에 더 잘 안내하기 위해 몇 가지를 이해해야 합니다:

1. 어떤 SDK를 선호하시나요? 세 가지 옵션이 있습니다:
   - Python (초보자 친화적, 프로토타이핑에 좋음)
   - TypeScript (웹 기반 도구에 좋음)
   - Kotlin (견고하고 타입 안전, 복잡한 애플리케이션에 좋음)

2. 어떤 유형의 확장 기능을 빌드하고 싶으신가요? 예:
   - 특정 작업을 수행하는 도구
   - LLM에 데이터를 제공하는 리소스
   - 또는 특정 아이디어가 있으신가요?

선호도를 알려주시면 적절한 설정과 구현으로 시작하는 것을 도와드리겠습니다!

참고로, 다룰 내용은 다음과 같습니다:
1. 개발 환경 설정
2. 기본 MCP 서버 생성
3. 특정 기능 구현
4. 테스트 및 디버깅

어떤 SDK를 사용하여 시작하시겠습니까?
```

## 다음 단계

튜토리얼을 완료한 후:
- 배운 개념을 자신의 프로젝트에 적용
- 경험을 공유하여 goose 커뮤니티에 기여
- 다른 사람들에게 도움이 될 새로운 튜토리얼 주제 제안

### 추가 도움이 필요하신가요?
질문이 있거나, 문제가 발생하거나, 아이디어를 브레인스토밍하고 싶으시면 [Discord 커뮤니티][discord]에 참여하세요!

[discord]: https://discord.gg/goose-oss
