---
title: 커스텀 확장 프로그램 만들기
description: goose 확장 프로그램으로 사용할 자체 MCP 서버 만들기
---

import { PanelLeft } from 'lucide-react';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# goose용 커스텀 확장 프로그램 만들기


goose는 MCP 서버로 구축된 자체 커스텀 확장 프로그램을 만들어 기능을 확장할 수 있습니다. goose가 [Model Context Protocol (MCP)][mcp-docs]를 준수하기 때문에 이러한 확장 프로그램은 goose와 호환됩니다. MCP는 애플리케이션이 LLM에 컨텍스트를 제공하는 방식을 표준화하는 개방형 프로토콜입니다. LLM을 다양한 데이터 소스 및 도구에 연결하는 일관된 방법을 제공하여 구조화되고 상호 운용 가능한 방식으로 기능을 확장하는 데 이상적입니다.

이 가이드에서는 [MCP용 Python SDK][mcp-python]를 사용하여 MCP 서버를 구축합니다. Wikipedia 문서를 읽고 Markdown으로 변환하는 MCP 서버를 만들어 goose의 확장 프로그램으로 통합하는 방법을 시연합니다. 유사한 프로세스를 따라 goose용 자체 커스텀 확장 프로그램을 개발할 수 있습니다.

[MCP 서버 저장소][mcp-servers]에서 다른 예제 서버들을 확인할 수 있습니다. MCP SDK는 [Typescript][mcp-typescript] 및 [Kotlin][mcp-kotlin]과 같은 다른 일반적인 언어로도 제공됩니다.

:::info
goose는 [Model Context Protocol](https://modelcontextprotocol.io/)의 Tools, Resources, Prompts를 지원합니다. 지원되는 프로토콜 버전과 클라이언트 기능은 [`mcp_client.rs`](https://github.com/block/goose/blob/main/crates/goose/src/agents/mcp_client.rs)를 참조하세요.
:::

---

## 사전 요구 사항

시작하기 전에 시스템에 다음이 설치되어 있는지 확인하세요:

- **Python 3.13 이상** - MCP 서버에 필요
- **[uv](https://docs.astral.sh/uv/)** - 이 튜토리얼에서 사용하는 Python 패키지 관리자
- **Node.js 및 npm** - [4단계](#step-4-test-your-mcp-server)에서 MCP Inspector 개발 도구를 사용하려는 경우에만 필요

---

## 1단계: 프로젝트 초기화

첫 번째 단계는 [uv][uv-docs]를 사용하여 새 프로젝트를 만드는 것입니다. 프로젝트 이름은 `mcp-wiki`로 하겠습니다.

터미널에서 다음 명령을 실행하여 MCP 서버의 기본 구조를 설정하세요:

```bash
uv init --lib mcp-wiki
cd mcp-wiki

mkdir -p src/mcp_wiki
touch src/mcp_wiki/server.py
touch src/mcp_wiki/__main__.py
```

프로젝트 디렉토리 구조는 다음과 같아야 합니다:

```plaintext
.
├── README.md
├── pyproject.toml
└── src
    └── mcp_wiki
        ├── __init__.py   # 기본 CLI 진입점
        ├── __main__.py   # Python 모듈로 실행 가능하게 함
        ├── py.typed      # 패키지가 타입 힌트를 지원함을 나타냄
        └── server.py     # MCP 서버 코드 (tool, resources, prompts)
```

---

## 2단계: MCP 서버 코드 작성

이 단계에서는 MCP 서버의 핵심 기능을 구현합니다. 주요 구성 요소에 대한 설명은 다음과 같습니다:

1. **`server.py`**: 이 파일에는 주요 MCP 서버 코드가 들어 있습니다. 이 예제에서는 Wikipedia 문서를 읽는 단일 도구를 정의합니다. 여기에 자체 커스텀 도구, 리소스, 프롬프트를 추가할 수 있습니다.
2. **`__init__.py`**: MCP 서버의 기본 CLI 진입점입니다.
3. **`__main__.py`**: 이 파일을 통해 MCP 서버를 Python 모듈로 실행할 수 있습니다.

아래는 Wikipedia MCP 서버의 예제 구현입니다:

### `server.py`

```python
import requests
from requests.exceptions import RequestException
from bs4 import BeautifulSoup
from html2text import html2text
from urllib.parse import urlparse

from mcp.server.fastmcp import FastMCP
from mcp.shared.exceptions import McpError
from mcp.types import ErrorData, INTERNAL_ERROR, INVALID_PARAMS

mcp = FastMCP("wiki")

@mcp.tool()
def read_wikipedia_article(url: str) -> str:
    """
    제공된 URL에서 Wikipedia 문서를 가져와 주요 콘텐츠를 파싱하고,
    Markdown으로 변환하여 결과 텍스트를 반환합니다.

    사용법:
        read_wikipedia_article("https://en.wikipedia.org/wiki/Python_(programming_language)")
    """
    try:
        # 입력 검증
        if not url.startswith("http"):
            raise ValueError("URL은 http 또는 https로 시작해야 합니다.")

        # SSRF 보호: Wikipedia 도메인만 허용
        parsed = urlparse(url)
        hostname = parsed.netloc.lower()

        # wikipedia.org 또는 *.wikipedia.org 서브도메인만 허용
        if hostname != 'wikipedia.org' and not hostname.endswith('.wikipedia.org'):
            raise ValueError(f"Wikipedia URL만 허용됩니다. 입력값: {parsed.netloc}")

        # Wikipedia에서 403 오류를 피하기 위해 User-Agent 헤더 추가
        headers = {
            'User-Agent': 'MCP-Wiki/1.0 (Educational purposes; Python requests)'
        }
        response = requests.get(url, headers=headers, timeout=10)
        if response.status_code != 200:
            raise McpError(
                ErrorData(
                    code=INTERNAL_ERROR,
                    message=f"문서를 가져오는 데 실패했습니다. HTTP 상태 코드: {response.status_code}"
                )
            )

        soup = BeautifulSoup(response.text, "html.parser")
        content_div = soup.find("div", {"id": "mw-content-text"})
        if not content_div:
            raise McpError(
                ErrorData(
                    code=INVALID_PARAMS,
                    message="제공된 Wikipedia URL에서 주요 콘텐츠를 찾을 수 없습니다."
                )
            )

        # Markdown으로 변환
        markdown_text = html2text(str(content_div))
        return markdown_text

    except ValueError as e:
        raise McpError(ErrorData(code=INVALID_PARAMS, message=str(e))) from e
    except RequestException as e:
        raise McpError(ErrorData(code=INTERNAL_ERROR, message=f"요청 오류: {str(e)}")) from e
    except Exception as e:
        raise McpError(ErrorData(code=INTERNAL_ERROR, message=f"예상치 못한 오류: {str(e)}")) from e
```

### `__init__.py`

```python
import argparse
from .server import mcp

def main():
    """MCP Wiki: Wikipedia 문서를 읽고 Markdown으로 변환합니다."""
    parser = argparse.ArgumentParser(
        description="Wikipedia 문서를 읽고 Markdown으로 변환하는 기능을 제공합니다."
    )
    parser.parse_args()
    mcp.run()

if __name__ == "__main__":
    main()
```

### `__main__.py`

```python
from mcp_wiki import main

main()
```

---

## 3단계: 프로젝트 구성 정의

`pyproject.toml`을 사용하여 프로젝트를 구성합니다. 이 구성은 mcp-wiki 명령을 바이너리로 사용할 수 있도록 CLI 스크립트를 정의합니다. 아래는 예제 구성입니다:

```toml
[project]
name = "mcp-wiki"
version = "0.1.0"
description = "Wikipedia용 MCP 서버"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "beautifulsoup4>=4.14.0",
    "html2text>=2025.4.15",
    "mcp[cli]>=1.25.0",
    "requests>=2.32.3",
]

[project.scripts]
mcp-wiki = "mcp_wiki:main"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

## 4단계: MCP 서버 테스트

MCP Inspector(브라우저 기반 개발 도구) 또는 서버 CLI에서 MCP 서버가 실행되는지 확인합니다.

<Tabs>
  <TabItem value="ui" label="MCP Inspector에서" default>
:::info
MCP Inspector를 사용하려면 컴퓨터에 Node.js와 npm이 설치되어 있어야 합니다.
:::

1. 프로젝트 환경 설정:

   ```bash
   uv sync
   ```

2. 가상 환경 활성화:

   ```bash
   source .venv/bin/activate
   ```

3. 개발 모드에서 서버 실행:

   ```bash
   mcp dev src/mcp_wiki/server.py
   ```

   MCP Inspector가 브라우저에서 자동으로 열립니다. 처음 실행 시 `@modelcontextprotocol/inspector` 설치를 요청받습니다.

4. 도구 테스트:
   1. `Connect`를 클릭하여 MCP 서버 초기화
   2. `Tools` 탭에서 `List Tools`를 클릭하고 `read_wikipedia_article` 도구 클릭
   3. URL에 `https://en.wikipedia.org/wiki/Bangladesh`를 입력하고 `Run Tool` 클릭

[![MCP Inspector UI](../assets/guides/custom-extension-mcp-inspector.png)](../assets/guides/custom-extension-mcp-inspector.png)

  </TabItem>
  <TabItem value="cli" label="CLI에서">
1. 프로젝트 환경 설정:

   ```bash
   uv sync
   ```

2. 가상 환경 활성화:

   ```bash
   source .venv/bin/activate
   ```

3. 프로젝트 로컬 설치:

   ```bash
   uv pip install .
   ```

4. CLI 확인:

   ```bash
   mcp-wiki --help
   ```

   다음과 유사한 출력이 표시됩니다:

   ```plaintext
   ❯ mcp-wiki --help
   usage: mcp-wiki [-h]

   Wikipedia 문서를 읽고 Markdown으로 변환하는 기능을 제공합니다.

   options:
     -h, --help  show this help message and exit
   ```
  </TabItem>
</Tabs>

---

## 5단계: goose와 통합

MCP 서버를 goose의 확장 프로그램으로 추가하려면:

1. 확장 프로그램 바이너리 빌드:

   ```bash
   uv pip install .
   ```

2. goose Desktop을 열고 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바 열기
3. 사이드바에서 `Extensions` 클릭
4. `Type`을 `STDIO`로 설정
5. 확장 프로그램의 이름과 설명 입력
6. `Command` 필드에 실행 파일의 절대 경로 입력:
   ```plaintext
   uv run /full/path/to/mcp-wiki/.venv/bin/mcp-wiki
   ```

   예시:
   ```plaintext
   uv run /Users/smohammed/Development/mcp/mcp-wiki/.venv/bin/mcp-wiki
   ```

:::tip 변경 후 바이너리 재빌드
goose와 통합한 후 MCP 서버 코드를 변경한 경우, `uv pip install .`을 다시 실행한 다음 goose Desktop을 재시작해야 변경 사항이 반영됩니다.
:::

이 가이드에서는 로컬 버전을 실행합니다. 또는 패키지를 PyPI에 게시할 수 있습니다. 게시되면 `uvx`를 사용하여 서버를 직접 실행할 수 있습니다. 예:

```
uvx mcp-wiki
```

---

## 6단계: goose에서 확장 프로그램 사용

통합되면 goose에서 확장 프로그램을 사용할 수 있습니다. goose 채팅 인터페이스를 열고 필요에 따라 도구를 호출하세요.

goose가 커스텀 확장 프로그램의 도구를 인식했는지 확인하려면 "어떤 도구가 있어?"라고 물어보세요.

![goose Chat - 도구에 대해 질문](../assets/guides/custom-extension-tools.png)

그런 다음 추가한 확장 프로그램을 사용해야 하는 질문을 해볼 수 있습니다.

![goose Chat - 커스텀 확장 프로그램 사용](../assets/guides/custom-extension-chat.png)

🎉 **축하합니다!** goose와 커스텀 MCP 서버를 성공적으로 구축하고 통합했습니다.

---

## MCP 확장 프로그램의 고급 기능

goose는 확장 프로그램을 향상시킬 수 있는 고급 MCP 기능을 지원합니다.

### MCP Sampling: AI 기반 도구

**[MCP Sampling](/docs/guides/mcp-sampling)**을 사용하면 MCP 서버가 goose의 LLM에서 AI 완성을 요청할 수 있어 간단한 도구를 지능형 에이전트로 변환할 수 있습니다.

**주요 이점:**
- MCP 서버에 자체 OpenAI/Anthropic API 키가 필요 없음
- 도구가 데이터를 분석하고, 설명을 제공하고, 지능적인 결정을 내릴 수 있음
- 더 스마트하고 상황에 맞는 응답으로 향상된 사용자 경험
- 설계상 안전: 요청이 격리되고 자동으로 귀속됨

**시작하기:**
- MCP 서버에서 `sampling/createMessage` 메서드를 사용하여 AI 지원 요청
- [goose 구현](https://github.com/block/goose/blob/main/crates/goose/src/agents/mcp_client.rs)은 현재 텍스트 및 이미지 콘텐츠 유형 지원
- goose는 모든 MCP 서버에 샘플링 기능을 자동으로 알림

**사용 사례:** 문서 요약, 스마트 검색 필터링, 코드 분석, 데이터 인사이트

**자세히 알아보기:** 기술적인 세부 사항은 [MCP 사양](https://modelcontextprotocol.io/specification/draft/client/sampling)을 참조하세요.

### MCP Apps: 대화형 확장 프로그램

**[MCP Apps](/docs/tutorials/building-mcp-apps)**는 텍스트 전용 응답 대신 풍부한 대화형 사용자 인터페이스를 활성화합니다.

**주요 이점:**
- MCP 서버 도구에서 대화형 UI 컴포넌트 반환
- 컴포넌트가 goose Desktop 내의 격리된 샌드박스에서 안전하게 렌더링
- 실시간 사용자 상호 작용이 서버에 콜백을 트리거

**사용 사례:** 대화형 양식, 데이터 시각화, 예약 인터페이스, 구성 마법사

**자세히 알아보기:** [MCP Apps 만들기 튜토리얼](/docs/tutorials/building-mcp-apps)

:::note
goose는 [MCP-UI](/docs/guides/interactive-chat/mcp-ui)도 지원하지만, 새 확장 프로그램에는 MCP Apps가 권장됩니다.
:::

[mcp-docs]: https://modelcontextprotocol.io/
[mcp-python]: https://github.com/modelcontextprotocol/python-sdk
[mcp-typescript]: https://github.com/modelcontextprotocol/typescript-sdk
[mcp-kotlin]: https://github.com/modelcontextprotocol/kotlin-sdk
[mcp-servers]: https://github.com/modelcontextprotocol/servers
[uv-docs]: https://docs.astral.sh/uv/getting-started/
