---
title: Apify 확장 프로그램
description: Apify MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

이 튜토리얼에서는 [Apify Store](https://apify.com/store)의 수천 가지 도구를 호출하여 소셜 미디어, 전자 상거래 사이트, 검색 엔진, 온라인 지도 또는 기타 웹사이트에서 데이터를 추출할 수 있도록 [Apify MCP 서버](https://mcp.apify.com)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

## 구성

<Tabs groupId="remote-or-local">
<!-- REMOTE SETUP -->
<TabItem value="remote" label="Apify Remote MCP" default>

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   [설치 프로그램 실행](goose://extension?type=streamable_http&url=https%3A%2F%2Fmcp.apify.com&id=apify&name=Apify&description=Extract%20data%20from%20any%20website%20with%20thousands%20of%20scrapers%2C%20crawlers%2C%20and%20automations%20on%20Apify%20Store&header=Authorization%3DBearer%20YOUR_APIFY_TOKEN)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  다음으로 `Remote Extension (Streaming HTTP)` 확장 프로그램 유형을 추가하세요:

  **Endpoint URL**
  ```
  https://mcp.apify.com
  ```
  </TabItem>
</Tabs>

  **Custom Request Header**
  ```
  Authorization: Bearer <YOUR_APIFY_TOKEN>
  ```
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="apify"
      extensionName="Apify"
      description="Apify Store의 수천 가지 스크래퍼, 크롤러, 자동화로 모든 웹사이트에서 데이터 추출"
      type="http"
      url="https://mcp.apify.com"
      envVars={[
        { name: "Authorization", label: "Bearer YOUR_APIFY_TOKEN" }
      ]}
      apiKeyLink="https://console.apify.com/settings/integrations"
      apiKeyLinkText="Apify Token"
    />
  </TabItem>

  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Apify"
      description="Apify Store의 수천 가지 스크래퍼, 크롤러, 자동화로 모든 웹사이트에서 데이터 추출"
      type="http"
      url="https://mcp.apify.com"
      timeout={300}
      envVars={[
        { key: "Authorization", value: "Bearer apify_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" }
      ]}
      infoNote={
        <>
          <a href="https://console.apify.com/settings/integrations" target="_blank" rel="noopener noreferrer">Apify Token</a>을 얻어서 <code>Bearer</code> 토큰으로 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

</TabItem>

<!-- LOCAL SETUP -->
<TabItem value="local" label="Apify Local MCP">

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=@apify/actors-mcp-server&arg=start&id=mcp_apify_local&name=Apify%20Local%20MCP%20Server&description=Run%20Apify%20MCP%20server%20locally%20using%20your%20token&env=APIFY_TOKEN%3DYour%20Apify%20Token)
  </TabItem>

  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @apify/actors-mcp-server
  ```
  </TabItem>
</Tabs>

**환경 변수**
```
APIFY_TOKEN: <YOUR_APIFY_TOKEN>
```
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="mcp_apify_local"
      extensionName="Apify Local MCP Server"
      description="Apify Store의 수천 가지 스크래퍼, 크롤러, 자동화로 모든 웹사이트에서 데이터 추출"
      type="stdio"
      command="npx"
      args={["-y", "@apify/actors-mcp-server"]}
      envVars={[
        { name: "APIFY_TOKEN", label: "<YOUR_APIFY_TOKEN>" }
      ]}
      apiKeyLink="https://console.apify.com/settings/integrations"
      apiKeyLinkText="Apify Token"
    />
  </TabItem>

  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Apify Local MCP Server"
      description="Apify Store의 수천 가지 스크래퍼, 크롤러, 자동화로 모든 웹사이트에서 데이터 추출"
      type="stdio"
      command="npx -y @apify/actors-mcp-server"
      timeout={300}
      envVars={[
        { key: "APIFY_TOKEN", value: "apify_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" }
      ]}
      infoNote={
        <>
          <a href="https://console.apify.com/settings/integrations" target="_blank" rel="noopener noreferrer">Apify Token</a>을 얻어서 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

</TabItem>
</Tabs>

## 사용 예제

Apify MCP 서버를 사용하면 goose를 모든 종류의 웹 스크래핑, 크롤링, 데이터 추출 사용 사례를 위한 수천 가지 기성 도구(_Actors_라고 함)가 있는 마켓플레이스인 [Apify Store](https://apify.com/store)에 연결할 수 있습니다. [공식 문서](https://docs.apify.com/platform/actors)에서 Apify Actors에 대해 자세히 알아보세요.

이 예제에서 goose는 공개 평점, 리뷰, 파인 다이닝 인정을 기반으로 프라하에서 가장 신뢰할 수 있는 최고 등급의 이탈리안 레스토랑을 찾기 위해 [Google Places Scraper](https://apify.com/compass/crawler-google-places) Actor를 사용합니다.

### goose 프롬프트

```

( O)> 검증 가능한 신뢰성을 갖춘 프라하의 최고 등급 이탈리안 레스토랑을 찾아줘 — Apify MCP를 사용하여 Google Maps 리뷰 스크래퍼 Actor를 찾고, 최소 200개 이상의 리뷰, Google, TripAdvisor 또는 OpenTable에서 4.7 이상의 평균 평점, 미쉐린 또는 파인 다이닝 인정이 있는 곳에 집중해줘. 레스토랑 이름, 평점, 총 리뷰 수, 주소, 요리 스타일, 그리고 돋보이는 점(예: 서비스, 분위기 또는 시그니처 요리)에 대한 간단한 요약을 포함해줘. 평점 대 리뷰 수 비율이 가장 높은 레스토랑 1개만 반환해줘

```

### goose 출력

```

수집된 정보를 기반으로 검증 가능한 신뢰성을 갖춘 프라하의 최고 등급 이탈리안 레스토랑은 다음과 같습니다:

### **Al Tagliere**

* **평점:** 4.7/5
* **총 리뷰:** 457
* **주소:** [TripAdvisor의 Al Tagliere](https://www.tripadvisor.com/Restaurant_Review-g274707-d6835155-Reviews-Al_Tagliere-Prague_Bohemia.html)
* **요리 스타일:** 이탈리안
* **요약:** Al Tagliere는 지역적 풍미를 보여주는 정통 이탈리안 요리로 유명합니다. 이 레스토랑은 즐거운 분위기, 세심한 서비스, 수제 파스타와 전통 디저트 같은 시그니처 요리로 알려져 있습니다. 손님들은 재료의 신선함과 환영하는 분위기를 자주 강조하며, 현지인과 관광객 모두에게 선호되는 선택입니다.

이 레스토랑은 강력한 평판을 가지고 있으며 품질과 신뢰성에 대한 귀하의 기준을 충족합니다.

```
