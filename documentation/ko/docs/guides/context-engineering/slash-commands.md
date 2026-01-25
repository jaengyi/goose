---
sidebar_position: 4
title: 사용자 정의 슬래시 명령
sidebar_title: 슬래시 명령
description: "모든 goose 채팅 세션에서 재사용 가능한 지침을 빠르게 적용하는 사용자 정의 단축키 만들기"
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Terminal } from 'lucide-react';

사용자 정의 슬래시 명령은 [레시피](/docs/guides/recipes)를 실행하는 개인화된 단축키입니다. 일일 보고서를 실행하는 레시피가 있다면 세션 내에서 해당 레시피를 호출하는 사용자 정의 슬래시 명령을 만들 수 있습니다:

```
/daily-report
```


## 슬래시 명령 만들기

레시피에 사용자 정의 명령을 할당합니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
   1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
   2. 사이드바에서 `Recipes`를 클릭합니다
   3. 사용하려는 레시피를 찾고 <Terminal className="inline" size={16} /> 버튼을 클릭합니다
   4. 팝업되는 모달에서 사용자 정의 명령을 입력합니다 (앞의 `/` 없이)
   5. `Save`를 클릭합니다

  명령은 `Recipes` 메뉴의 레시피 아래에 나타납니다. 레시피 라이브러리에 없는 레시피의 경우 `goose CLI` 단계를 따르세요.

  </TabItem>
  <TabItem value="cli" label="goose CLI">

  [구성 파일](/docs/guides/config-files)에서 슬래시 명령을 구성합니다. 명령(앞의 `/` 없이)과 함께 컴퓨터의 레시피 파일 경로를 나열합니다:

```yaml title="~/.config/goose/config.yaml"
slash_commands:
  - command: "run-tests"
    recipe_path: "/path/to/recipe.yaml"
  - command: "daily-report"
    recipe_path: "/Users/me/.local/share/goose/recipes/report.yaml"
```

   </TabItem>
</Tabs>

## 슬래시 명령 사용

모든 채팅 세션에서 메시지 시작 부분에 앞에 슬래시가 있는 사용자 정의 명령을 입력합니다:

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

```
/run-tests
```

:::tip 사용 가능한 명령
goose 데스크톱에서 `/`를 입력하면 사용 가능한 슬래시 명령이 있는 팝업 메뉴가 표시됩니다.
:::

  </TabItem>
  <TabItem value="cli" label="goose CLI">

```sh
Context: ●○○○○○○○○○ 5% (9695/200000 tokens)
( O)> /run-tests
```

  </TabItem>
</Tabs>

명령 뒤에 하나의 매개변수를 전달할 수 있습니다 (필요한 경우). 따옴표는 선택사항입니다:

```
/translator where is the library
```

슬래시 명령을 사용하여 레시피를 실행하면 레시피의 지침과 프롬프트 필드가 모델로 전송되고 대화에 로드되지만 채팅에 표시되지는 않습니다. 모델은 직접 열었을 때와 마찬가지로 레시피의 컨텍스트와 지침을 사용하여 응답합니다.

## 제한 사항

- 슬래시 명령은 하나의 [매개변수](/docs/guides/recipes/recipe-reference#parameters)만 허용합니다. 레시피의 추가 매개변수에는 기본값이 있어야 합니다.
- 명령 이름은 대소문자를 구분하지 않습니다 (`/Bug`와 `/bug`는 같은 명령으로 처리됩니다).
- 명령 이름은 고유해야 하며 공백을 포함할 수 없습니다.
- `/recipe`, `/compact` 또는 `/help`와 같은 [내장 CLI 슬래시 명령](/docs/guides/goose-cli-commands#slash-commands)과 충돌하는 이름은 사용할 수 없습니다.
- 레시피 파일이 없거나 유효하지 않으면 명령은 모델로 전송되는 일반 텍스트로 처리됩니다.

## 추가 리소스

import ContentCardCarousel from '@site/src/components/ContentCardCarousel';

<ContentCardCarousel
  items={[
    {
      type: 'topic',
      title: '레시피',
      description: 'goose 레시피를 마스터하는 데 도움이 되는 더 많은 문서, 도구 및 리소스는 레시피 가이드를 확인하세요.',
      linkUrl: '/goose/docs/guides/recipes'
    },
    {
      type: 'topic',
      title: '연구 → 계획 → 구현 패턴',
      description: '슬래시 명령이 대화형 RPI 워크플로우에 지침을 쉽게 통합하는 방법을 확인하세요.',
      linkUrl: '/goose/docs/tutorials/rpi'
    }
  ]}
/>
