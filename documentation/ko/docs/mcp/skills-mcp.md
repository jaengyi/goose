---
title: Skills 확장 기능
description: goose에 특정 워크플로우를 가르치는 재사용 가능한 지침 세트 로드
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PlatformExtensionNote } from '@site/src/components/PlatformExtensionNote';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

Skills 확장 기능은 *스킬* &mdash; goose가 특정 작업을 수행하거나 특정 워크플로우를 따르는 방법을 가르치는 재사용 가능한 지침 세트를 로드합니다.

goose는 시작 시 자동으로 스킬을 검색하고 요청과 관련이 있을 때 사용합니다. goose는 프로젝트 디렉토리의 `.agents/skills/`와 전역적으로 `~/.config/agents/skills/`에서 스킬을 로드하여 스킬이 다양한 AI 코딩 에이전트 간에 이식 가능하게 만듭니다. 스킬 생성 및 goose가 스킬을 사용하는 방법에 대해 알아보려면 [스킬 사용](/docs/guides/context-engineering/using-skills)을 참조하세요.

## 구성

<PlatformExtensionNote/>

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseBuiltinInstaller
    extensionName="Skills"
    description=".agents/skills 디렉토리에서 스킬 로드 및 사용"
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
  │  ● skills
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예시

goose가 시작 시 `~/.config/agents/skills/deploy/SKILL.md`에서 발견하는 스킬이 있다고 가정해 봅시다:

```markdown
---
name: production-deploy
description: 프로덕션 환경을 위한 안전한 배포 절차
---

# 프로덕션 배포

## 배포 전 체크리스트
1. 모든 테스트 통과 확인
2. 스테이징 환경이 안정적인지 확인
3. #deployments 채널에 알림

## 배포 단계
1. main에서 릴리스 브랜치 생성
2. `npm run build:prod` 실행
3. 먼저 스테이징에 배포, 그 다음 프로덕션
4. 30분 동안 오류율 모니터링
```

### goose 프롬프트

```
현재 브랜치를 프로덕션에 배포해줘
```

### goose 출력

:::note 데스크톱
현재 브랜치를 프로덕션에 배포하는 것을 도와드리겠습니다. 먼저 안전한 배포 절차를 따르기 위해 프로덕션 배포 스킬을 로드하겠습니다.

추적되지 않은 파일이 있는 **`my-feature-branch`** 브랜치에 있는 것을 볼 수 있습니다. 프로덕션 배포를 진행하기 전에 안전한 배포 절차를 따라야 합니다.

#### ⚠️ 배포 전 확인 필요

프로덕션 배포 스킬에 따라 다음 체크리스트를 완료해야 합니다:

**배포 전 체크리스트**
1. **모든 테스트 통과 확인** - 테스트가 실행되었나요?
2. **스테이징 환경이 안정적인지 확인** - 먼저 스테이징에 배포되었나요?
3. **#deployments 채널에 알림** - 팀에 알려야 하나요?

**배포 단계 (확인 후)**
1. main에서 릴리스 브랜치 생성
2. `npm run build:prod` 실행
3. 먼저 스테이징에 배포, 그 다음 프로덕션
4. 30분 동안 오류율 모니터링

---

**진행하기 전에 확인해주세요:**

1. ✅ `my-feature-branch` 브랜치에 대해 모든 테스트가 통과되었나요?
2. ✅ 스테이징에서 테스트되었나요?
3. ✅ #deployments 채널에 알려야 하나요?
4. ✅ `my-feature-branch` 브랜치(`main`이 아닌)를 프로덕션에 배포하시겠습니까?

이 항목들을 확인해주시면 배포를 진행하겠습니다.
:::
