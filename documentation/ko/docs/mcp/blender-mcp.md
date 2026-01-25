---
title: Blender 확장 프로그램
description: Blender MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/izoQatKtJ2I" />

이 튜토리얼에서는 3D 장면 생성, 자연어로 Blender 제어, 모델 생성, 재질 적용 등을 위해 [Blender MCP 서버](https://github.com/ahujasid/blender-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=blender-mcp&id=blender&name=Blender&description=Blender%203D%20scene%20creation%20integration)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  uvx blender-mcp
  ```
  </TabItem>
</Tabs>
:::

**요구 사항**

[Blender 애플리케이션](https://www.blender.org/download/)과 [Blender MCP 애드온 파일](https://github.com/ahujasid/blender-mcp/blob/main/addon.py)을 다운로드하세요

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

1. [Blender 애플리케이션](https://www.blender.org/download/) 다운로드
2. Blender MCP 애드온 추가
    - [Blender MCP 저장소](https://github.com/ahujasid/blender-mcp/blob/main/addon.py)에서 `addon.py` 파일을 다운로드합니다.
    - Blender 열기
    - `Edit` > `Preferences` > `Add-ons`로 이동합니다.
    - 아래쪽 화살표를 클릭하고 `install from disk`를 선택한 다음 다운로드한 `addon.py` 파일을 추가합니다.
    - 설치 후 체크박스를 선택하여 `Blender MCP`를 활성화합니다.
3. Blender MCP 서버 시작
    - Blender에서 N을 눌러 사이드바를 엽니다.
    - Blender MCP 탭으로 이동합니다.
    - `connect to MCP server` 클릭

### Blender MCP 서버 추가

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="blender"
    extensionName="Blender"
    description="Blender 3D 장면 생성 통합"
    command="uvx"
    args={["blender-mcp"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Blender"
      description="Blender 3D 장면 생성 통합"
      command="uvx blender-mcp"
    />
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose와 Blender 확장 프로그램을 사용하여 처음부터 전체 3D 캠프파이어 장면을 만드는 방법을 보여드리겠습니다. 자연어를 통해 Blender 제어를 자동화하면 goose가 통나무와 불덩이 같은 객체를 만들고, 사실적인 재질을 적용하고, 조명 효과를 추가하고, 불꽃을 위한 파티클을 생성하고, 미묘한 연기를 만들고, 불이 시간에 따라 부드럽게 맥동하도록 애니메이션까지 할 수 있습니다.

이 예제에서는 goose와 Blender 확장 프로그램을 사용하여 원하는 것을 설명하기만 하면 통나무, 불덩이, 조명, 미묘한 불 애니메이션을 포함한 전체 3D 캠프파이어 장면을 만드는 방법을 보여드리겠습니다.

### goose 프롬프트

> 아늑한 캠프파이어 장면을 만들어줘. 지면을 추가하고, 원형으로 몇 개의 통나무를 배치하고, 가운데에 빛나는 불덩이를 추가해줘. 조명을 일몰처럼 따뜻한 톤으로 설정하고, 극적인 샷을 위해 카메라 각도를 맞춰줘. 통나무에는 거친 나무 질감을 주고 불덩이가 빛을 발산하게 해줘.


### goose 출력

:::note CLI

Blender에서 아늑한 캠프파이어 장면을 만드는 것을 도와드리겠습니다. 단계별로 나누어 기본 객체, PolyHaven의 텍스처, 적절한 조명을 조합하여 사용하겠습니다. 먼저 작업 중인 것을 확인해 보겠습니다.

─── **Get Scene Info** ───

단계별로 장면을 만들어 보겠습니다:

먼저, 기본 큐브를 제거하고 지면을 설정하겠습니다:

─── **Execute Blender Code** ───

2. 이제 원형으로 통나무를 만들겠습니다:

─── **Execute Blender Code** ───

3. 가운데에 빛나는 불덩이를 만들겠습니다:

─── **Execute Blender Code** ───

불덩이 재질을 수정하겠습니다:

─── **Execute Blender Code** ───

4. 마지막으로, 극적인 일몰 샷을 위해 조명과 카메라를 설정하겠습니다:

─── **Execute Blender Code** ───

다음 요소로 아늑한 캠프파이어 장면을 만들었습니다:

1. 어두운 갈색 재질의 큰 지면
2. 거친 나무 재질로 원형으로 배열된 5개의 통나무
3. 발광 재질의 빛나는 불덩이
4. 불의 빛나는 효과를 만드는 포인트 라이트
5. 따뜻한 일몰 분위기를 만들기 위해 조정된 메인 태양광
6. 극적인 각도를 위해 배치된 카메라
7. Cycles로 더 나은 렌더링을 위한 장면 설정

장면은 빛나는 불덩이 주위에 자연스럽게 배열된 통나무로 따뜻하고 아늑한 분위기를 가지고 있습니다. 조명은 따뜻한 톤의 일몰을 시뮬레이션하고, 카메라는 흥미로운 각도에서 장면을 포착하도록 배치되었습니다.

조정을 원하시면:

- 요소의 크기나 위치 변경
- 조명 강도 조절
- 카메라 각도 수정
- 작은 돌이나 추가 통나무 같은 세부 사항 추가

알려주시면 장면을 더 세밀하게 다듬는 것을 도와드리겠습니다!
:::

#### Blender 이미지

![Blender MCP Scene](/img/blender.png)
