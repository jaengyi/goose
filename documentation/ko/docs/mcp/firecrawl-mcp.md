---
title: Firecrawl 확장 프로그램
description: Firecrawl MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://youtube.com/embed/_rKBFLREVcM" />


이 튜토리얼에서는 AI 에이전트를 위한 강력한 웹 스크래핑, 크롤링, 검색 기능을 활성화하기 위해 [Firecrawl MCP 서버](https://github.com/firecrawl/firecrawl-mcp-server)를 goose 확장 프로그램으로 시작하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=firecrawl-mcp&id=firecrawl&name=Firecrawl&description=Web%20scraping%20and%20crawling%20capabilities&env=FIRECRAWL_API_KEY%3DYour%20API%20Key)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y firecrawl-mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  FIRECRAWL_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="firecrawl"
    extensionName="Firecrawl"
    description="웹 스크래핑 및 크롤링 기능"
    command="npx"
    args={["-y", "firecrawl-mcp"]}
    envVars={[{ name: "FIRECRAWL_API_KEY", label: "Your Firecrawl API Key" }]}
    apiKeyLink="https://firecrawl.dev/app/api-keys"
    apiKeyLinkText="Firecrawl API Key"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="firecrawl"
      description="웹 스크래핑 및 크롤링 기능"
      type="stdio"
      command="npx -y firecrawl-mcp"
      timeout={300}
      envVars={[
        { key: "FIRECRAWL_API_KEY", value: "<Your Firecrawl API Key>" }
      ]}
      infoNote={
        <>
          <a href="https://firecrawl.dev/app/api-keys" target="_blank" rel="noopener noreferrer">Firecrawl API Key</a>를 얻어서 붙여넣으세요.
        </>
      }
    />
    </TabItem>
</Tabs>

## 사용 예제

Firecrawl MCP 서버는 다음을 포함한 포괄적인 웹 스크래핑 및 크롤링 기능을 제공합니다:

- **단일 페이지 스크래핑**: 개별 웹 페이지에서 깔끔하고 구조화된 콘텐츠 추출
- **배치 처리**: 배치 작업으로 여러 URL을 효율적으로 스크래핑
- **사이트 크롤링**: 사용자 정의 가능한 깊이로 전체 웹사이트 또는 특정 섹션 크롤링
- **검색 통합**: 고급 필터링으로 크롤링된 콘텐츠 검색

### goose 프롬프트

```
I'm building a multiplayer game and trying to decide between Photon and Unity Netcode. Can you scrape their official documentation and recent comparison articles to help me understand the pros and cons of each platform, including performance, pricing, ease of use, and scalability?
```

### goose 출력

```
I'll help you research Photon and Unity Netcode by scraping their official documentation and finding recent comparison articles. Let me start by gathering information from both platforms' official sites and then look for comparison articles.

──────────────  Firecrawl ─────────────────
Firecrawl Scrape url, formats, onlyMainContent

Firecrawl Scrape url, formats, onlyMainContent

Firecrawl Search query, limit, sources

Firecrawl Scrape url, formats, onlyMainContent

Firecrawl Scrape url, formats, onlyMainContent

Firecrawl Search query, limit, sources

Firecrawl Scrape url, formats, onlyMainContent

Firecrawl Search query, limit, sources

Firecrawl Scrape url, formats, onlyMainContent


Based on my research of the official documentation and recent discussions, I'll provide you with a comprehensive comparison of Photon and Unity Netcode for multiplayer game development.


Photon vs Unity Netcode: Comprehensive Comparison

Overview

Photon is a mature, third-party multiplayer networking solution with multiple products (PUN, Fusion, Quantum) that has been powering games for over a decade. Unity Netcode refers to Unity's first-party networking solutions, primarily Netcode for GameObjects and Netcode for Entities.


Performance Comparison

Photon Fusion

Bandwidth Usage: 6x more efficient than Unity's MLAPI/Mirror
Player Count: Supports thousands of networked objects across hundreds of client connections
Features: Tick-based simulation, client-side prediction, lag compensation, snapshot interpolation
Optimization: Advanced replication algorithms (full consistency vs eventual consistency)

Unity Netcode

Netcode for GameObjects: Designed for casual co-op games (smaller scale)
Netcode for Entities: Built for competitive action games with ECS architecture
Performance: Generally less optimized than Photon Fusion for bandwidth usage
Features: Client/server authoritative models, prediction, interpolation
Winner: Photon Fusion for raw performance and bandwidth efficiency


Pricing Comparison

Photon Pricing (2024)
  Free Tier:
  - 20 CCU for development only
  - 100 CCU for launched games (≈40k MAU)

  Paid Plans:
  - 500 CCU: $95/month (≈200k MAU)
  - 1,000 CCU: $195/month (≈400k MAU)
  - 2,000 CCU: $395/month (≈800k MAU)
  - Premium Cloud: $580/month minimum (auto-scaling up to 50k CCU)

  One-time Options:
  - PUN Plus: $95 one-time for 100 CCU (12 months)

Unity Gaming Services Pricing
  Free Tier:
  - Relay: First 50 average monthly CCU free
  - Lobby: 10 GiB/month bandwidth free
  - Multiplay: $800 credit for 6 months

  Paid Plans:
  - Relay: $0.16 per additional average CCU

  Multiplay Hosting:
  - CPU: $0.038/hour per core
  - RAM: $0.0051/GiB/hour
  - Network: $0.14/GiB

  No free tier after credits expire

Winner: Photon for small to medium games; Unity potentially cheaper for very large scale with dedicated servers


Ease of Use

Photon

Pros:
- Mature documentation and tutorials
- Large community and extensive examples
- Works out-of-the-box with minimal setup
- Cross-platform support (including WebGL without server setup)
Cons:
- Third-party dependency
- Learning curve for advanced features

Unity Netcode

Pros:
- First-party Unity integration
- Consistent with Unity's development patterns
- Direct support from Unity
Cons:
- Newer solution with evolving documentation
- Requires more setup for WebGL (needs Relay service)
- Steeper learning curve for Netcode for Entities

Winner: Photon for immediate productivity and WebGL deployment


Scalability

Photon
- Global Infrastructure: 15+ regions worldwide
- DDoS Protection: Built-in for higher tiers
- Auto-scaling: Available in Premium Cloud plans
- Maximum: Up to 50k CCU (Premium), unlimited (Enterprise)

Unity Netcode
- Infrastructure: Leverages Google Cloud
- Scaling: Manual server management required
- Distributed Authority: New topology for better scaling
- Maximum: Depends on your server infrastructure investment

Winner: Photon for managed scaling; Unity for custom infrastructure control


Platform Support

Photon
- Platforms: All major platforms including WebGL, mobile, console, VR/AR
- WebGL: Works seamlessly without dedicated servers
- Cross-play: Full support across all platforms

Unity Netcode
- Platforms: All Unity-supported platforms
- WebGL: Requires Unity Relay service for multiplayer
- Cross-play: Supported but requires additional setup

Winner: Tie - both support all major platforms


Key Considerations for Your Decision

Choose Photon if:
- You want to deploy quickly, especially to WebGL
- You need proven, battle-tested networking
- You prefer managed infrastructure
- Your game will have moderate CCU (under 10k)
- You want extensive documentation and community support

Choose Unity Netcode if:
- You want first-party Unity integration
- You plan to scale to very high CCU with custom servers
- You need tight control over server infrastructure
- You're building competitive games requiring ECS performance
- You want to avoid third-party dependencies

Recent Community Insights

From the Unity forums discussion, developers note:
- Photon Fusion offers 100 CCU free for one app, combinable with subscriptions
- Unity Multiplay has no free tier but provides $800 credit for 6 months
- WebGL deployment is simpler with Photon as it handles the server backend
- Migration between frameworks requires touching every networking script

Recommendation

For most indie and small-to-medium studios, Photon offers better value and ease of use, especially for WebGL deployment. For larger studios with dedicated infrastructure teams or those requiring maximum performance at scale, Unity Netcode with custom server management might be more cost-effective long-term.

The choice ultimately depends on your specific needs: immediate deployment ease (Photon) vs. long-term infrastructure control (Unity).
```
