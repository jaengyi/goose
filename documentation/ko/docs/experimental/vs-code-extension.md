---
title: VS Code용 goose 확장 프로그램
sidebar_label: VS Code 확장 프로그램
sidebar_position: 4
---

import IconGoose from "@site/src/components/icons/goose";

[VS Code용 goose 확장 프로그램](https://marketplace.visualstudio.com/items?itemName=block.vscode-goose)을 사용하면 VS Code 내에서 직접 goose와 상호 작용할 수 있습니다.

:::warning 실험적 기능
VS Code용 goose는 활발히 개발 중입니다. 동작과 구성은 향후 릴리스에서 변경될 수 있습니다.
:::

이 확장 프로그램은 [Agent Client Protocol (ACP)](/docs/guides/acp-clients)를 사용하여 goose와 통신합니다.

## 확장 프로그램 설치

:::info 사전 요구 사항
- [VS Code](https://code.visualstudio.com/) 버전 1.95.0 이상
- [goose CLI](/docs/getting-started/installation) 버전 1.16.0 이상
:::

1. [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=block.vscode-goose)로 이동
2. `Install` 클릭
3. 팝업 창에서 `Open Visual Studio Code` 클릭
4. Visual Studio Code에서 `Install` 클릭
5. Activity Bar에서 <IconGoose className="inline" size={16} /> 아이콘을 클릭하여 사이드 패널을 열고 goose와 채팅 시작

## goose와 컨텍스트 공유

goose에 컨텍스트를 제공하는 두 가지 방법이 있습니다.

### 선택한 코드 보내기

1. 에디터에서 논의하고 싶은 코드 선택
2. 선택 항목을 goose로 보내기:
   - macOS: `Cmd+Shift+G`
   - Windows/Linux: `Ctrl+Shift+G`
   - 우클릭하고 `Send to goose` 선택

선택한 코드가 채팅 입력 위에 컨텍스트 칩으로 나타납니다.

### @ 멘션으로 파일 첨부

1. 채팅 입력에 `@`를 입력하고 파일 이름 입력 시작
2. 파일 선택:
   - 화살표 키(↑/↓)를 사용하여 파일을 선택하고 `Enter` 누르기
   - 마우스를 사용하여 파일 선택

첨부된 파일이 채팅 입력 위에 컨텍스트 칩으로 나타납니다.

## 사용 예제

VS Code용 goose 확장 프로그램은 에디터에 직접 연결되어 AI로 파일, 프로젝트, 워크플로우를 관리할 수 있습니다. 다음 기능이 포함됩니다:

- 스트리밍 응답이 있는 대화형 채팅 UI
- goose의 AI 기능 및 활성화된 확장 프로그램에 대한 액세스
- 기록 재생이 있는 세션 관리
- 컨텍스트 공유 및 채팅 UI의 시각적 컨텍스트 칩
- 쉬운 공유를 위한 코드 스니펫/응답 복사를 위한 클립보드 도구

### goose 프롬프트

```
Update the contributing guide at /goose/CONTRIBUTING.md with instructions on how to start docusaurus dev server
```

### goose 출력

```

I'll help you update the CONTRIBUTING.md file with instructions for starting the Docusaurus dev server. First, let me check the current content of the file.

─── developer__text_editor | view ──────────────────────────

I'll now update the CONTRIBUTING.md file to include instructions for starting the Docusaurus dev server. I'll add this information in a new section under the Node section.

─── developer__text_editor | str_replace ──────────────────────────

I've successfully updated the CONTRIBUTING.md file to include instructions for starting the Docusaurus dev server. I added a new "Documentation" section right after the Node section, which provides clear step-by-step instructions for:

Navigating to the docs directory
Installing dependencies
Starting the development server
I also included helpful information about the hot-reloading feature and the local server URL. The formatting is consistent with the rest of the document, using proper markdown syntax for code blocks and headers.

Is there anything else you'd like me to add or modify in the contributing guide?

```
