---
title: Speech 확장 프로그램
description: Speech MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

Unlist per https://github.com/block/goose/issues/5431

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://youtube.com/embed/rurAp_WzOiY" />


이 튜토리얼에서는 실시간 음성 상호 작용, 오디오/비디오 전사, 텍스트-음성 변환, 다중 화자 오디오 생성을 활성화하기 위해 [Speech MCP 서버](https://github.com/Kvadratni/speech-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::info 요구 사항
마이크에서 오디오를 캡처하기 위해 PyAudio가 필요한 [PortAudio](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/main/scripts/readme-gen/templates/install_portaudio.tmpl.rst#install-portaudio)가 설치되어 있어야 합니다
:::

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=-p&arg=3.10.14&arg=speech-mcp@latest&id=speech_mcp&name=Speech%20Interface&description=Voice%20interaction%20with%20audio%20visualization%20for%20goose)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  uvx -p 3.10.14 speech-mcp@latest
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.

이 확장 프로그램을 추가하기 전에 시스템에 [PortAudio](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/main/scripts/readme-gen/templates/install_portaudio.tmpl.rst#install-portaudio)가 설치되어 있는지 확인하세요. **PortAudio는** 마이크에서 오디오를 캡처하기 위해 PyAudio에 **필수**입니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="speech_mcp"
    extensionName="Speech Interface"
    description="goose를 위한 오디오 시각화가 포함된 음성 상호 작용"
    command="uvx"
    args={["-p", "3.10.14", "speech-mcp@latest"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Command-line Extension` 추가 선택
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◆  What type of extension would you like to add?
    │  ○ Built-in Extension
    // highlight-start
    │  ● Command-line Extension (Run a local command or script)
    // highlight-end
    │  ○ Remote Extension (Streamable HTTP)
    └
  ```

  3. 확장 프로그램 이름 입력
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    // highlight-start
    ◆  What would you like to call this extension?
    │  speech
    // highlight-end
    └
  ```

  4. 명령 입력
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  speech
    │
    // highlight-start
    ◆  What command should be run?
    │  uvx -p 3.10.14 speech-mcp@latest
    // highlight-end
    └
  ```

  5. 작업이 완료되기 전에 goose가 기다려야 하는 타임아웃 시간(초) 입력. 기본값은 300초
   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  speech
    │
    ◇  What command should be run?
    │  uvx -p 3.10.14 speech-mcp@latest
    │
    // highlight-start
    ◆  Please set the timeout for this tool (in secs):
    │  300
    // highlight-end
    └
  ```

  6. 설명 추가 여부 선택. 여기서 "Yes"를 선택하면 확장 프로그램에 대한 설명을 입력하라는 메시지가 표시됩니다.
   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  speech
    │
    ◇  What command should be run?
    │  uvx -p 3.10.14 speech-mcp@latest
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    // highlight-start
    ◇  Would you like to add a description?
    │  No
    // highlight-end
    └
  ```

  7. 환경 변수 추가 여부를 묻는 질문에 No 선택
   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  speech
    │
    ◇  What command should be run?
    │  uvx -p 3.10.14 speech-mcp@latest
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    ◇  Would you like to add a description?
    │  No
    │
    // highlight-start
    ◆  Would you like to add environment variables?
    │  No
    │
    // highlight-end
    └  Added speech extension
  ```

  </TabItem>
</Tabs>


## 사용 예제
이 예제에서는 Speech MCP 서버 확장 프로그램과 함께 goose를 사용하여 음성 명령을 코드로 변환하는 방법을 보여줍니다. goose에 직접 말하면 음성 인터페이스를 시작하고, 음성 프롬프트를 제공하고, goose가 요청에 따라 웹 페이지를 생성하고 열도록 할 수 있습니다.

이를 통해 핸즈프리로 goose와 함께 개발할 수 있어 개발을 더 접근 가능하고 상호 작용적으로 만듭니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   1. goose Desktop에서 새 세션 열기
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  1. 터미널을 열고 새 goose 세션 시작:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

### goose 프롬프트
음성 인터페이스를 활성화하기 위해 말할 수 있는 몇 가지 문구입니다:

- "goose, 오늘은 타이핑하고 싶지 않아."
- "음성으로 대화하자."
- "음성 대화를 할 수 있을까?"
- "타이핑 대신 말하고 싶어."
- "goose, 음성 UI를 실행해줘."

```
goose 오늘은 말하고 싶지 않아
```
### goose 출력
```
알겠습니다! 그런 경우 음성 명령을 대신 사용하도록 도와드릴 수 있습니다. 음성 인터페이스를 실행해서 타이핑 대신 요청을 말씀하실 수 있게 해드리겠습니다

---- Launch Speech UI ----
```

### goose Speech 🎙️

**goose:**
>_음성 인터페이스를 실행했습니다. 이제 타이핑 대신 요청을 말씀하실 수 있습니다. 제 목소리가 들리고 마이크가 활성화되면 말씀해 주세요. 무엇을 도와드릴까요?_


**Max:**

>_goose, 'vibes'라는 단어가 있는 간단한 페이지를 만들고 색상을 바꾸고 페이지 주위로 움직이게 해줘. 완료되면 브라우저에서 이 페이지를 열어줘_


<div style={{ width: "100%", height: 0, position: "relative", paddingBottom: "56.25%" }}>
  <iframe
    src="https://www.youtube.com/embed/vbD8IHwx-OY"
    frameBorder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowFullScreen
    title="YouTube video"
    style={{ position: "absolute", top: 0, left: 0, width: "100%", height: "100%" }}
  ></iframe>
</div>

