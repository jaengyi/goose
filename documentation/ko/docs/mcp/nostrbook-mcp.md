---
title: Nostrbook 확장 기능
description: Nostrbook MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';


이 튜토리얼은 [Nostrbook MCP 서버](https://gitlab.com/soapbox-pub/nostrbook)를 goose 확장 기능으로 추가하여 분산 소셜 프로토콜, 구현 가능성 및 기술 사양을 이해하는 데 도움이 되는 Nostr 문서의 포괄적인 레지스트리에 액세스하는 방법을 다룹니다.


:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40nostrbook%2Fmcp&id=nostrbook&name=Nostrbook&description=A%20comprehensive%20registry%20of%20Nostr%20documentation%20with%20structured%20knowledge)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    **명령**
    ```sh
    npx -y @nostrbook/mcp@latest
    ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용).
:::


<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseDesktopInstaller
    extensionId="nostrbook"
    extensionName="Nostrbook"
    description="구조화된 지식을 갖춘 Nostr 문서의 포괄적인 레지스트리"
    command="npx"
    args={["-y", "@nostrbook/mcp"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Nostrbook"
      description="구조화된 지식을 갖춘 Nostr 문서의 포괄적인 레지스트리"
      command="npx -y @nostrbook/mcp@latest"
    />
  </TabItem>
</Tabs>

## 사용 예시

### goose 프롬프트

> _goose, NIP-61이 무엇이고 어떻게 구현할 수 있어?_


### goose 출력

ID는 일반적인 것으로 변경되었습니다

:::note CLI

<details>
    <summary>도구 호출</summary>

    ─── read_nip | nostrbook ──────────────────────────

    nip: 61

</details>



# NIP-61: Nutzaps 구현

NIP-61은 결제 자체가 영수증 역할을 하는 P2PK Cashu 토큰인 "Nutzaps"를 정의합니다. 이 프로토콜은 Cashu 토큰을 사용하여 Nostr 생태계를 통해 비트코인 소액 결제를 가능하게 합니다.

## 핵심 개념

Nutzaps를 사용하면 사용자가 Nostr 이벤트를 통해 서로에게 작은 비트코인 결제를 보낼 수 있으며, 토큰은 수신자의 지정된 공개 키에 잠깁니다.

## 구현 가이드

### 사전 요구 사항

1. [Cashu](https://cashu.space/) 및 [P2PK 토큰 잠금](https://github.com/cashubtc/nuts/blob/main/11.md)에 익숙해지세요
2. 지갑 기능을 위한 [NIP-60](https://github.com/nostr-protocol/nips/blob/master/60.md) 이해
3. 구현이 NUT-11 (P2PK) 및 NUT-12 (DLEQ 증명)를 지원하는지 확인

### 1단계: 수신자 정보 설정 (Nutzaps 수신용)

다른 사람에게 돈을 보내는 방법을 알리기 위해 `kind:10019` 이벤트를 생성하고 게시합니다:

```json
{
    "kind": 10019,
    "tags": [
        ["relay", "wss://your-relay1.com"],
        ["relay", "wss://your-relay2.com"],
        ["mint", "https://your-trusted-mint.com", "sat"],
        ["pubkey", "<your-p2pk-pubkey>"]
    ]
}
```

- `pubkey`는 전용 공개 키여야 합니다 (Nostr ID 키가 아님)
- 해당 개인 키를 NIP-60 지갑 이벤트에 안전하게 저장
- 신뢰하고 모니터링하는 민트만 나열

### 2단계: 발신자 기능 구축

nutzapping을 구현하려면:

1. **수신자 정보 쿼리**:
   ```javascript
   // 수신자의 kind:10019 이벤트 가져오기
   const recipientInfo = await relay.get({
     kinds: [10019],
     authors: [recipientPubkey]
   });
   ```

2. **민트 및 P2PK 정보 추출**:
   ```javascript
   const recipientMints = recipientInfo.tags
     .filter(tag => tag[0] === 'mint')
     .map(tag => tag[1]);

   const recipientLockKey = recipientInfo.tags
     .find(tag => tag[0] === 'pubkey')?.[1];
   ```

3. 수신자의 신뢰할 수 있는 민트 중 하나에서 **토큰을 발행하거나 교환**하여 지정된 키에 P2PK 잠금

4. **nutzap 이벤트 게시**:
   ```json
   {
       "kind": 9321,
       "content": "게시물 감사합니다!",
       "tags": [
           ["proof", "{\"amount\":1,\"C\":\"02...3f\",\"id\":\"000...\",\"secret\":\"[\\\"P2PK\\\",...]\"}"],
           ["u", "https://recipient-specified-mint.com"],
           ["e", "<event-being-zapped>", "<relay-hint>"],
           ["p", "<recipient-pubkey>"]
       ]
   }
   ```

### 3단계: 수신자 기능 구축

nutzap 수신을 구현하려면:

1. **수신 nutzaps 쿼리**:
   ```javascript
   // nutzap 이벤트 구독
   relay.subscribe([
     {
       kinds: [9321],
       "#p": [myPubkey],
       "#u": myTrustedMints,
       since: lastCheckTimestamp
     }
   ]);
   ```

2. **수신 nutzaps 검증**:
   - 토큰이 지정한 민트에서 온 것인지 확인
   - 토큰이 P2PK 키에 잠겨 있는지 확인
   - DLEQ 증명 검증

3. **토큰 상환**:
   - P2PK 토큰을 지갑으로 교환
   - 상환을 기록하기 위해 `kind:7376` 이벤트 생성:
   ```json
   {
       "kind": 7376,
       "content": "<encrypted-content>",
       "tags": [
           ["e", "<nutzap-event-id>", "", "redeemed"],
           ["p", "<sender-pubkey>"]
       ]
   }
   ```

### 중요한 구현 세부 사항

1. **P2PK 키 접두사**: 호환성을 위해 항상 P2PK 잠금 공개 키 앞에 "02"를 붙임
2. **토큰 검증**: DLEQ 증명의 로컬 검증 구현
3. **민트 URL 정규화**: 약간의 URL 변형 문제를 피하기 위해 민트 URL 정규화
4. **사용자 안내**: NUT-11 및 NUT-12 호환 민트만 사용하도록 사용자 안내
5. **게시 위치**: 수신자가 지정한 릴레이에 nutzap 이벤트 게시

## 테스트

구현을 테스트할 때:
1. 적은 금액으로 테스트 nutzaps 생성
2. 토큰 상환이 올바르게 작동하는지 확인
3. 상환 기록이 올바르게 생성되는지 확인
4. 호환성을 위해 여러 민트로 테스트

## 보안 고려 사항

- P2PK 작업에 사용자의 주요 Nostr 키를 절대 사용하지 마세요
- 민감한 지갑 데이터의 적절한 암호화 보장
- 토큰 상환 시도 전에 모든 증명 검증
- 명시적으로 신뢰하는 민트와만 상호작용

이 단계를 따르면 사용자가 Cashu 토큰을 사용하여 Nostr 프로토콜을 통해 비트코인 소액 결제를 보내고 받을 수 있는 완전한 nutzap 솔루션을 구현할 수 있습니다.

:::
