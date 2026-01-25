---
sidebar_position: 1
title: 공유 가능한 레시피
description: "다른 사람이 한 번의 클릭으로 실행할 수 있는 재사용 가능한 레시피로 goose 세션 설정 (도구, 목표 및 지침 포함) 공유"
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, ChefHat, SquarePen, Link, Clock, Terminal, Share2 } from 'lucide-react';
import RecipeFields from '@site/src/components/RecipeFields';

때때로 goose에서 작업을 마치고 "이 설정은 다시 유용할 수 있겠다"고 깨닫게 됩니다. 도구의 훌륭한 조합을 선별하고, 명확한 목표를 정의하고, 그 흐름을 보존하고 싶을 수 있습니다. 또는 다른 사람이 단계별로 안내하지 않고도 방금 한 것을 복제할 수 있도록 돕고 싶을 수 있습니다.

현재 goose 세션을 현재 사용 중인 도구, 목표 및 설정을 포함하는 재사용 가능한 레시피로 변환하고 다른 사람(또는 미래의 자신)이 한 번의 클릭으로 실행할 수 있는 새 에이전트로 패키징할 수 있습니다.

## 레시피 만들기

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  현재 세션 또는 템플릿에서 레시피를 만듭니다.

  <Tabs>
    <TabItem value="session" label="현재 세션" default>
      1. 레시피로 저장하려는 세션에서 앱 하단의 <ChefHat className="inline" size={16} /> 버튼을 클릭합니다
      2. 열리는 대화 상자에서 필요에 따라 레시피 필드를 검토하고 편집합니다:
         <RecipeFields />
      3. 완료되면 다음을 수행할 수 있습니다:
         - `Create Recipe`를 클릭하여 레시피 라이브러리에 레시피를 저장합니다
         - `Create & Run Recipe`를 클릭하여 저장하고 새 세션에서 즉시 레시피를 실행합니다
    </TabItem>
    <TabItem value="new" label="템플릿">
      1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
      2. 사이드바에서 `Recipes`를 클릭합니다
      3. `Create Recipe`를 클릭합니다
      4. 열리는 대화 상자에서 필요에 따라 레시피 필드를 입력합니다:
         <RecipeFields />
      5. 완료되면 다음을 수행할 수 있습니다:
         - 레시피 링크를 복사하여 다른 사람과 레시피를 공유합니다
         - `Save Recipe`를 클릭하여 레시피 라이브러리에 레시피를 저장합니다
         - `Save & Run Recipe`를 클릭하여 저장하고 새 세션에서 즉시 레시피를 실행합니다
    </TabItem>
  </Tabs>

   :::warning
   기존 레시피 세션에서 레시피를 만들 수 없지만 레시피를 보거나 [편집](#레시피-편집)할 수 있습니다.
   :::

  </TabItem>

  <TabItem value="cli" label="goose CLI">
   레시피 파일은 JSON (.json) 또는 YAML (.yaml) 파일일 수 있습니다. [세션](/docs/guides/sessions/session-management#start-session) 중에 이 명령을 실행하여 현재 디렉토리에 recipe.yaml 파일을 생성합니다:

   ```sh
   /recipe
   ```

   다른 이름을 지정하려면 인수로 제공할 수 있습니다:

   ```sh
   /recipe my-custom-recipe.yaml
   ```

   <details>
   <summary>레시피 파일 구조</summary>

   ```yaml
   # 필수 필드
   version: 1.0.0
   title: $title
   description: $description
   instructions: $instructions    # 모델의 동작 정의

   # 선택 필드
   prompt: $prompt                # 시작할 초기 메시지
   extensions:                    # 레시피에 필요한 도구
   - $extensions
   activities:                    # 데스크톱 앱에 표시할 예시 프롬프트
   - $activities
   settings:                      # 추가 설정
     goose_provider: $provider    # 이 레시피에 사용할 프로바이더
     goose_model: $model          # 이 레시피에 사용할 특정 모델
     temperature: $temperature    # 이 레시피의 모델 온도 설정 (0.0에서 1.0)
   retry:                         # 성공 검증을 통한 자동 재시도 로직
     max_retries: $max_retries    # 최대 재시도 횟수
     checks:                      # 성공 검증 검사
     - type: shell
       command: $validation_command
     on_failure: $cleanup_command # 실패 시 선택적 정리 명령
   ```
   </details>

    모든 레시피 필드에 대한 자세한 설명과 예시 구성은 [레시피 참조 가이드](/docs/guides/recipes/recipe-reference)를 참조하세요.

   :::warning
   기존 레시피 세션에서 레시피를 만들 수 없습니다 - `/recipe` 명령이 작동하지 않습니다.
   :::

   :::tip 레시피 검증
   레시피가 완전하고 올바른 형식인지 확인하려면 [레시피를 검증](#레시피-검증)해야 합니다.
   :::

   #### 선택적 매개변수

   레시피에 매개변수를 추가할 수 있으며, 이는 레시피를 실행할 때 사용자가 데이터를 입력해야 합니다. 매개변수는 레시피의 모든 부분(지침, 프롬프트, 활동 등)에 추가할 수 있습니다.

   매개변수를 사용하려면:
   1. 레시피 내용에서 `{{ variable_name }}` 구문을 사용하여 템플릿 변수를 추가합니다
   2. YAML 파일의 `parameters` 섹션에서 각 매개변수를 정의합니다

   <details>
   <summary>매개변수가 있는 레시피 예시</summary>

   ```yaml
   version: 1.0.0
   title: "{{ project_name }} Code Review" # YAML 파싱 오류를 방지하려면 템플릿 구문으로 시작하는 값을 따옴표로 묶습니다
   description: Automated code review for {{ project_name }} with {{ language }} focus
   instructions: You are a code reviewer specialized in {{ language }} development.
   prompt: |
      Apply the following standards:
      - Complexity threshold: {{ complexity_threshold }}
      - Required test coverage: {{ test_coverage }}%
      - Style guide: {{ style_guide }}
   activities:
   - "Review {{ language }} code for complexity"
   - "Check test coverage against {{ test_coverage }}% requirement"
   - "Verify {{ style_guide }} compliance"
   settings:
     goose_provider: "anthropic"
     goose_model: "claude-3-7-sonnet-latest"
     temperature: 0.7
   parameters:
   - key: project_name
     input_type: string
     requirement: required # required, optional 또는 user_prompt가 될 수 있음
     description: name of the project
   - key: language
     input_type: string
     requirement: required
     description: language of the code
   - key: complexity_threshold
     input_type: number
     requirement: optional
     default: 20 # optional 매개변수에는 기본값이 필수
     description: a threshold that defines the maximum allowed complexity
   - key: test_coverage
     input_type: number
     requirement: optional
     default: 80
     description: the minimum test coverage threshold in percentage
   - key: style_guide
     input_type: string
     description: style guide name
     requirement: user_prompt
     # style_guide 매개변수 값이 명령에 지정되지 않은 경우 비대화형 모드에서도 사용자에게 값을 제공하라는 메시지가 표시됨
   ```
   </details>

   레시피 필드에 대한 자세한 내용은 [레시피 참조 가이드](/docs/guides/recipes/recipe-reference)를 참조하세요.

   </TabItem>


  <TabItem value="generator" label="레시피 생성기">
    온라인 [레시피 생성기](https://block.github.io/goose/recipe-generator) 도구를 사용하여 레시피를 만들 수 있습니다. 먼저 원하는 형식을 선택합니다:

    - **URL 형식**: goose 데스크톱에서 세션을 여는 공유 가능한 링크를 생성합니다
    - **YAML 형식**: 파일로 저장한 다음 goose CLI에서 실행할 수 있는 YAML 콘텐츠를 생성합니다

    그런 다음 레시피 양식을 작성합니다:
      - 레시피의 **제목**
      - **설명**
      - 레시피에 대한 **지침** 세트.
      - 선택적 초기 **프롬프트**:
        - 데스크톱에서는 프롬프트가 채팅 상자에 표시됩니다.
        - CLI에서는 프롬프트가 실행할 초기 메시지를 제공합니다. 헤드리스(비대화형) 모드에서 레시피를 실행하려면 프롬프트가 필요합니다.
      - 데스크톱에 표시할 선택적 **활동** 세트.
      - YAML 형식만: 선택적 **작성자** 연락처 정보 및 레시피가 사용하는 **확장 기능**.

  </TabItem>
</Tabs>

## 레시피 편집
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

   1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
   2. 사이드바에서 `Recipes`를 클릭합니다
   3. 편집하려는 레시피를 찾아 <SquarePen className="inline" size={16} /> 버튼을 클릭합니다
   4. 나타나는 대화 상자에서 다음 중 하나를 편집합니다:
      <RecipeFields />
   5. 완료되면 다음을 수행할 수 있습니다:
      - 레시피 링크를 복사하여 다른 사람과 레시피를 공유합니다
      - `Save Recipe`를 클릭하여 변경 사항을 저장합니다
      - `Save & Run Recipe`를 클릭하여 저장하고 새 세션에서 즉시 레시피를 실행합니다

  :::tip 사용 중인 레시피 편집
  세션에서 레시피를 사용하는 동안 편집 대화 상자에 액세스할 수도 있습니다: 앱 하단의 <ChefHat className="inline" size={16} /> 버튼을 클릭하기만 하면 됩니다. 이 버튼은 첫 번째 메시지를 보낸 후에 나타납니다.
  :::

  </TabItem>

  <TabItem value="cli" label="goose CLI">
  레시피 파일이 생성되면 원하는 텍스트 편집기로 열어 모든 필드의 값을 수정할 수 있습니다.

</TabItem>
</Tabs>

## 레시피 사용

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  1. 직접 링크, 수동 URL 입력 또는 레시피 라이브러리에서 레시피를 엽니다:

     **직접 링크:**

         1. 공유된 레시피 링크를 클릭합니다

     **수동 URL 입력:**

         1. 브라우저의 주소 표시줄에 레시피 링크를 붙여넣습니다
         2. `Enter`를 누르고 `Open Goose.app` 프롬프트를 클릭합니다

     **레시피 라이브러리:**

         1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
         2. 사이드바에서 `Recipes`를 클릭합니다
         3. 레시피 라이브러리에서 레시피를 찾습니다
         4. 열려는 레시피 옆의 `Use`를 클릭합니다

     **슬래시 명령:**

         1. goose 채팅 세션에서 [사용자 정의 슬래시 명령](/docs/guides/context-engineering/slash-commands)을 입력합니다

  2. 레시피를 처음 실행할 때 레시피의 제목, 설명 및 지침을 표시하는 경고 대화 상자가 표시됩니다. 레시피 내용을 신뢰하면 `Trust and Execute`를 클릭하여 계속합니다. 레시피가 변경되지 않는 한 동일한 레시피에 대해 다시 메시지가 표시되지 않습니다.

  3. 레시피에 매개변수가 포함된 경우 `Recipe Parameters` 대화 상자에 값을 입력하고 `Start Recipe`를 클릭합니다.

     매개변수는 레시피에서 사용되는 동적 값입니다:

     - **필수 매개변수**는 빨간색 별표(*)로 표시됩니다
     - **선택적 매개변수**는 변경할 수 있는 기본값을 표시합니다

  4. 레시피를 실행하려면 활동 버블을 클릭하거나 프롬프트를 전송합니다.

  :::info 개인정보 보호 및 격리
  - 각 사용자는 자신만의 개인 세션을 받습니다
  - 사용자 간에 데이터가 공유되지 않습니다
  - 세션은 원래 레시피 작성자의 세션에 영향을 미치지 않습니다
  :::
  </TabItem>

  <TabItem value="cli" label="goose CLI">

  goose CLI로 레시피를 사용하면 다음 작업이 포함될 수 있습니다:
  - [레시피 위치 구성](#레시피-위치-구성)
  - [레시피 실행](#레시피-실행)
  - [레시피 예약](#레시피-예약)

   #### 레시피 위치 구성

  레시피는 장치에 로컬로 저장하거나 GitHub 리포지토리에 저장할 수 있습니다. `goose configure` 명령 또는 [구성 파일](/docs/guides/config-files#global-settings)을 사용하여 레시피 리포지토리를 구성합니다.

  :::tip 리포지토리 구조
  - 각 레시피는 자체 디렉토리에 있어야 합니다
  - 디렉토리 이름은 명령에서 사용하는 레시피 이름과 일치합니다
  - 레시피 파일은 recipe.yaml 또는 recipe.json일 수 있습니다
  :::

   <Tabs>
     <TabItem value="configure" label="goose configure 사용" default>

       configure 명령을 실행합니다:
       ```sh
       goose configure
       ```

       다음 프롬프트가 표시됩니다:

       ```sh
       ┌  goose-configure
       │
       ◆  What would you like to configure?
       │  ○ Configure Providers
       │  ○ Add Extension
       │  ○ Toggle Extensions
       │  ○ Remove Extension
       // highlight-start
       │  ● goose settings (Set the goose mode, Tool Output, Tool Permissions, Experiment, goose recipe github repo and more)
       // highlight-end
       │
       ◇  What would you like to configure?
       │  goose settings
       │
       ◆  What setting would you like to configure?
       │  ○ goose mode
       │  ○ Tool Permission
       │  ○ Tool Output
       │  ○ Toggle Experiment
       // highlight-start
       │  ● goose recipe github repo (goose will pull recipes from this repo if not found locally.)
       // highlight-end
       └
       ┌  goose-configure
       │
       ◇  What would you like to configure?
       │  goose settings
       │
       ◇  What setting would you like to configure?
       │  goose recipe github repo
       │
       ◆  Enter your goose recipe GitHub repo (owner/repo): eg: my_org/goose-recipes
       // highlight-start
       │  squareup/goose-recipes (default)
       // highlight-end
       └
       ```

     </TabItem>

     <TabItem value="config" label="구성 파일 사용">

       구성 파일에 추가합니다:
       ```yaml title="~/.config/goose/config.yaml"
       GOOSE_RECIPE_GITHUB_REPO: "owner/repo"
       ```

     </TabItem>
   </Tabs>

   #### 레시피 실행

   <Tabs groupId="interface">
     <TabItem value="local" label="로컬 레시피" default>

       **기본 사용법** - 한 번 실행하고 종료 (자세한 내용은 [실행 옵션](/docs/guides/goose-cli-commands#run-options) 및 [레시피 명령](/docs/guides/goose-cli-commands#recipe) 참조):
       ```sh
       # 현재 디렉토리 또는 [`GOOSE_RECIPE_PATH`](/docs/guides/environment-variables#recipe-configuration) 디렉토리의 레시피 파일 사용
       goose run --recipe recipe.yaml

       # 전체 경로 사용
       goose run --recipe ./recipes/my-recipe.yaml
       ```

       **레시피 미리보기** - 실행 전 세부 정보를 보려면 [`explain`](/docs/guides/goose-cli-commands#run-options) 명령을 사용합니다:

       **대화형 모드** - 대화형 세션을 시작합니다:
       ```sh
       goose run --recipe recipe.yaml --interactive
       ```
       대화형 모드는 필수 값을 프롬프트합니다:
       ```sh
       ◆ Enter value for required parameter 'language':
       │ Python
       │
       ◆ Enter value for required parameter 'style_guide':
       │ PEP8
       ```

       **매개변수 사용** - 레시피를 실행할 때 매개변수 값을 제공합니다. 자세한 예시와 옵션은 [`run` 명령 문서](/docs/guides/goose-cli-commands#run-options)를 참조하세요.

       기본 예시:
       ```sh
       goose run --recipe recipe.yaml --params language=Python
       ```

       **슬래시 명령** - goose 채팅 세션에서 [사용자 정의 슬래시 명령](/docs/guides/context-engineering/slash-commands)을 입력합니다

     </TabItem>

     <TabItem value="github" label="GitHub 레시피">

       GitHub 리포지토리를 구성한 후 이름으로 레시피를 실행할 수 있습니다:

       **기본 사용법** - 디렉토리와 일치하는 레시피 이름을 사용하여 구성된 리포지토리에서 레시피를 실행합니다 (자세한 내용은 [실행 옵션](/docs/guides/goose-cli-commands#run-options) 및 [레시피 명령](/docs/guides/goose-cli-commands#recipe) 참조):

       ```sh
       goose run --recipe recipe-name
       ```

       예를 들어, 리포지토리 구조가 다음과 같은 경우:
       ```
       my-repo/
       ├── code-review/
       │   └── recipe.yaml
       └── setup-project/
           └── recipe.yaml
       ```

       코드 리뷰 레시피를 실행하려면 다음 명령을 실행합니다:
       ```sh
       goose run --recipe code-review
       ```

      **레시피 미리보기** - 실행 전 세부 정보를 보려면 [`explain`](/docs/guides/goose-cli-commands#run-options) 명령을 사용합니다:

       **대화형 모드** - 매개변수 프롬프트 사용:
       ```sh
       goose run --recipe code-review --interactive
       ```
       대화형 모드는 필수 값을 프롬프트합니다:
       ```sh
       ◆ Enter value for required parameter 'project_name':
       │ MyProject
       │
       ◆ Enter value for required parameter 'language':
       │ Python
       ```

       **매개변수 사용** - 레시피를 실행할 때 매개변수 값을 제공합니다. 자세한 예시와 옵션은 [`run` 명령 문서](/docs/guides/goose-cli-commands#run-options)를 참조하세요.

     </TabItem>
   </Tabs>
  :::info 개인정보 보호, 격리 및 비밀
  - 각 사용자는 자신만의 개인 세션을 받습니다
  - 사용자 간에 데이터가 공유되지 않습니다
  - 세션은 원래 레시피 작성자의 세션에 영향을 미치지 않습니다
  - CLI는 필요한 [확장 기능 비밀](/docs/guides/recipes/recipe-reference#extension-secrets)에 대해 사용자에게 프롬프트할 수 있습니다
  :::

   </TabItem>
</Tabs>

## 레시피 검증

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    레시피 검증은 CLI를 통해서만 사용할 수 있습니다.
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    레시피 파일이 올바르게 구성되었는지 검증합니다. 검증은 다음을 확인합니다:
    - 모든 필수 필드가 있는지
    - 매개변수가 올바른 형식인지
    - 참조된 확장 기능이 존재하고 유효한지
    - YAML/JSON 구문이 올바른지

   ```sh
   goose recipe validate recipe.yaml
   ```

   :::info
   방금 만든 레시피를 검증하려면 [`validate` 하위 명령](/docs/guides/goose-cli-commands#recipe)을 실행하기 전에 [세션을 종료](/docs/guides/sessions/session-management#exit-session)해야 합니다.
   :::

   레시피 검증은 다음에 유용할 수 있습니다:
    - 예상대로 작동하지 않는 레시피 문제 해결
    - 수동 편집 후 레시피 확인
    - CI/CD 파이프라인에서 자동화된 테스트

  </TabItem>
</Tabs>

## 레시피 공유
레시피 링크 또는 레시피 파일을 사용하여 goose 사용자와 레시피를 공유합니다.

:::info 개인정보 보호 및 격리
공유된 레시피를 사용할 때 각 수신자는 자신만의 개인 세션을 받습니다. 사용자 간에 데이터가 공유되지 않으며 원본 세션과 레시피는 영향을 받지 않습니다.
:::

### 레시피 링크를 통한 공유
레시피 링크를 통해 데스크톱 사용자와 레시피를 공유할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    레시피 라이브러리에서 딥링크를 복사하여 다른 사람과 공유합니다:
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Recipes`를 클릭합니다
    3. 공유하려는 레시피를 찾아 <Link className="inline" size={16} /> 버튼을 클릭하여 링크를 복사합니다

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    레시피 파일에서 딥링크를 생성하여 다른 사람과 공유합니다:
    ```sh
    goose recipe deeplink <FILE>
    ```

    `Recipe Parameters` 대화 상자를 미리 채우기 위해 매개변수 값을 제공할 수도 있습니다:
    ```sh
    goose recipe deeplink <FILE> --param key1=value1 --param key2=value2
    ```
  </TabItem>
</Tabs>

누군가 링크를 클릭하면 레시피 구성으로 goose 데스크톱이 열립니다. 레시피 링크를 사용하여 향후 사용을 위해 [레시피를 가져올](/docs/guides/recipes/storing-recipes#importing-recipes) 수도 있습니다.

### 레시피 파일을 통한 공유
레시피 파일을 직접 전송하여 데스크톱 또는 CLI 사용자와 레시피를 공유할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

  goose 데스크톱에서 레시피 파일을 내보내거나 내용을 복사하여 다른 사람과 공유할 수 있습니다.

  1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
  2. 사이드바에서 `Recipes`를 클릭합니다
  3. 공유하려는 레시피를 찾아 <Share2 className="inline" size={16} /> 버튼을 클릭합니다
  4. 공유 방법을 선택합니다:
     - 레시피를 `.yaml` 파일로 다운로드하려면: `Export to File`을 선택하고 다운로드 위치를 선택한 다음 `Save`를 클릭합니다
     - 레시피의 YAML 내용을 클립보드에 복사하려면: `Copy YAML`을 선택합니다

  다른 데스크톱 사용자는 레시피 라이브러리로 [레시피를 가져올](/docs/guides/recipes/storing-recipes#importing-recipes) 수 있습니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">

  레시피 내용 내보내기 또는 복사는 데스크톱에서만 가능하지만 로컬 레시피 파일을 직접 복사할 수 있습니다.

  CLI 사용자는 `goose run --recipe <FILE>`을 사용하여 공유된 레시피 파일을 실행하거나 `goose recipe open <FILE>`로 goose 데스크톱에서 직접 열 수 있습니다. 자세한 내용은 [CLI 명령 가이드](/docs/guides/goose-cli-commands#recipe)를 참조하세요.

  </TabItem>
</Tabs>

## 레시피 예약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
예약에 따라 goose 레시피를 자동으로 실행합니다. 예약을 만들 때 다음을 구성합니다:
- **이름**: 예약에 대한 설명적인 이름
- **소스**: 실행할 레시피
- **실행 모드**: 레시피가 백그라운드에서 실행되는지 (창 없음, 결과 저장) 또는 포그라운드에서 실행되는지 (goose 데스크톱이 실행 중이면 창 열림, 그렇지 않으면 백그라운드에서 실행)
- **빈도 및 시간**: 레시피를 실행할 시기 (예: 20분마다, 매주 금요일 오전 10시). 선택 항목은 goose에서 사용하는 [cron 표현식](https://en.wikipedia.org/wiki/Cron#Cron_expression)으로 변환됩니다.

**레시피 라이브러리에서 예약:**

   1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
   2. 사이드바에서 `Recipes`를 클릭합니다
   3. 예약하려는 레시피를 찾아 <Clock className="inline" size={16} /> 버튼을 클릭합니다
   4. `Create Schedule`을 클릭합니다
   5. 나타나는 대화 상자에서 예약을 구성합니다. **Source**의 경우 레시피 링크가 이미 제공됩니다.
   6. `Create Schedule`을 클릭합니다

**스케줄러 뷰에서 예약:**

   1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
   2. `Scheduler`를 클릭합니다
   3. `Create Schedule`을 클릭합니다
   4. 나타나는 대화 상자에서 예약을 구성합니다. **Source**의 경우 `.yaml` 또는 `.yml` 파일을 선택하거나 [레시피 링크](#레시피-공유)를 제공합니다.
   5. `Create Schedule`을 클릭합니다

**예약된 레시피 관리**

예약된 레시피는 `Scheduler` 페이지에 나열됩니다.
예약을 클릭하여 세부 정보를 보고, 마지막으로 실행된 시간을 확인하고, 예약된 레시피로 작업을 수행합니다:
- `Run Schedule Now`를 클릭하여 레시피를 수동으로 트리거합니다
- `Edit Schedule`을 클릭하여 예약된 빈도를 변경합니다
- `Pause Schedule`을 클릭하여 레시피가 자동으로 실행되는 것을 중지합니다

`Schedule Details` 페이지 하단에서 예약된 레시피로 생성된 세션 목록을 보고 각 세션을 열거나 복원할 수 있습니다.

  </TabItem>
  <TabItem value="cli" label="goose CLI">
  [cron 표현식](https://en.wikipedia.org/wiki/Cron#Cron_expression)을 사용하여 goose 레시피를 예약하여 실행합니다.

  ```bash
  # 매일 오전 9시에 실행되는 새 예약된 레시피 추가
  goose schedule add --schedule-id daily-report --cron "0 0 9 * * *" --recipe-source ./recipes/daily-report.yaml
  ```
  "초 분 시 일 월 요일 년" 형식을 따르는 5, 6 또는 7자리 cron 표현식을 사용하여 전체 예약 정밀도를 설정할 수 있습니다.

  자세한 예시와 옵션은 [`schedule` 명령 문서](/docs/guides/goose-cli-commands.md#schedule)를 참조하세요.
</TabItem>
</Tabs>

## 핵심 구성 요소

 레시피에는 다음 핵심 구성 요소가 필요합니다:

   - **지침**: 에이전트의 동작과 기능을 정의합니다
      - 에이전트의 미션 선언문 역할을 합니다
      - 모든 관련 작업에 에이전트를 준비시킵니다
      - 프롬프트가 제공되지 않으면 필수입니다

   - **프롬프트** (선택 사항): 대화를 자동으로 시작합니다
      - 프롬프트가 없으면 에이전트가 사용자 입력을 기다립니다
      - 특정하고 즉각적인 작업에 유용합니다
      - 지침이 제공되지 않으면 필수입니다

   - **활동**: 클릭 가능한 버블로 나타나는 예시 작업
      - 사용자가 레시피가 할 수 있는 것을 이해하도록 도와줍니다
      - 시작하기 쉽게 만듭니다

## 고급 기능

### 자동화된 재시도 로직

레시피에는 성공 기준이 충족될 때까지 자동으로 작업 완료를 여러 번 시도하는 재시도 로직이 포함될 수 있습니다. 이는 다음에 특히 유용합니다:

- 성공적인 완료를 보장해야 하는 **자동화 워크플로우**
- 여러 번 시도가 필요할 수 있는 테스트 실행과 같은 **개발 작업**
- 검증 및 정리가 필요한 **시스템 작업**

**기본 재시도 구성:**
```yaml
retry:
  max_retries: 3
  checks:
    - type: shell
      command: "test -f output.txt"  # 출력 파일이 있는지 확인
  on_failure: "rm -f temp_files*"   # 실패 시 정리
```

**작동 방식:**
1. 제공된 지침으로 레시피가 정상적으로 실행됩니다
2. 완료 후 성공 검사가 결과를 검증합니다
3. 검증이 실패하고 재시도가 남아 있으면:
   - 선택적 정리 명령이 실행됩니다
   - 에이전트 상태가 초기 조건으로 재설정됩니다
   - 레시피 실행이 다시 시작됩니다
4. 성공 또는 최대 재시도에 도달할 때까지 프로세스가 계속됩니다

완전한 재시도 구성 옵션과 예시는 [레시피 참조 가이드](/docs/guides/recipes/recipe-reference#retry)를 참조하세요.

### 자동화를 위한 구조화된 출력

레시피는 [구조화된 JSON 출력](/docs/guides/recipes/recipe-reference#response)을 강제할 수 있어 에이전트 응답을 안정적으로 파싱하고 처리해야 하는 자동화 워크플로우에 이상적입니다. 주요 이점은 다음과 같습니다:

- **안정적인 파싱**: 스크립트, 자동화 및 CI/CD 파이프라인을 위한 일관된 JSON 형식
- **내장 검증**: 출력이 요구 사항과 일치하는지 확인
- **쉬운 추출**: 최종 출력이 간단한 파싱을 위해 단일 행으로 나타남

구조화된 출력은 다음에 특히 유용합니다:
- **개발 워크플로우**: 코드 분석 보고서, 통과/실패 횟수가 포함된 테스트 결과, 배포 준비 상태가 포함된 빌드 상태
- **데이터 처리**: 횟수 및 검증 상태가 포함된 결과, 구조화된 발견 사항이 포함된 콘텐츠 분석
- **문서 생성**: 추가 처리를 위한 일관된 메타데이터 및 구조화된 프로젝트 보고서

**구조화된 출력 구성 예시:**
```yaml
response:
  json_schema:
    type: object
    properties:
      build_status:
        type: string
        enum: ["success", "failed", "warning"]
        description: "Overall build result"
      tests_passed:
        type: number
        description: "Number of tests that passed"
      tests_failed:
        type: number
        description: "Number of tests that failed"
      artifacts:
        type: array
        items:
          type: string
        description: "Generated build artifacts"
      deployment_ready:
        type: boolean
        description: "Whether the build is ready for deployment"
    required:
      - build_status
      - tests_passed
      - tests_failed
      - deployment_ready
```

**작동 방식:**
1. 제공된 지침으로 레시피가 정상적으로 실행됩니다
2. goose가 스키마와 일치하는 JSON으로 `final_output` 도구를 호출합니다
3. 출력이 JSON 스키마에 대해 검증됩니다
4. 검증이 실패하면 goose가 오류 세부 정보를 받고 출력을 수정해야 합니다
5. 최종 검증된 JSON이 쉬운 추출을 위해 출력의 마지막 행으로 나타납니다

**자동화 사용 예시:**
```bash
# 레시피 실행 및 JSON 출력 추출
goose run --recipe analysis.yaml --params project_path=./src > output.log
RESULT=$(tail -n 1 output.log)
echo "Analysis Status: $(echo $RESULT | jq -r '.build_status')"
echo "Issues Found: $(echo $RESULT | jq -r '.tests_failed')"
```

:::info
구조화된 출력은 goose CLI와 goose 데스크톱 모두에서 실행되는 레시피에서 지원됩니다. 그러나 `json_schema` 구성을 만들고 편집하는 것은 레시피 파일에서 수동으로 수행해야 합니다.
:::

## 포함되는 내용

레시피는 다음을 캡처합니다:

- AI 지침 (목표/목적)
- 제안된 활동 (사용자가 클릭할 예시)
- 활성화된 확장 기능 및 구성
- 프로젝트 폴더 또는 파일 컨텍스트
- 초기 설정 (전체 대화 기록은 아님)
- 레시피를 실행할 때 사용할 모델 및 프로바이더 (선택 사항)
- 재시도 로직 및 성공 검증 구성 (구성된 경우)


개인정보 보호 및 시스템 무결성을 보호하기 위해 goose는 다음을 제외합니다:

- 전역 및 로컬 메모리
- API 키 및 개인 자격 증명
- 시스템 수준 goose 설정


즉, 레시피가 이러한 요소에 의존하는 경우 다른 사람들이 자신의 자격 증명이나 메모리 컨텍스트를 제공해야 할 수 있습니다.

## 더 알아보기
goose 레시피를 마스터하는 데 도움이 되는 더 많은 문서, 도구 및 리소스는 [레시피](/docs/guides/recipes) 가이드를 확인하세요.
