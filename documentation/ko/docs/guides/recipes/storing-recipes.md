---
title: 레시피 저장
sidebar_position: 4
sidebar_label: 레시피 저장
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, ChefHat } from 'lucide-react';

이 가이드에서는 나중에 다시 액세스해야 할 때를 위해 goose 레시피를 저장, 정리 및 찾는 방법을 다룹니다.

:::info 데스크톱 UI vs CLI
- **goose 데스크톱**은 저장된 레시피를 탐색하고 관리하기 위한 시각적 레시피 라이브러리가 있습니다
- **goose CLI**는 레시피를 파일로 저장하고 파일 경로나 환경 변수를 사용하여 찾습니다
:::

## 레시피 저장소 이해

레시피를 저장하기 전에 어디에 저장할 수 있는지와 이것이 가용성에 어떤 영향을 미치는지 이해하는 것이 중요합니다.

### 레시피 저장 위치

| 유형 | 위치 | 가용성 | 적합한 용도 |
|------|----------|-------------|----------|
| **전역** | `~/.config/goose/recipes/` | 모든 프로젝트와 세션 | 개인 워크플로우, 범용 레시피 |
| **로컬** | `YOUR_WORKING_DIRECTORY/.goose/recipes/` | 해당 프로젝트에서 작업할 때만 | 프로젝트별 워크플로우, 팀 레시피 |

**전역 저장소를 선택해야 할 때:**
- 모든 프로젝트에서 레시피를 사용하고 싶을 때
- 개인 워크플로우 또는 범용 레시피일 때
- 레시피의 주 사용자가 본인일 때

**로컬 저장소를 선택해야 할 때:**
- 레시피가 특정 프로젝트에 한정될 때
- 팀과 함께 작업하고 레시피를 공유하고 싶을 때
- 레시피가 프로젝트별 파일이나 구성에 의존할 때


## 레시피 저장

<Tabs groupId="interface">
  <TabItem value="desktop" label="goose 데스크톱" default>

**새 레시피 저장:**

1. 채팅 세션에서 레시피를 만들려면 [레시피 만들기](/docs/guides/recipes/session-recipes#create-recipe)를 참조하세요
2. 레시피 편집기에서 `Save Recipe`를 클릭하여 레시피 라이브러리에 저장합니다

**수정된 레시피 저장:**

이미 레시피를 사용 중이고 수정된 버전을 저장하려면:
1. 첫 번째 메시지를 보낸 후 나타나는 앱 하단의 <ChefHat className="inline" size={16}/> 버튼을 클릭합니다
2. 지침, 프롬프트 또는 기타 필드를 원하는 대로 편집합니다
3. `Save Recipe`를 클릭합니다

:::info
레시피를 수정하고 새 이름으로 저장하면 새 레시피와 새 링크가 생성됩니다. 레시피 라이브러리에서 원래 레시피를 계속 실행하거나 원래 링크를 사용할 수 있습니다. 이름을 변경하지 않고 레시피를 편집하면 레시피 라이브러리의 버전이 업데이트되지만 링크를 통해 원래 레시피를 계속 실행할 수 있습니다.
:::

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    [레시피를 만들면](/docs/guides/recipes/recipe-reference) 다음 위치에 저장됩니다:

    * 기본적으로 작업 디렉토리: `./recipe.yaml`
    * 지정하는 경로: `/recipe /path/to/my-recipe.yaml`
    * 로컬 프로젝트 레시피: `/recipe .goose/recipes/my-recipe.yaml`

    :::note
    CLI는 레시피를 `.yaml` 파일로 저장합니다. CLI는 `.json` 형식의 레시피를 실행할 수 있지만 JSON으로 레시피를 저장하는 옵션은 제공하지 않습니다.
    :::

  </TabItem>
</Tabs>

### 레시피 가져오기

<Tabs groupId="interface">
  <TabItem value="desktop" label="goose 데스크톱" default>
    딥링크 또는 레시피 파일을 사용하여 레시피를 가져옵니다:

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Recipes`를 클릭합니다
    3. `Import Recipe`를 클릭합니다
    4. 가져오기 방법을 선택합니다:
       - 링크로 가져오려면: `Recipe Deeplink`에서 [레시피 링크](/docs/guides/recipes/session-recipes#share-via-recipe-link)를 붙여넣습니다
       - 파일로 가져오려면: `Recipe File`에서 `Choose File`을 클릭하고 레시피 파일을 선택한 다음 `Open`을 클릭합니다
    5. `Import Recipe`를 클릭하여 레시피 라이브러리에 레시피 복사본을 저장합니다

  :::warning 레시피 파일 형식
  goose 데스크톱은 `.yaml`, `.yml` 및 `.json` 파일을 허용하지만 **CLI는 `.yaml` 및 `.json`만 지원**합니다. 두 인터페이스 모두에서 완전한 호환성을 위해 `.yml` 확장자는 피하세요.

  모든 레시피 형식은 동일한 [스키마 구조](/docs/guides/recipes/recipe-reference#core-recipe-schema)를 따릅니다.
  :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    레시피 가져오기는 goose 데스크톱에서만 사용할 수 있습니다.
  </TabItem>
</Tabs>

## 사용 가능한 레시피 찾기

<Tabs groupId="interface">
  <TabItem value="desktop" label="goose 데스크톱" default>

**레시피 라이브러리 액세스:**
1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
2. `Recipes`를 클릭하여 레시피 라이브러리를 봅니다
3. 다음을 보여주는 사용 가능한 레시피를 탐색합니다:
   - 레시피 제목 및 설명
   - 마지막 수정 날짜
   - 전역 또는 로컬 저장 여부

:::info 데스크톱 vs CLI 레시피 검색
데스크톱 레시피 라이브러리는 명시적으로 저장하거나 가져온 모든 레시피를 표시합니다. CLI처럼 파일 시스템에서 레시피 파일을 자동으로 검색하지 않습니다.
:::

  </TabItem>
  <TabItem value="cli" label="goose CLI">

`goose recipe list` 명령을 사용하여 여러 소스에서 사용 가능한 모든 레시피를 찾습니다:

**기본 사용법**

```bash
# 사용 가능한 모든 레시피 나열
goose recipe list

# 제목과 전체 경로를 포함한 상세 정보 표시
goose recipe list --verbose

# 자동화를 위해 JSON 형식으로 출력
goose recipe list --format json
```

**레시피 검색 프로세스**

goose는 다음 위치에서 레시피를 순서대로 검색합니다:

1. **현재 디렉토리**: `.` (`*.yaml` 및 `*.json` 파일 검색)
2. **사용자 정의 경로**: [`GOOSE_RECIPE_PATH`](/docs/guides/environment-variables#recipe-configuration) 환경 변수에 지정된 디렉토리
3. **전역 레시피 라이브러리**: `~/.config/goose/recipes/` (또는 OS에 해당하는 경로)
4. **로컬 프로젝트 레시피**: `./.goose/recipes/`
5. **GitHub 저장소**: [`GOOSE_RECIPE_GITHUB_REPO`](/docs/guides/environment-variables#recipe-configuration) 환경 변수가 구성된 경우

**출력 예시**

*기본 텍스트 형식:*
```bash
$ goose recipe list
Available recipes:
goose-self-test - A comprehensive meta-testing recipe - local: ./goose-self-test.yaml
hello-world - A sample recipe demonstrating basic usage - local: ~/.config/goose/recipes/hello-world.yaml
job-finder - Find software engineering positions - local: ~/.config/goose/recipes/job-finder.yaml
```

*상세 모드:*
```bash
$ goose recipe list --verbose
Available recipes:
  goose-self-test - A comprehensive meta-testing recipe - local: ./goose-self-test.yaml
    Title: goose Self-Testing Integration Suite
    Path: ./goose-self-test.yaml
  hello-world - A sample recipe demonstrating basic usage - local: ~/.config/goose/recipes/hello-world.yaml
    Title: Hello World Recipe
    Path: /Users/username/.config/goose/recipes/hello-world.yaml
```

*자동화를 위한 JSON 형식:*
```json
[
  {
    "name": "goose-self-test",
    "source": "Local",
    "path": "./goose-self-test.yaml",
    "title": "goose Self-Testing Integration Suite",
    "description": "A comprehensive meta-testing recipe"
  },
  {
    "name": "hello-world",
    "source": "GitHub",
    "path": "recipes/hello-world.yaml",
    "title": "Hello World Recipe",
    "description": "A sample recipe demonstrating basic usage"
  }
]
```

**레시피 소스 구성**

사용자 정의 레시피 디렉토리 추가:
```bash
export GOOSE_RECIPE_PATH="/path/to/my/recipes:/path/to/team/recipes"
goose recipe list
```

GitHub 레시피 저장소 구성:
```bash
export GOOSE_RECIPE_GITHUB_REPO="myorg/goose-recipes"
goose recipe list
```

더 많은 구성 옵션은 [환경 변수 가이드](/docs/guides/environment-variables#recipe-configuration)를 참조하세요.

**수동 디렉토리 탐색 (고급)**

레시피 디렉토리를 수동으로 탐색해야 하는 경우:

```bash
# 기본 전역 위치의 레시피 나열
ls ~/.config/goose/recipes/

# 현재 프로젝트의 레시피 나열
ls .goose/recipes/

# 모든 레시피 파일 검색
find . -name "*.yaml" -path "*/recipes/*" -o -name "*.json" -path "*/recipes/*"
```

:::tip
`goose recipe list` 명령은 모든 구성된 소스를 자동으로 검색하고 일관된 형식을 제공하므로 레시피를 찾는 데 권장되는 방법입니다.
:::

  </TabItem>
</Tabs>

## 저장된 레시피 사용

<Tabs groupId="interface">
  <TabItem value="desktop" label="goose 데스크톱" default>

1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
2. `Recipes`를 클릭합니다
3. 레시피 라이브러리에서 레시피를 찾습니다
4. 다음 중 하나를 선택합니다:
   - `Use`를 클릭하여 즉시 실행
   - `Preview`를 클릭하여 먼저 레시피 세부 정보를 본 다음 **Load Recipe**를 클릭하여 실행

  </TabItem>
  <TabItem value="cli" label="goose CLI">

레시피 파일을 찾은 후 [레시피 실행](/docs/guides/recipes/session-recipes#run-a-recipe) 또는 [goose 데스크톱에서 열기](/docs/guides/goose-cli-commands#recipe).

:::tip 형식 호환성
CLI는 goose 데스크톱에서 저장한 레시피를 변환 없이 실행할 수 있습니다. CLI에서 만든 레시피와 데스크톱에서 저장한 레시피 모두 모든 레시피 명령에서 작동합니다.
:::

  </TabItem>
</Tabs>
