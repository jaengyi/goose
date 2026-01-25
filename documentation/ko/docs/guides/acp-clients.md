---
sidebar_position: 105
title: ACP 클라이언트에서 goose 사용하기
sidebar_label: ACP 클라이언트에서 goose
---

[Agent Client Protocol (ACP)](https://agentclientprotocol.com/)를 지원하는 클라이언트 애플리케이션은 goose에 네이티브로 연결할 수 있습니다. 이 통합을 통해 클라이언트에서 직접 goose와 원활하게 상호작용할 수 있습니다.

:::warning 실험적 기능
ACP는 클라이언트가 goose와 같은 AI 에이전트와 통신할 수 있게 해주는 새로운 사양입니다. 이 기능은 채택이 제한적이며 프로토콜이 발전함에 따라 변경될 수 있습니다.
:::

## 작동 방식
ACP 클라이언트에서 goose를 에이전트로 구성하면 확장 기능과 도구를 포함한 goose의 핵심 에이전트 기능에 액세스할 수 있습니다. goose는 또한 ACP 클라이언트에서 [구성된 MCP 서버](#acp-클라이언트에서-mcp-서버-사용)를 자체 확장 기능과 함께 자동으로 로드하여 추가 구성 없이 해당 도구를 사용할 수 있게 합니다.

클라이언트는 다음을 포함하여 goose 수명 주기를 자동으로 관리합니다:

- **초기화**: 클라이언트가 `goose acp` 명령을 실행하여 연결을 초기화합니다
- **통신**: 클라이언트가 JSON-RPC를 사용하여 stdio를 통해 goose와 통신합니다
- **다중 세션**: 클라이언트가 여러 개의 동시 goose 대화를 동시에 관리합니다

:::info 세션 지속성
ACP 세션은 goose의 세션 기록에 저장되며, goose를 사용하여 액세스하고 관리할 수 있습니다. ACP 클라이언트에서의 세션 기록 액세스는 다를 수 있습니다.
:::

:::tip 참조 구현
[goose for VS Code](/docs/experimental/vs-code-extension) 확장 프로그램은 ACP를 사용하여 goose와 통신합니다. 구현 세부 정보는 [vscode-goose](https://github.com/block/vscode-goose) 저장소를 참조하세요.
:::

## ACP 클라이언트에서 설정
ACP를 지원하는 모든 에디터나 IDE는 에이전트 서버로서 goose에 연결할 수 있습니다. 사용 가능한 클라이언트와 문서 링크는 [공식 ACP 클라이언트 목록](https://agentclientprotocol.com/overview/clients)을 확인하세요.

### 예시: Zed 에디터 설정

ACP는 원래 [Zed](https://zed.dev/)에서 개발되었습니다. Zed에서 goose를 구성하는 방법은 다음과 같습니다:

#### 1. 사전 요구 사항

Zed와 goose CLI가 모두 설치되어 있는지 확인하세요:

- **Zed**: [zed.dev](https://zed.dev/)에서 다운로드
- **goose CLI**: [설치 가이드](/docs/getting-started/installation)를 따르세요

  - ACP 지원은 버전 1.16.0 이상에서 가장 잘 작동합니다 - `goose --version`으로 확인하세요.

  - ACP 지원이 작동하는지 테스트하려면 임시로 `goose acp`를 실행하세요:

    ```
    ~ goose acp
    Goose ACP agent started. Listening on stdio...
    ```

    `Ctrl+C`를 눌러 테스트를 종료하세요.

#### 2. goose를 사용자 정의 에이전트로 구성

Zed 설정에 goose를 추가하세요:

1. Zed를 엽니다
2. `Cmd+Option+,` (macOS) 또는 `Ctrl+Alt+,` (Linux/Windows)를 눌러 설정 파일을 엽니다
3. 다음 구성을 추가하세요:

```json
{
  "agent_servers": {
    "goose": {
      "command": "goose",
      "args": ["acp"],
      "env": {}
    }
  },
  // 추가 설정
}
```

이제 Zed에서 직접 goose와 상호작용할 수 있습니다. ACP 세션은 goose 구성에서 활성화된 동일한 확장 기능을 사용하며, 도구(Developer, Computer Controller 등)는 일반 goose 세션과 동일하게 작동합니다.

#### 3. Zed에서 goose 사용 시작

1. **에이전트 패널 열기**: Zed 상태 표시줄의 스파클 에이전트 아이콘을 클릭합니다
2. **새 스레드 만들기**: `+` 버튼을 클릭하여 스레드 옵션을 표시합니다
3. **goose 선택**: `New goose`를 선택하여 goose와 새 대화를 시작합니다
4. **채팅 시작**: 에이전트 패널에서 직접 goose와 상호작용합니다

#### 고급 구성

##### 공급자 및 모델 재정의

기본적으로 goose는 [구성 파일](/docs/guides/config-files)에 정의된 공급자와 모델을 사용합니다. `GOOSE_PROVIDER` 및 `GOOSE_MODEL` 환경 변수를 사용하여 특정 ACP 구성에 대해 이를 재정의할 수 있습니다.

다음 Zed 설정 예시는 두 개의 goose 에이전트 인스턴스를 구성합니다. 이는 다음과 같은 용도에 유용합니다:
- 동일한 작업에서 모델 성능 비교
- 간단한 작업에는 비용 효율적인 모델을, 복잡한 작업에는 강력한 모델을 사용

```json
{
  "agent_servers": {
    "goose": {
      "command": "goose",
      "args": ["acp"],
      "env": {}
    },
    "goose (GPT-4o)": {
      "command": "goose",
      "args": ["acp"],
      "env": {
        "GOOSE_PROVIDER": "openai",
        "GOOSE_MODEL": "gpt-4o"
      }
    }
  },
  // 추가 설정
}
```

## ACP 클라이언트에서 MCP 서버 사용

ACP 클라이언트의 `context_servers`에 구성된 MCP 서버는 goose에서 자동으로 사용할 수 있습니다. 이를 통해 네이티브 클라이언트 기능과 goose 에이전트 통합을 사용할 때 모두 해당 MCP 서버를 사용할 수 있습니다.

**예시 (Zed):**

```json
{
  "context_servers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/allowed/dir"
      ]
    }
  },
  "agent_servers": {
    "goose": {
      "command": "goose",
      "args": ["acp"],
      "env": {}
    }
  },
  // 추가 설정
}
```

사용 가능한 도구를 확인하려면 클라이언트에서 goose가 실행되는 동안 goose에게 물어보세요.

:::info
`context_servers`의 모든 MCP 서버는 stdio(명령 기반) 또는 HTTP 전송을 사용하는 경우 goose에서 자동으로 사용할 수 있습니다. goose는 더 이상 사용되지 않는 SSE 전송을 사용하는 서버를 지원하지 않습니다.

`context_servers`의 서버가 goose 확장 기능과 동일한 이름을 가진 경우, goose는 자체 [구성](/docs/guides/config-files)을 사용합니다.
:::
## 추가 리소스

import ContentCardCarousel from '@site/src/components/ContentCardCarousel';
import chooseYourIde from '@site/blog/2025-10-24-intro-to-agent-client-protocol-acp/choose-your-ide.png';

<ContentCardCarousel
  items={[
    {
      type: 'video',
      title: 'Agent Client Protocol (ACP) 소개 | Vibe Code with goose',
      description: 'ACP를 사용하여 goose를 코드 에디터에 원활하게 통합하고 분산된 워크플로우를 간소화하는 방법을 시청하세요.',
      thumbnailUrl: 'https://img.youtube.com/vi/Hvu5KDTb6JE/maxresdefault.jpg',
      linkUrl: 'https://www.youtube.com/watch?v=Hvu5KDTb6JE',
      date: '2025-10-16',
      duration: '50:23'
    },
   {
      type: 'blog',
      title: 'Agent Client Protocol (ACP) 소개: AI 에이전트-에디터 통합 표준',
      description: 'ACP를 통해 goose와 같은 AI 에이전트를 코드 에디터에 직접 통합하여 창 전환과 벤더 종속을 제거하는 방법을 알아보세요.',
      thumbnailUrl: chooseYourIde,
      linkUrl: '/goose/blog/2025/10/24/intro-to-agent-client-protocol-acp',
      date: '2025-10-24',
      duration: '7분 읽기'
    }
  ]}
/>
