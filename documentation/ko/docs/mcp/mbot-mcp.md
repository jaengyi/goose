---
title: mbot 확장 프로그램
description: MQTT와 MCP를 통해 MakeBlock mbot2 로버를 goose 확장 프로그램으로 제어하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/QKg2Q6YCzdw" />

이 튜토리얼에서는 [MakeBlock mbot2 로버](https://www.makeblock.com/products/buy-mbot2)를 위한 [deemkeen의 MQTT MCP 서버](https://github.com/deemkeen/mbotmcp)를 시작하고 그 과정에서 수행한 일부 코드 변경 사항을 설명합니다.

:::tip 요약
  **Command**
  ```sh
  /path/to/java -jar /path/to/mbotmcp-0.0.1-SNAPSHOT.jar
  ```

  **환경 변수**
  ```
  MQTT_SERVER_URI: tcp://1.2.3.4:1883
  MQTT_PASSWORD: <문자열 또는 비어 있음>
  MQTT_USERNAME: <문자열 또는 비어 있음>
  ```
:::

## 구성

<Tabs groupId="interface">
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="mbot2"
      description="MQTT와 MCP를 통해 MakeBlock mbot2 로버 제어"
      command="/path/to/java -jar /path/to/mbotmcp-0.0.1-SNAPSHOT.jar"
      envVars={[
        { key: "MQTT_SERVER_URI", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" },
        { key: "MQTT_USERNAME", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" },
        { key: "MQTT_PASSWORD", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      commandNote={
        <>
          <code>/path/to/java</code>와 <code>/path/to/mbotmcp-0.0.1-SNAPSHOT.jar</code>를 실제 Java 설치 경로와 JAR 파일 경로로 바꾸세요.
        </>
      }
      infoNote={
        <>
          MQTT_SERVER_URI는 <code>tcp://1.2.3.4:1883</code>과 같은 값을 가집니다. MQTT_USERNAME과 MQTT_PASSWORD는 존재해야 하지만, MQTT 서버가 인증을 요구하지 않는 경우 빈 문자열일 수 있습니다.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예제

사용 가능한 명령으로 mbot2 로버를 운전할 수 있습니다:

- "turn left" 또는 "turn right"
- "forward" 또는 "backward"로 주행
- 무작위로 "explore"
- 탐색 "stop"
- "beep"

기본 이동 거리는 70cm(약 27인치)이고 회전 각도는 90도로 설정되어 있습니다. [mbot2의 Python 코드](https://github.com/deemkeen/mbotmcp/blob/main/assets/mbot-mqtt.py)에서 이 값들을 변경할 수 있습니다. mbot2는 근접 센서, 조명, 장치 하단의 색상 감지 센서 등 다른 많은 기능이 있으며 Python 코드에 추가할 수 있고, MCP를 통해 이러한 명령을 포함하도록 [Java 코드](https://github.com/deemkeen/mbotmcp/blob/main/src/main/java/de/emkeen/mbotmcp/service/BotService.java)를 업데이트해야 합니다.

#### goose 상호작용

```
( O)> mbot2가 영역을 탐색하게 해줘

    좋아요, mbot2 로버를 움직이게 하겠습니다. 영역을 탐색하는 임무로 보내겠습니다.

    > Mbotexplore

( O)> 로버를 멈추고, 왼쪽으로 돌고 앞으로 이동해줘

    알겠습니다, 로버를 멈추고, 왼쪽으로 돌고, 앞으로 이동시키겠습니다.

    > Mbotstop

    > Mbotleft

    > Mbotforward

( O)> 뒤로 이동하고 완료되면 알 수 있게 소리를 내줘

    네, 로버를 뒤로 이동시키고 완료되면 소리를 내겠습니다.

    > Mbotbackward

    > Mbotbeep

    좋아요, mbot2가 이동하고 소리를 냈습니다. 로버로 무엇을 더 하고 싶으신가요?

```
