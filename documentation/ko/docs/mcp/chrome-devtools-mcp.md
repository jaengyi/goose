---
title: Chrome DevTools 확장 프로그램
description: Chrome DevTools MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/I9FwKbPvgvU" />

이 튜토리얼에서는 Chrome 브라우저에서 브라우저 자동화, 웹 성능 테스트, 대화형 웹 애플리케이션 디버깅을 활성화하기 위해 Chrome DevTools MCP 서버를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=chrome-devtools-mcp%40latest&id=chrome-devtools&name=Chrome%20DevTools&description=Browser%20automation%20and%20web%20performance%20testing%20capabilities)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y chrome-devtools-mcp@latest
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="chrome-devtools"
      extensionName="Chrome DevTools"
      description="브라우저 자동화 및 웹 성능 테스트 기능"
      command="npx"
      args={["-y", "chrome-devtools-mcp@latest"]}
      cliCommand="npx -y chrome-devtools-mcp@latest"
      timeout={300}
    />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Chrome DevTools"
      description="브라우저 자동화 및 웹 성능 테스트 기능"
      command="npx -y chrome-devtools-mcp@latest"
    />
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose와 Chrome DevTools 확장 프로그램을 사용하여 데모 웹사이트에서 자동화된 성능 테스트를 수행하는 방법을 보여드리겠습니다.

### goose 프롬프트
```
chrome dev tools 확장 프로그램을 사용하여 https://www.saucedemo.com으로 이동해줘
그런 다음 performance_glitch_user로 로그인하고 사이트 성능을 측정해줘
```
### goose 출력

```

Sauce Demo 사이트로 이동하여 performance_glitch_user로 로그인하고 성능 영향을 측정하는 것을 도와드리겠습니다. 먼저 현재 열려 있는 페이지를 확인한 다음 사이트로 이동하겠습니다.

완벽합니다! Swag Labs의 로그인 페이지를 볼 수 있습니다. `performance_glitch_user`가 허용되는 사용자 이름 중 하나이고, 모든 사용자의 비밀번호가 `secret_sauce`임을 알 수 있습니다. 이 자격 증명으로 로그인한 다음 성능을 측정하겠습니다.

Saucedemo 웹사이트로 성공적으로 이동하고 `performance_glitch_user`로 로그인하여 사이트 성능을 측정했습니다. 발견한 내용은 다음과 같습니다:

## performance_glitch_user의 성능 분석 결과

### 초기 관찰

1. **로그인 성능**: performance_glitch_user로 초기 로그인이 정상보다 눈에 띄게 더 오래 걸렸습니다 - 로그인 버튼 클릭이 실제로 5초 후에 타임아웃되어 상당한 지연을 나타냈습니다.

2. **페이지 로드 메트릭**:
   - **총 로드 시간**: ~5.03초 (5030ms)
   - **First Paint**: 5.08초
   - **First Contentful Paint**: 5.08초
   - **응답 시간**: 2.5ms (서버 응답은 실제로 빠름)
   - **DOM Content Loaded**: 0.1ms

### 식별된 성능 문제

`performance_glitch_user` 계정의 성능 문제는 네트워크나 서버 문제보다 **클라이언트 측 렌더링 지연**으로 보입니다:

- 서버가 빠르게 응답합니다 (2.5ms 응답 시간)
- 주요 지연은 렌더링 단계에서 발생합니다 (first paint까지 5초 이상)
- 이는 이 특정 사용자에 대해 클라이언트 측 JavaScript 실행에 의도적으로 구축된 성능 결함을 시사합니다

### 대화형 성능

- **버튼 클릭**: 장바구니에 항목 추가가 작동하지만 눈에 띄는 지연이 있음
- **DOM 쿼리**: 표준 DOM 작업이 정상적으로 수행됩니다 (~0.1ms)
- **JavaScript 실행**: 기본 계산이 정상 속도로 실행됩니다 (100k 작업에 ~2ms)

### 장바구니 기능

장바구니에 여러 항목을 성공적으로 추가할 수 있었습니다 (장바구니 카운터가 1에서 3으로 증가), 성능 문제에도 불구하고 핵심 기능이 온전함을 보여줍니다.

### 결론

`performance_glitch_user` 계정은 주로 다음에 영향을 미치는 **의도적인 성능 저하**를 보여줍니다:
- 초기 페이지 렌더링 (5초 이상 지연)
- 로그인 프로세스 (타임아웃)
- 시각적 피드백 지연

이는 느린 기기나 열악한 네트워크 조건에서 사용자가 경험할 수 있는 실제 성능 문제를 시뮬레이션하면서 기능적 정확성을 유지하기 때문에 성능 테스트 시나리오에 탁월한 테스트 계정이 됩니다.
```
