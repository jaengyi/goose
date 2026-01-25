---
title: Remotion 스킬로 비디오 만들기
description: goose와 Remotion 에이전트 스킬을 사용하여 React로 프로그래매틱 비디오 생성하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

goose는 React로 비디오를 만들 수 있는 프레임워크인 [Remotion](https://www.remotion.dev/)을 사용하여 프로그래매틱 비디오를 만들 수 있습니다. [Remotion Agent Skills](https://www.remotion.dev/docs/ai/skills)를 로드하면 goose는 애니메이션, 컴포지션, 텍스트 효과, 비디오 렌더링에 대한 모범 사례 지식을 얻습니다.

### 사전 요구 사항

- [Node.js](https://nodejs.org/) 18 이상
- 터미널에서 `npx skills add remotion-dev/skills`를 실행하여 [Remotion Agent Skills](https://www.remotion.dev/docs/ai/skills) 설치

:::note Remotion 라이선스
Remotion은 개인 및 소규모 팀에게는 무료이지만, 직원이 3명 이상인 회사는 [상용 라이선스](https://www.remotion.dev/license)가 필요합니다.
:::

## 구성

goose가 Agent Skills를 로드하고 사용할 수 있도록 [Skills 확장 프로그램](/docs/mcp/skills-mcp)을 활성화합니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Skills"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Toggle Extensions` 선택
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
  |
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose에게 git 워크플로우를 보여주는 터미널 스타일 비디오를 만들도록 요청합니다. 비디오에는 타자기 효과, 현실적인 터미널 출력, 마지막에 축하 애니메이션이 포함됩니다.

:::info LLM
이 예제에서는 Anthropic의 Claude Opus 4.5가 사용되었습니다.
:::

### goose 프롬프트

```
Create a terminal-style video showing a git workflow: type out git add .,
then git commit -m 'feat: add new feature', then git push origin main.
Each command should appear with a typewriter effect, followed by realistic output.
End with 'Deployed to production 🚀'
```

### goose 출력

```
─── load_skill | skills ───────────────────────────────────────
name: remotion-best-practices

reading ~/.agents/skills/remotion-best-practices/rules/animations.md

─── shell | developer ─────────────────────────────────────────
mkdir -p git-terminal-video && cd git-terminal-video && npm init -y

─── shell | developer ─────────────────────────────────────────
npm install remotion @remotion/cli react react-dom typescript @types/react

─── text_editor | developer ───────────────────────────────────
writing git-terminal-video/src/GitTerminal.tsx

─── text_editor | developer ───────────────────────────────────
writing git-terminal-video/src/Root.tsx

─── text_editor | developer ───────────────────────────────────
writing git-terminal-video/remotion.config.ts

─── shell | developer ─────────────────────────────────────────
npx remotion render GitTerminal out/git-workflow.mp4

✓ Video rendered successfully

✅ Git Terminal 비디오 생성 완료
위치: git-terminal-video/out/git-workflow.mp4

비디오 세부 정보
├── 길이: 14초 (30fps에서 420프레임)
├── 해상도: 1280x720 (HD)
├── 크기: ~875 KB
└── 코덱: H.264

비디오 내용
├── 터미널 창 - 신호등 버튼이 있는 macOS 스타일
├── 타자기 효과가 있는 명령 시퀀스:
│   ├── ~/my-project ❯ git add .
│   ├── ~/my-project ❯ git commit -m 'feat: add new feature'
│   │   └── 변경된 파일이 포함된 현실적인 출력 표시
│   └── ~/my-project ❯ git push origin main
│       └── push 출력 표시 (enumerating, compressing, writing)
└── 최종 메시지 - 바운스 애니메이션과 함께 "Deployed to production 🚀"

기능
├── 입력 중 깜빡이는 커서
├── 지연된 출력 표시 (각 줄이 딜레이와 함께 나타남)
├── 색상 구분된 구문 (프롬프트, 명령, 출력)
├── 성공 메시지를 위한 스프링 애니메이션
└── 떠다니며 바운스하는 애니메이션 로켓
```
