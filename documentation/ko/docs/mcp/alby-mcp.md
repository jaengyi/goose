---
title: Alby Bitcoin Payments 확장 프로그램

description: goose를 Bitcoin Lightning Wallet에 연결하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import { PanelLeft } from 'lucide-react';




이 튜토리얼에서는 Lightning 지갑과 상호 작용하고, 결제를 주고받고, 거래 내역을 조회하고, 법정 화폐 금액을 사토시로 변환하고, Lightning 주소에서 인보이스를 요청하고, 유료 MCP 도구([PaidMCP](https://github.com/getAlby/paidmcp)로 구축된 도구 등)와 상호 작용하기 위해 [Alby Bitcoin Payments MCP 서버](https://github.com/getalby/mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::info
[NWC](https://nwc.dev)를 지원하는 Lightning 지갑이 필요합니다. 아직 없다면 [Alby Hub](https://albyhub.com)를 사용해 보세요.
:::

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40getalby%2Fmcp&id=alby&name=Alby&description=Connect%20goose%20to%20your%20Bitcoin%20Lightning%20Wallet&env=NWC_CONNECTION_STRING%3DNWC%20Connection%20Secret)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @getalby/mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  NWC_CONNECTION_STRING: nostr+walletconnect://...
  ```
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다

**또는** Alby 호스팅 MCP를 사용할 수 있습니다 (아래 원격 옵션 참조).
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <Tabs>
      <TabItem value="local" label="Local" default>
        <GooseDesktopInstaller
          extensionId="alby"
          extensionName="Alby"
          description="goose를 Bitcoin Lightning Wallet에 연결"
          command="npx"
          args={["-y", "@getalby/mcp"]}
          envVars={[
            { name: "NWC_CONNECTION_STRING", label: "NWC Connection Secret" }
          ]}
          customStep3="Lightning 지갑에서 NWC Connection secret (nostr+walletconnect://...)을 얻어서 'NWC Connection Secret' 필드에 붙여넣으세요"
        />
      </TabItem>
      <TabItem value="remote" label="Remote">
          <GooseDesktopInstaller
            extensionId="alby-remote"
            extensionName="Alby"
            description="goose를 Bitcoin Lightning Wallet에 연결"
            type="http"
            url="https://mcp.getalby.com/mcp"
            envVars={[
              { name: "Authorization", label: "Bearer YOUR_NWC_CONNECTION_STRING" }
            ]}
            apiKeyLink="https://nwc.dev"
            apiKeyLinkText="NWC connection secret"
            customStep3="Lightning 지갑에서 NWC connection secret을 얻어서 Bearer 토큰으로 붙여넣으세요 (Bearer nostr+walletconnect://...)"
          />
      </TabItem>
    </Tabs>
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <Tabs>
      <TabItem value="local" label="Local" default>
        <CLIExtensionInstructions
          name="Alby"
          description="goose를 Bitcoin Lightning Wallet에 연결"
          command="npx -y @getalby/mcp"
          envVars={[
            { key: "NWC_CONNECTION_STRING", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
          ]}
          infoNote={
            <>
              Lightning 지갑에서 NWC Connection secret (<code>nostr+walletconnect://...</code>)을 얻어서 붙여넣으세요.
            </>
          }
        />
      </TabItem>
      <TabItem value="remote" label="Remote">
        <CLIExtensionInstructions
          name="Alby"
          description="goose를 Bitcoin Lightning Wallet에 연결"
          type="http"
          url="https://mcp.getalby.com/mcp"
          envVars={[
            { key: "Authorization", value: "Bearer nostr+walletconnect://..." }
          ]}
          infoNote={
            <>
              Lightning 지갑에서 NWC connection secret을 얻어서 <code>Bearer</code> 토큰으로 붙여넣으세요.
            </>
          }
        />
      </TabItem>
    </Tabs>
  </TabItem>
</Tabs>

## 사용 예제

:::info LLM
이 작업에는 Claude Sonnet 3.7이 사용되었습니다. 도구가 올바르게 사용되도록 비슷한 성능의 모델을 권장합니다.
:::

:::tip Memory 확장 프로그램
내장된 memory 확장 프로그램을 사용하여 연락처를 저장하세요. 예: "내 친구 Rene의 lightning 주소는 reneaaron@getalby.com입니다. 메모리에 저장해 주세요."
:::

### goose 프롬프트

> _Rene에게 $1 USD를 보내줘, 메시지는 "Hi Rene, it's Roland. I'm making a goose tutorial!"_

### goose 출력

:::note CLI

<details>
    <summary>Tool Calls</summary>

    ─── fiat_to_sats | alby ──────────────────────────

    fiat_amount: 1

    fiat_currency: USD



    ─── request_invoice | alby ──────────────────────────

    amount_in_sats: 935

    description: ...

    lightning_address: reneaaron@getalby.com



    ─── pay_invoice | alby ──────────────────────────

    invoice: ...



</details>



✅ **결제 성공!**

Rene에게 $1 USD (935 사토시)를 `reneaaron@getalby.com`으로 성공적으로 보냈습니다. 메시지: "Hi Rene, it's Roland. I'm making a goose tutorial!"

**거래 세부 정보:**
- 금액: 935 사토시 ($1 USD)
- 수수료: 3 사토시
- 총 비용: 938 사토시
- 메시지: "Hi Rene, it's Roland. I'm making a goose tutorial!"
- 결제 해시: `cd30642470d81c2e8eb04c9ac95e679c53778537aecdf5554b15288ac42fe129`

Rene은 결제 확인으로 "Thanks, sats received!" 메시지를 받게 됩니다.
:::
