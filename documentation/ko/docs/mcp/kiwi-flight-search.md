---
title: Kiwi Flight Search 확장 프로그램
description: Kiwi Flight Search MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';
import { PanelLeft } from 'lucide-react';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/MhLU91zKE4M" />

Server moved: https://apify.com/agentify/kiwi-mcp-server

이 튜토리얼에서는 항공편 검색 및 가격 비교를 활성화하기 위해 [Kiwi Flight Search MCP 서버](https://mcp.kiwi.com)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  Settings → Extensions에서 `Add custom extension`을 사용하여 다음으로 `Streamable HTTP` 확장 프로그램 유형을 추가하세요:
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  `goose configure`를 사용하여 다음으로 `Remote Extension (Streaming HTTP)` 확장 프로그램 유형을 추가하세요:
  </TabItem>
</Tabs>

  **Endpoint URL**
  ```
  https://mcp.kiwi.com
  ```
:::

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바 열기
    2. `Extensions` 클릭
    3. `Add custom extension` 클릭
    4. `Add custom extension` 모달에서 다음을 입력:
       - **Extension Name**: Kiwi Flight Search
       - **Type**: Streamable HTTP
       - **Endpoint**: `https://mcp.kiwi.com`
    5. `Add Extension`을 클릭하여 확장 프로그램 저장
    6. 채팅으로 이동

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="kiwi-flight-search"
      type="http"
      url="https://mcp.kiwi.com"
      timeout={300}
    />

  </TabItem>
</Tabs>

## 사용 예제

특정 날짜에 두 위치 간의 항공편을 검색해 보겠습니다. 도시 이름이나 공항 코드로 검색할 수 있으며, 확장 프로그램이 가격, 소요 시간, 예약 링크가 포함된 이용 가능한 항공편을 찾습니다.

### goose 프롬프트

```
8월 9일 파리에서 로마로 가는 항공편을 검색해줄 수 있나요?
```

### goose 출력

```
8월 9일 파리에서 로마로 가는 항공편을 검색하는 것을 기꺼이 도와드리겠습니다! 이용 가능한 옵션을 검색하겠습니다.

## ✈️ 항공편 옵션: 파리 → 로마 (2025년 8월 9일)

### 💰 **최저 가격** (가장 저렴한 옵션)

| 노선 | 출발 → 도착 | 클래스 | 가격 | 예약 |
|-------|-------------------|-------|-------|------|
| Paris ORY → Rome FCO | 09/08 22:10 → 00:10+1 (2시간 0분) | 이코노미 | €138 | [지금 예약](https://on.kiwi.com/ZyIJ2r) |
| Paris ORY → Rome FCO | 09/08 09:05 → 11:10 (2시간 5분) | 이코노미 | €154 | [지금 예약](https://on.kiwi.com/31IDMW) |
| Paris ORY → Rome FCO | 09/08 06:25 → 08:25 (2시간 0분) | 이코노미 | €157 | [지금 예약](https://on.kiwi.com/2dNs3f) |

### ⚡ **최단 항공편** (가장 빠른 소요 시간)

| 노선 | 출발 → 도착 | 클래스 | 가격 | 예약 |
|-------|-------------------|-------|-------|------|
| Paris ORY → Rome FCO | 09/08 06:25 → 08:25 (2시간 0분) | 이코노미 | €157 | [지금 예약](https://on.kiwi.com/2dNs3f) |
| Paris ORY → Rome FCO | 09/08 22:10 → 00:10+1 (2시간 0분) | 이코노미 | €138 | [지금 예약](https://on.kiwi.com/ZyIJ2r) |
| Paris CDG → Rome FCO | 09/08 21:10 → 23:10 (2시간 0분) | 이코노미 | €187 | [지금 예약](https://on.kiwi.com/62jyt7) |

### 🕐 **기타 좋은 옵션**

| 노선 | 출발 → 도착 | 클래스 | 가격 | 예약 |
|-------|-------------------|-------|-------|------|
| Paris ORY → Rome FCO | 09/08 12:00 → 14:05 (2시간 5분) | 이코노미 | €164 | [지금 예약](https://on.kiwi.com/m55oIC) |
| Paris ORY → Rome FCO | 09/08 21:10 → 23:10 (2시간 0분) | 이코노미 | €165 | [지금 예약](https://on.kiwi.com/tkAU0T) |

## 📋 **요약 및 권장 사항**

**🏆 전체 최고 가치:** **€138 저녁 항공편** (ORY 22:10 → FCO 00:10+1)은 가장 짧은 비행 시간으로 최저 가격을 제공하지만, 자정 이후에 도착합니다.

**🌅 최고 아침 옵션:** **€157 이른 항공편** (ORY 06:25 → FCO 08:25)은 2시간의 빠른 비행으로 아침 일찍 로마에 도착합니다.

**⏰ 최고 주간 항공편:** **€154 오전 중반 항공편** (ORY 09:05 → FCO 11:10)은 훌륭한 가치와 편리한 시간대를 제공합니다.

모든 직항편은 약 2시간이며, Paris Orly (ORY) 또는 Charles de Gaulle (CDG)에서 Rome Fiumicino (FCO)로 출발합니다.

즐거운 로마 여행 되세요! 🇮🇹 재미있는 사실: 로마는 세계 어느 도시보다 많은 분수가 있습니다 - 2,000개 이상입니다! 영원의 도시로 다시 돌아오기 위해 트레비 분수에 동전을 던지는 것을 잊지 마세요! ⛲✨

```
