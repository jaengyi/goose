---
title: Google Maps 확장 프로그램
description: Google Maps MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

Server archived

이 튜토리얼에서는 지오코딩, 장소 검색, 거리 계산, 고도 데이터 검색, 경로 안내를 활성화하기 위해 [Google Maps MCP 서버](https://www.pulsemcp.com/servers/google-maps-docs)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40modelcontextprotocol%2Fserver-google-maps&id=google-maps&name=Google%20Maps&description=Google%20Maps%20API%20integration&env=GOOGLE_MAPS_API_KEY%3DGoogle%20Maps%20API%20key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @modelcontextprotocol/server-google-maps
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  GOOGLE_MAPS_API_KEY: <YOUR_TOKEN>
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface" defaultValue="ui">
  <TabItem value="ui" label="goose Desktop">
  <GooseDesktopInstaller
    extensionId="google-maps"
    extensionName="Google Maps"
    description="Google Maps API 통합"
    command="npx"
    args={["-y", "@modelcontextprotocol/server-google-maps"]}
    envVars={[
      { name: "GOOGLE_MAPS_API_KEY", label: "Google Maps API Key" }
    ]}
    apiKeyLink="https://developers.google.com/maps/documentation/javascript/get-api-key"
    apiKeyLinkText="Google Maps API Key"
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
    │  Google Maps
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
    │  Google Maps
    │
    // highlight-start
    ◆  What command should be run?
    │  npx -y @modelcontextprotocol/server-google-maps
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
    │  Google Maps
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-google-maps
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
    │  Google Maps
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-google-maps
    │
    ◆  Please set the timeout for this tool (in secs):
    │  300
    │
    // highlight-start
    ◇  Would you like to add a description?
    │  No
    // highlight-end
    │
    └
  ```

  7. [Google Maps API Key](https://developers.google.com/maps/documentation/javascript/get-api-key)를 얻어서 붙여넣으세요.

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
    │  Google Maps
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-google-maps
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    ◇  Would you like to add a description?
    │  No
    │
    // highlight-start
    ◆  Would you like to add environment variables?
    │  Yes
    │
    ◇  Environment variable name:
    │  GOOGLE_MAPS_API_KEY
    │
    ◇  Environment variable value:
    │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪
    │
    ◇  Add another environment variable?
    │  No
    // highlight-end
    └  Added github extension
  ```

  </TabItem>
</Tabs>

## 사용 예제

goose는 배달 기사의 위치를 추적하고, 고객에게 실시간 업데이트를 제공하며, Google Maps 교통 데이터를 기반으로 ETA를 동적으로 조정하는 자율 에이전트 역할을 합니다.

goose는 다음을 수행할 수 있습니다:

**기사 위치 모니터링**
* goose가 몇 초마다 기사의 GPS 좌표를 가져옵니다.
* 기사의 위치가 일정보다 크게 뒤쳐지면 goose가 ETA를 다시 계산합니다.

**교통 인식 ETA 조정**
* goose가 Google Maps에 실시간 교통 상황을 쿼리합니다.
* 지연이 감지되면 goose가 고객 추적기에 새로운 예상 시간을 업데이트합니다.

**동적 고객 알림**
* 기사가 교통 체증에 갇히면 goose가 고객에게 알림을 보냅니다: "교통으로 인해 기사가 늦어지고 있습니다. 업데이트된 ETA: 오후 7:25."
* 기사가 일정보다 앞서면 goose가 레스토랑에 음식을 더 일찍 준비하도록 알립니다.


### goose 프롬프트
```
기사 ID #12345의 실시간 GPS 위치를 추적해줘. Google Maps에서 실시간 교통 데이터를 쿼리하고 지연이 5분을 초과하면 예상 배달 시간을 조정해줘. ETA가 변경되면 고객의 실시간 추적기를 업데이트하고 SMS 알림을 보내줘. 지연이 20분을 초과하면 1마일 반경 내에 배달을 대신할 수 있는 다른 기사가 있는지 확인해줘.
```

### goose 출력

:::note CLI
좋습니다, 기사 #12345를 추적하고, Google Maps에서 교통 데이터를 쿼리하고, ETA를 조정하고, 고객에게 알리겠습니다.

**기사 GPS 위치 추적**

*   **기사 ID:** #12345
*   **시작 지점:** 1955 Broadway, Suite 600, Oakland, CA 94612

기사 #12345의 이동:

```
2025-02-23 08:20:00: Latitude: 37.8074, Longitude: -122.2697 (시작 지점)
2025-02-23 08:21:00: Latitude: 37.8080, Longitude: -122.2690
2025-02-23 08:22:00: Latitude: 37.8090, Longitude: -122.2680
2025-02-23 08:23:00: Latitude: 37.8100, Longitude: -122.2670
```


**목적지 주소 지오코딩**

*   **고객 주소:** 2111 Mission Street, San Francisco, CA

이 주소를 지리 좌표로 변환하겠습니다:


`─── maps_geocode | googlemaps ──────────────────────────`

address: 2111 Mission Street, San Francisco, CA

```
Latitude: 37.7631446
Longitude: -122.4192073
```


**초기 경로 및 ETA 가져오기**

기사의 시작 지점에서 고객 주소까지의 초기 경로와 ETA를 가져오겠습니다.


`─── maps_directions | googlemaps ──────────────────────────`

destination: 2111 Mission St, San Francisco, CA
mode: driving
origin: ...

```
거리: 12.1 마일
초기 ETA: 21분
예상 도착 시간: 오전 08:41
```


**교통 모니터링 및 ETA 조정**

교통 업데이트를 모니터링하고 주기적으로 ETA를 다시 계산하겠습니다.

```
남은 거리: 8 마일
업데이트된 ETA: 15분
예상 도착 시간: 오전 08:45
```

큰 지연이 없습니다.

*오전 08:35:* 교통 사고가 있습니다.


`─── maps_directions | googlemaps ──────────────────────────`

destination: 2111 Mission St, San Francisco, CA
mode: driving
origin: Latitude: 37.8100, Longitude: -122.2670


죄송합니다, 요청에 위도/경도를 사용할 수 없습니다. 대신 이름으로 google maps api를 호출하겠습니다.

`─── maps_directions | googlemaps ──────────────────────────`

destination: 2111 Mission St, San Francisco, CA
mode: driving
origin: ...

```
남은 거리: 7 마일
업데이트된 ETA: 26분
예상 도착 시간: 오전 08:46
```


ETA가 원래 오전 08:41에서 5분 증가했습니다. 경로에 이제 메시지가 포함됩니다: "혼잡으로 인한 지연이 예상됩니다."

**고객 업데이트 및 SMS 전송**

[... 다른 확장 프로그램 사용 (예: Webhooks, MySQL, ClickSend)...]

*   **실시간 추적기 업데이트:** 고객의 실시간 추적 인터페이스가 새 ETA인 오전 08:46으로 업데이트되었습니다.
*   **SMS 알림 전송:** _"교통으로 인해 배달이 약 오전 08:46에 도착할 예정입니다. 불편을 끼쳐 죄송합니다."_
:::
