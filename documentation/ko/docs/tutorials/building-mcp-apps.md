---
title: MCP Apps 만들기
description: goose Desktop 내에서 렌더링되는 대화형 UI 애플리케이션 만들기
---

import { PanelLeft } from 'lucide-react';

# goose용 MCP Apps 만들기

MCP Apps는 MCP 서버가 텍스트만으로 응답하는 대신 goose 채팅 인터페이스 내에서 직접 렌더링되는 대화형 UI를 반환할 수 있게 합니다. 이를 통해 사용자는 상호 작용을 통해 의도를 표현할 수 있으며, 입력, 반복 또는 시각적 피드백이 필요한 워크플로우에 유용합니다.

:::warning 실험적 기능
goose의 MCP Apps 지원은 실험적이며 초안 사양에 기반합니다. 구현은 최소한이며 변경될 수 있으며, 아직 고급 기능이나 영구 앱 창을 지원하지 않습니다.
:::

이 튜토리얼에서는 JavaScript와 Node.js를 사용하여 MCP App을 만듭니다. 앱에는 대화형 카운터가 포함되어 있고, 호스트 테마와 동기화되며, 채팅으로 메시지를 보내 UI에서 에이전트로 사용자 의도가 어떻게 흐르는지 보여줍니다.

:::info 사전 요구 사항
- Node.js 18+ 설치됨
- goose Desktop 1.19.1+ 설치됨
:::

---

## 1단계: 프로젝트 초기화

새 디렉토리를 만들고 Node.js 프로젝트를 초기화합니다:

```bash
mkdir mcp-app-demo
cd mcp-app-demo
npm init -y
```

MCP SDK 설치:

```bash
npm install @modelcontextprotocol/sdk
```

ES 모듈을 사용하도록 `package.json`에 `"type": "module"` 추가:

```json5
{
  "name": "mcp-app-demo",
  "version": "1.0.0",
  // highlight-next-line
  "type": "module",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "@modelcontextprotocol/sdk": "^1.0.0"
  }
}
```

---

## 2단계: MCP 서버 만들기

`server.js` 생성 - HTML을 로드하고 제공하는 MCP 서버입니다:

<details>
<summary>server.js</summary>

```javascript
#!/usr/bin/env node

import { readFileSync } from "fs";
import { fileURLToPath } from "url";
import { dirname, join } from "path";
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
  ListResourcesRequestSchema,
  ReadResourceRequestSchema,
} from "@modelcontextprotocol/sdk/types.js";

// 파일에서 HTML 로드
const __dirname = dirname(fileURLToPath(import.meta.url));
const APP_HTML = readFileSync(join(__dirname, "index.html"), "utf-8");

// MCP 서버 생성
const server = new Server(
  {
    name: "mcp-app-demo",
    version: "1.0.0",
  },
  {
    capabilities: {
      tools: {},
      resources: {},
    },
  }
);

// 사용 가능한 도구 목록
server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "show_demo_app",
        description: "채팅에 대화형 데모 MCP App UI를 표시합니다",
        inputSchema: {
          type: "object",
          properties: {},
          required: [],
        },
      },
    ],
  };
});

// 도구 호출 처리
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name } = request.params;

  if (name === "show_demo_app") {
    return {
      content: [
        {
          type: "text",
          text: "데모 앱이 표시되었습니다!",
        },
      ],
      // 이 메타데이터는 goose에게 MCP App을 렌더링하도록 지시
      _meta: {
        ui: {
          resourceUri: "ui://mcp-app-demo/main",
        },
      },
    };
  }

  throw new Error(`알 수 없는 도구: ${name}`);
});

// 사용 가능한 리소스 목록
server.setRequestHandler(ListResourcesRequestSchema, async () => {
  return {
    resources: [
      {
        uri: "ui://mcp-app-demo/main",
        name: "MCP App 데모",
        description: "대화형 데모",
        mimeType: "text/html;profile=mcp-app",
      },
    ],
  };
});

// 리소스 콘텐츠 읽기 - HTML 반환
server.setRequestHandler(ReadResourceRequestSchema, async (request) => {
  const { uri } = request.params;

  if (uri === "ui://mcp-app-demo/main") {
    return {
      contents: [
        {
          uri: "ui://mcp-app-demo/main",
          mimeType: "text/html;profile=mcp-app",
          text: APP_HTML,
          _meta: {
            ui: {
              csp: {
                connectDomains: [],
                resourceDomains: [],
                frameDomains: [],
                baseUriDomains: [],
              },
              prefersBorder: true,
            },
          },
        },
      ],
    };
  }

  throw new Error(`리소스를 찾을 수 없음: ${uri}`);
});

// 서버 시작
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("MCP App Demo 서버가 stdio에서 실행 중");
}

main().catch(console.error);
```

</details>

---

## 3단계: 앱 HTML 만들기

`index.html` 생성 - 대화형 UI입니다:

<details>
<summary>index.html</summary>

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>MCP App 데모</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      padding: 20px;
      min-height: 100vh;
      transition: background-color 0.3s, color 0.3s;
    }

    body.light { background: #f5f5f7; color: #1d1d1f; }
    body.dark { background: #1d1d1f; color: #f5f5f7; }

    .container {
      max-width: 500px;
      margin: 0 auto;
      padding: 24px;
      border-radius: 16px;
    }

    body.light .container { background: #ffffff; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
    body.dark .container { background: #2d2d2f; box-shadow: 0 1px 3px rgba(0,0,0,0.3); }

    h1 { font-size: 24px; margin-bottom: 8px; }
    .subtitle { opacity: 0.7; margin-bottom: 20px; font-size: 14px; }

    .counter-section {
      text-align: center;
      padding: 24px;
      border-radius: 12px;
      margin-bottom: 20px;
    }

    body.light .counter-section { background: #f5f5f7; }
    body.dark .counter-section { background: #1d1d1f; }

    .counter-value { font-size: 64px; font-weight: bold; color: #0071e3; }
    .counter-label { font-size: 14px; opacity: 0.6; margin-top: 4px; }

    .button-row { display: flex; gap: 12px; justify-content: center; margin-top: 16px; }

    button {
      padding: 12px 24px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
      color: white;
      transition: opacity 0.2s;
    }

    button:hover { opacity: 0.85; }
    button:active { opacity: 0.7; }

    .btn-increment { background: #0071e3; }
    .btn-decrement { background: #ff3b30; }
    .btn-reset { background: #86868b; }
    .btn-send { background: #34c759; }

    .message-section { margin-top: 20px; }
    .message-section h3 { font-size: 16px; margin-bottom: 12px; }
    .message-input { display: flex; gap: 8px; }

    input[type="text"] {
      flex: 1;
      padding: 12px 16px;
      border-radius: 8px;
      border: 2px solid transparent;
      font-size: 14px;
      transition: border-color 0.2s;
    }

    body.light input { background: #f5f5f7; color: #1d1d1f; }
    body.dark input { background: #1d1d1f; color: #f5f5f7; }
    input:focus { outline: none; border-color: #0071e3; }

    .status {
      margin-top: 16px;
      padding: 12px;
      border-radius: 8px;
      font-size: 13px;
      display: none;
    }

    .status.show { display: block; }
    .status.success { background: rgba(52, 199, 89, 0.15); color: #34c759; }
    .status.error { background: rgba(255, 59, 48, 0.15); color: #ff3b30; }

    .info-section {
      margin-top: 20px;
      padding: 16px;
      border-radius: 8px;
      font-size: 12px;
      opacity: 0.8;
    }

    body.light .info-section { background: #f5f5f7; }
    body.dark .info-section { background: #1d1d1f; }

    .info-section code {
      background: rgba(0, 113, 227, 0.1);
      padding: 2px 6px;
      border-radius: 4px;
      font-family: 'SF Mono', Monaco, monospace;
    }
  </style>
</head>
<body class="light">
  <div class="container">
    <h1>🎮 MCP App 데모</h1>
    <p class="subtitle">goose 내에서 실행되는 대화형 UI</p>

    <div class="counter-section">
      <div class="counter-value" id="counter">0</div>
      <div class="counter-label">카운터 값</div>
      <div class="button-row">
        <button class="btn-decrement" onclick="updateCounter(-1)">−</button>
        <button class="btn-reset" onclick="resetCounter()">리셋</button>
        <button class="btn-increment" onclick="updateCounter(1)">+</button>
      </div>
    </div>

    <div class="message-section">
      <h3>💬 goose에게 메시지 보내기</h3>
      <div class="message-input">
        <input type="text" id="messageInput" placeholder="메시지 입력..." />
        <button class="btn-send" onclick="sendMessage()">전송</button>
      </div>
      <div class="status" id="status"></div>
    </div>

    <div class="info-section">
      <strong>작동 방식:</strong><br><br>
      이 UI는 <code>ui://</code> 스킴의 MCP 리소스로 제공됩니다.
      샌드박스 브릿지를 통해 JSON-RPC 메시지로 goose와 통신합니다.
      <br><br>
      • 카운터는 로컬 상태 사용<br>
      • "전송"은 <code>ui/message</code>를 호출하여 채팅에 텍스트 추가<br>
      • 테마는 goose의 테마 설정과 동기화
    </div>
  </div>

  <script>
    class McpAppClient {
      constructor() {
        this.pendingRequests = new Map();
        this.requestId = 0;
        this.initialized = false;
        this.hostContext = null;

        window.addEventListener('message', (e) => this.handleMessage(e));
        this.initialize();
      }

      async initialize() {
        try {
          const result = await this.request('ui/initialize', {});
          this.hostContext = result.hostContext;
          this.initialized = true;

          if (this.hostContext?.theme) {
            this.applyTheme(this.hostContext.theme);
          }

          this.notify('ui/notifications/initialized', {});
          this.reportSize();
        } catch (error) {
          console.error('MCP App 초기화 실패:', error);
        }
      }

      handleMessage(event) {
        const data = event.data;
        if (!data || typeof data !== 'object') return;

        if ('id' in data && this.pendingRequests.has(data.id)) {
          const { resolve, reject } = this.pendingRequests.get(data.id);
          this.pendingRequests.delete(data.id);
          data.error ? reject(new Error(data.error.message)) : resolve(data.result);
          return;
        }

        if (data.method === 'ui/notifications/host-context-changed') {
          if (data.params?.theme) {
            this.applyTheme(data.params.theme);
          }
        }
      }

      request(method, params) {
        return new Promise((resolve, reject) => {
          const id = ++this.requestId;
          this.pendingRequests.set(id, { resolve, reject });
          window.parent.postMessage({ jsonrpc: '2.0', id, method, params }, '*');

          setTimeout(() => {
            if (this.pendingRequests.has(id)) {
              this.pendingRequests.delete(id);
              reject(new Error('요청 시간 초과'));
            }
          }, 30000);
        });
      }

      notify(method, params) {
        window.parent.postMessage({ jsonrpc: '2.0', method, params }, '*');
      }

      applyTheme(theme) {
        document.body.className = theme;
      }

      reportSize() {
        this.notify('ui/notifications/size-changed', { height: document.body.scrollHeight });
      }

      async sendMessageToChat(text) {
        return this.request('ui/message', { content: { type: 'text', text } });
      }
    }

    const mcpApp = new McpAppClient();

    let counter = 0;

    function updateCounter(delta) {
      counter += delta;
      document.getElementById('counter').textContent = counter;
      mcpApp.reportSize();
    }

    function resetCounter() {
      counter = 0;
      document.getElementById('counter').textContent = counter;
      mcpApp.reportSize();
    }

    async function sendMessage() {
      const input = document.getElementById('messageInput');
      const message = input.value.trim();

      if (!message) {
        showStatus('메시지를 입력하세요', 'error');
        return;
      }

      try {
        await mcpApp.sendMessageToChat(message);
        showStatus('채팅에 메시지가 전송되었습니다!', 'success');
        input.value = '';
      } catch (error) {
        showStatus('전송 실패: ' + error.message, 'error');
      }
    }

    function showStatus(message, type) {
      const status = document.getElementById('status');
      status.textContent = message;
      status.className = 'status show ' + type;
      setTimeout(() => { status.className = 'status'; }, 3000);
    }

    document.getElementById('messageInput').addEventListener('keypress', (e) => {
      if (e.key === 'Enter') sendMessage();
    });
  </script>
</body>
</html>
```

</details>

---

## 4단계: goose Desktop에 추가

1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바 열기
2. `Extensions` 클릭
3. `Add custom extension` 클릭
4. 세부 정보 입력:
   - **Type**: `Standard IO`
   - **ID**: `mcp-app-demo`
   - **Name**: `MCP App Demo`
   - **Command**: `node /full/path/to/mcp-app-demo/server.js`
5. `Add` 클릭

더 많은 옵션은 [확장 프로그램 추가](/docs/getting-started/using-extensions#adding-extensions)를 참조하세요.

---

## 5단계: 앱 테스트

1. goose를 재시작하여 새 확장 프로그램 로드
2. goose에게 프롬프트: "데모 앱 보여줘"
3. goose가 `show_demo_app` 도구를 호출
4. 대화형 앱이 채팅에 렌더링됩니다!

시도해 보세요:
- 카운터 버튼 클릭
- 메시지 입력 후 "전송" 클릭
- goose의 라이트/다크 모드 전환

---

## 작동 방식

```
┌──────────────────────────────────────┐
│           Your MCP App               │  샌드박스된 iframe의 HTML/JS
└──────────────────┬───────────────────┘
                   │ postMessage
┌──────────────────▼───────────────────┐
│          goose Desktop               │  UI 렌더링, 메시지 라우팅
└──────────────────┬───────────────────┘
                   │ MCP Protocol
┌──────────────────▼───────────────────┐
│          Your MCP Server             │  리소스를 통해 HTML 제공
└──────────────────────────────────────┘
```

서버가 `ui://` 리소스 URI를 반환하면 goose가 HTML을 가져와 iframe에 렌더링합니다. 앱은 `postMessage`를 통해 다시 통신합니다—테마 정보 요청, 채팅에 메시지 전송, 크기 조정 등.

MCP Apps는 엄격한 콘텐츠 보안 정책 제한이 있는 샌드박스된 iframe에서 실행됩니다.

### 콘텐츠 보안 정책 구성

기본적으로 앱은 자체 출처의 리소스만 로드할 수 있습니다. 앱이 외부 도메인과 상호 작용해야 하는 경우—CDN에서 리소스 로드, API 호출, 지도 임베딩 등—리소스의 `_meta.ui` 섹션에 있는 `csp` 객체를 통해 허용되는 도메인을 구성할 수 있습니다.

```javascript
_meta: {
  ui: {
    csp: {
      connectDomains: [],      // fetch/XHR 요청용 도메인
      resourceDomains: [],     // 스크립트, 스타일, 이미지, 폰트, 미디어용 도메인
      frameDomains: [],        // 중첩 iframe에 허용되는 출처
      baseUriDomains: [],      // 추가 허용 기본 URI
    },
  },
}
```

| 옵션 | CSP 지시어 | 용도 | 기본값 |
|--------|---------------|---------|---------|
| `connectDomains` | `connect-src` | 앱이 네트워크 요청할 수 있는 도메인 | 동일 출처만 |
| `resourceDomains` | `script-src`, `style-src`, `img-src`, `font-src`, `media-src` | 외부 리소스 로드용 도메인 | 동일 출처만 |
| `frameDomains` | `frame-src` | 중첩 `<iframe>` 요소에 허용되는 출처 | `'none'` (iframe 없음) |
| `baseUriDomains` | `base-uri` | `<base>` 요소에 허용되는 추가 도메인 | `'self'`만 |

<details>
<summary>예제</summary>

**지도 임베딩:**

```javascript
csp: {
  frameDomains: ['https://www.openstreetmap.org'],
  resourceDomains: ['https://tile.openstreetmap.org'],
}
```

**CDN에서 리소스 로드:**

```javascript
csp: {
  resourceDomains: ['https://cdn.jsdelivr.net', 'https://unpkg.com'],
  connectDomains: ['https://api.example.com'],
}
```

</details>

보안 및 전체 프로토콜에 대한 자세한 내용은 [MCP Apps 사양](https://github.com/modelcontextprotocol/ext-apps)을 참조하세요.

:::warning 보안 고려 사항
신뢰하는 도메인만 추가하세요. 추가하는 각 도메인은 앱에 로드되거나 임베딩될 수 있는 외부 콘텐츠를 확장합니다. 보안 위험을 줄이기 위해 목록을 최소화하고 구체적으로 유지하세요.
:::
