---
sidebar_position: 1
title: 세션 관리
sidebar_label: 세션 관리
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { AppWindow, PanelLeft, FolderDot, Paperclip, Copy, Edit2, Trash2, Download, Upload } from 'lucide-react';


세션은 사용자와 goose 간의 단일 지속적인 상호작용으로, 질문하고 작업을 요청할 수 있는 공간을 제공합니다. 이 가이드에서는 세션 수명 주기를 관리하는 방법을 다룹니다.

## 세션 시작

:::info 최초 설정
첫 번째 세션에서 goose는 [LLM(대규모 언어 모델) 공급자 설정](/docs/getting-started/installation#set-llm-provider)을 안내합니다.
:::

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        goose를 열면 바로 사용할 수 있는 세션 인터페이스가 표시됩니다. 입력 필드에 직접 질문, 요청 또는 지시를 입력하면&mdash;[또는 음성으로 말하면](/docs/guides/sessions/in-session-actions#voice-dictation "음성 받아쓰기를 활성화하는 방법 알아보기")&mdash;goose가 즉시 작업을 시작합니다.

        동일한 goose 창이나 새 창에서 언제든지 새 세션을 시작할 수도 있습니다.

        <Tabs groupId="window-type">
            <TabItem value="same-window" label="같은 창" default>

                1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
                2. 사이드바에서 `Home`을 클릭합니다
                3. 채팅 상자에서 첫 번째 프롬프트를 보냅니다
            </TabItem>
            <TabItem value="new-window" label="새 창">

                1. 왼쪽 상단의 <AppWindow className="inline" size={16} /> 버튼을 클릭합니다
                2. 새 창에서 채팅 상자에서 첫 번째 프롬프트를 보냅니다

            </TabItem>
        </Tabs>

        작업 디렉토리를 변경하려면 앱 하단의 <FolderDot className="inline" size={16} /> 디렉토리 전환기를 클릭합니다.

        :::tip
        macOS에서는 goose 독 아이콘을 사용하여 빠르게 세션을 시작할 수 있습니다:
            - 폴더를 goose 아이콘으로 **끌어다 놓기**하면 해당 디렉토리에서 새 세션을 엽니다
            - goose 아이콘을 **우클릭**하고 `New Window`를 선택하면 가장 최근 디렉토리에서 새 세션을 엽니다
        :::

        #### 키보드 단축키

        키보드 단축키를 사용하여 새 세션을 시작하거나 goose 창을 관리할 수도 있습니다.

        | 작업 | macOS | Windows/Linux |
        |--------|-------|---------------|
        | [빠른 실행기](#빠른-실행기)로 새 세션 | `Cmd+Option+Shift+G` | `Ctrl+Alt+Shift+G` |
        | 현재 디렉토리에서 새 세션 | `Cmd+N` | `Ctrl+N` |
        | 현재 디렉토리에서 새 세션 (같은 창) | `Cmd+T` | `Ctrl+T` |
        | 다른 디렉토리에서 새 세션 | `Cmd+O` | `Ctrl+O` |
        | 사이드바 토글 | `Cmd+B` | `Ctrl+B` |
        | 설정 열기 | `Cmd+,` | `Ctrl+,` |
        | goose 창 항상 위에 유지 | `Cmd+Shift+T` | `Ctrl+Shift+T` |

        <br />
        #### 빠른 실행기
        팝업에 프롬프트를 입력하여 새 세션을 시작합니다:
        1. `Cmd+Option+Shift+G` (macOS) 또는 `Ctrl+Alt+Shift+G` (Windows/Linux)를 눌러 팝업을 엽니다
        2. 프롬프트를 입력하고 `Enter`를 누릅니다

        세션은 새 goose 창에서 가장 최근에 열린 디렉토리로 열립니다.

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        터미널에서 시작하려는 디렉토리로 이동하고 [session](/docs/guides/goose-cli-commands#session-options) 명령을 실행합니다:
        ```sh
        goose session
        ```

        웹 기반 채팅 인터페이스에서 goose와 상호작용하려면 [`web`](/docs/guides/goose-cli-commands#web) 명령으로 세션을 시작합니다:
        ```sh
        goose web --open
        ```
    </TabItem>
</Tabs>

## 세션 이름 지정
<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        데스크톱 앱에서 세션 타일은 초기 프롬프트의 컨텍스트를 기반으로 자동 생성된 설명을 표시합니다.

        세션 설명은 생성 후 편집할 수 있습니다:

        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
        2. 사이드바에서 `History`를 클릭합니다
        3. 이름을 변경하려는 세션 위에 마우스를 올립니다
        4. 세션 카드에 나타나는 <Edit2 className="inline" size={16} /> 버튼을 클릭합니다
        5. 열리는 "Edit Session Description" 모달에서:
           - 새 세션 설명을 입력합니다 (최대 200자)
           - `Enter`를 눌러 저장하거나 `Escape`를 눌러 취소합니다
           - 또는 `Save` 또는 `Cancel` 버튼을 클릭합니다
        6. 성공 토스트 알림이 변경을 확인합니다

        :::tip
        세션 설명은 여러 goose 창을 관리하는 데 도움이 됩니다. goose 채팅 인터페이스에서 세션 설명은 `Window` 메뉴와 Dock (macOS) 또는 작업 표시줄 (Windows) 메뉴에 표시되어 다른 goose 세션을 쉽게 식별하고 전환할 수 있습니다.
        :::

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        기본적으로 goose는 `YYYYMMDD_<COUNT>` 형식의 현재 타임스탬프를 사용하여 세션 이름을 지정합니다. 특정 이름을 제공하려면 여기서 지정합니다. 예를 들어 세션 이름을 `react-migration`으로 지정하려면 다음을 실행합니다:

        ```
        goose session -n react-migration
        ```

        터미널이 다음과 유사하게 표시되면 세션이 시작된 것입니다:

        ```
        starting session | provider: openai model: gpt-4o
        session id: react-migration
        working directory: /path/to/your/project
        ```
    </TabItem>
</Tabs>

## 세션 종료
세션은 종료할 때 자동으로 저장됩니다.
<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
    세션을 종료하려면 애플리케이션을 닫으면 됩니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        세션을 종료하려면 `exit`를 입력합니다. 또는 `Ctrl+C`를 눌러 세션을 종료할 수 있습니다.

        세션은 `~/.local/share/goose/sessions/sessions.db`의 로컬 SQLite 데이터베이스에 저장됩니다.
    </TabItem>
</Tabs>

## 세션 검색

검색을 사용하면 세션 내에서 특정 콘텐츠를 찾거나 특정 세션을 찾을 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>

    goose 데스크톱에서 키보드 단축키와 검색 창 버튼을 사용하여 세션을 검색할 수 있습니다.

    | 작업 | macOS | Windows/Linux |
    |--------|-------|---------------|
    | 검색 열기 | `Cmd+F`  | `Ctrl+F`  |
    | 다음 일치 항목 | `Cmd+G` 또는 `↓` | `Ctrl+G` 또는 `↓` |
    | 이전 일치 항목 | `Shift+Cmd+G` 또는 `↑` | `Shift+Ctrl+G` 또는 `↑` |
    | 선택 항목을 검색에 사용 | `Cmd+E` | 해당 없음 |
    | 대소문자 구분 토글 | `Aa` | `Aa` |
    | 검색 닫기 | `Esc` 또는 `X` | `Esc` 또는 `X` |

    다음 시나리오가 지원됩니다:

    #### 현재 세션 내 검색

    현재 세션 내에서 특정 콘텐츠를 찾으려면:

    1. `Cmd+F`를 사용하여 검색 창을 엽니다
    2. 검색어를 입력합니다
    3. 단축키와 검색 창 버튼을 사용하여 결과를 탐색합니다

    #### 이름 또는 경로로 세션 검색

    이름 또는 작업 디렉토리 경로로 모든 세션을 검색하려면:

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `History`를 클릭합니다
    3. `Cmd+F`를 사용하여 검색 창을 엽니다
    4. 검색어를 입력합니다
    5. 키보드 단축키와 검색 창 버튼을 사용하여 결과를 탐색합니다 (`Cmd+E`는 지원되지 않음)

    이것은 메타데이터 전용 검색입니다. 대화 내용을 검색하지 않습니다. 세션 ID(예: `20251108_1`)로 검색하는 것도 지원되지만 이 속성은 UI에 표시되지 않습니다.

    :::tip
    나중에 기억할 수 있는 설명적인 이름을 지정하기 위해 [세션 이름을 변경](#세션-이름-지정)할 수 있습니다.
    :::

    #### 모든 세션 콘텐츠 검색

    모든 세션의 대화 내용을 검색하려면 채팅 세션에서 직접 goose에게 요청하세요. 예를 들어:

    - "지난주 React hooks에 대한 이전 대화 찾기"
    - "데이터베이스 마이그레이션 작업을 한 세션 보여주기"

    goose는 세션 기록을 검색하고 일치하는 세션의 컨텍스트와 함께 관련 대화를 표시합니다.

    :::info
    이 기능을 사용하려면 내장된 [Chatrecall 확장 기능](/docs/mcp/chatrecall-mcp)이 활성화되어 있어야 합니다.
    :::

    #### 이전 세션 내 검색

    이전 세션 내에서 특정 콘텐츠를 찾으려면:

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `History`를 클릭합니다
    3. 목록에서 특정 세션 타일을 클릭하여 내용을 봅니다
    4. `Cmd+F`를 사용하여 검색 창을 엽니다
    5. 검색어를 입력합니다
    6. 키보드 단축키와 검색 창 버튼을 사용하여 결과를 탐색합니다

    :::info 정규식 또는 연산자 지원 없음
    검색 텍스트에서 정규식이나 검색 연산자를 사용하는 것은 지원되지 않습니다.
    :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">

    #### 현재 세션 내 검색

    검색 기능은 터미널 인터페이스에서 제공됩니다. 환경에 맞는 적절한 단축키를 사용하세요:

    | 터미널 | 운영 체제 | 단축키 |
    |----------|-----------------|-----------|
    | iTerm2 | macOS | `Cmd+F` |
    | Terminal.app | macOS | `Cmd+F` |
    | Windows Terminal | Windows | `Ctrl+F` |
    | Linux Terminal | Linux | `Ctrl+F` |

    현재 세션 내에서 특정 콘텐츠를 찾으려면:

    1. 단축키를 사용하여 검색 창을 엽니다
    2. 검색어를 입력합니다
    3. 단축키와 검색 창 버튼을 사용하여 결과를 탐색합니다

    :::info
    특정 터미널 에뮬레이터는 다른 키보드 단축키를 사용할 수 있습니다. 검색 명령에 대한 터미널 문서나 설정을 확인하세요.
    :::

    #### 모든 세션 콘텐츠 검색

    모든 세션의 대화 내용을 검색하려면 goose 세션을 시작하고 직접 요청하세요:

    - "지난주 React hooks에 대한 이전 대화 찾기"
    - "데이터베이스 마이그레이션 작업을 한 세션 보여주기"

    goose는 세션 기록을 검색하고 일치하는 세션의 컨텍스트와 함께 관련 대화를 표시합니다.

    :::info
    이 기능을 사용하려면 내장된 [Chatrecall 확장 기능](/docs/mcp/chatrecall-mcp)이 활성화되어 있어야 합니다.
    :::

    #### 세션 데이터 직접 검색

    지원되는 옵션과 함께 [`session list`](/docs/guides/goose-cli-commands#session-list-options) 하위 명령은 일부 검색 작업에 유용할 수 있습니다.

    SQLite 데이터베이스를 직접 쿼리할 수도 있습니다:

    ```bash
    # 세션 설명 검색
    sqlite3 ~/.local/share/goose/sessions/sessions.db \
      "SELECT id, description FROM sessions WHERE description LIKE '%검색어%';"

    # 작업 디렉토리로 검색
    sqlite3 ~/.local/share/goose/sessions/sessions.db \
      "SELECT id, description, working_dir FROM sessions WHERE working_dir LIKE '%프로젝트명%';"

    # 최근 세션 나열
    sqlite3 ~/.local/share/goose/sessions/sessions.db \
      "SELECT id, description, created_at FROM sessions ORDER BY created_at DESC LIMIT 10;"
    ```

    :::info 세션 저장소 마이그레이션
    버전 1.10.0부터 goose는 개별 `.jsonl` 파일 대신 SQLite 데이터베이스(`sessions.db`)를 사용합니다. 레거시 `.jsonl` 파일은 디스크에 남아 있지만 더 이상 goose에서 관리하지 않습니다.
    :::

  </TabItem>
</Tabs>

## 세션 재개

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `History`를 클릭합니다
    3. 재개하려는 세션을 클릭합니다. goose는 세션을 찾는 데 도움이 되는 [검색 기능](#세션-검색)을 제공합니다.
    4. 재개 방법을 선택합니다:
       - 현재 창에서 계속하려면 `Resume`을 클릭합니다
       - 새 창에서 열려면 `New Window`를 클릭합니다

    :::tip
    `Home` 페이지의 `Recent chats` 섹션에서 가장 최근 세션 세 개 중 하나를 클릭하여 빠르게 재개할 수도 있습니다.
    :::

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        가장 최근 세션을 재개하려면 다음 명령을 실행합니다:

        ```
         goose session -r
        ```

        특정 세션을 재개하려면 다음 명령을 실행합니다:

        ```
        goose session -r --name <이름>
        ```
        예를 들어 `react-migration`이라는 세션을 재개하려면 다음을 실행합니다:

        ```
        goose session -r --name react-migration
        ```

        :::tip
        위의 명령으로 세션을 재개할 수 있지만 [무한 루프에 빠지는 문제](/docs/troubleshooting/known-issues#stuck-in-a-loop-or-unresponsive)의 가능성을 줄이기 위해 새 작업에는 새 세션을 만드는 것이 좋습니다.
        :::
    </TabItem>
</Tabs>

### 인터페이스 간 세션 재개

CLI 세션을 데스크톱에서 재개할 수 있습니다.

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
    CLI 세션을 포함한 저장된 모든 세션이 데스크톱 앱에 나열됩니다. 데스크톱 내에서 CLI 세션을 재개하려면:

    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `History`를 클릭합니다
    3. 재개하려는 세션을 클릭합니다
    4. 재개 방법을 선택합니다:
       - 현재 창에서 계속하려면 `Resume`을 클릭합니다
       - 새 창에서 열려면 `New Window`를 클릭합니다

    </TabItem>
    <TabItem value="cli" label="goose CLI">
    현재 CLI 내에서 데스크톱 세션을 재개할 수 없습니다.
    </TabItem>
</Tabs>

### 프로젝트 기반 세션 재개

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        프로젝트 기반 세션은 CLI를 통해서만 사용할 수 있습니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        세션 메타데이터가 있는 추적된 작업 디렉토리인 프로젝트에서 세션을 시작하거나 재개하려면 [`project`](/docs/guides/goose-cli-commands#project) 및 [`projects`](/docs/guides/goose-cli-commands#projects) 명령을 사용할 수 있습니다. 프로젝트 사용에 대한 전체 가이드는 [프로젝트 관리 가이드](/docs/guides/managing-projects)를 참조하세요.
    </TabItem>
</Tabs>

## 세션 삭제

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        데스크톱 앱에서 직접 세션을 삭제할 수 있습니다:

        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
        2. 사이드바에서 `History`를 클릭합니다
        3. 삭제하려는 세션을 찾습니다
        4. 세션 카드 위에 마우스를 올려 작업 버튼을 표시합니다
        5. 나타나는 <Trash2 className="inline" size={16} /> 버튼을 클릭합니다
        6. 나타나는 모달에서 삭제를 확인합니다

        :::warning 영구 삭제
        goose 데스크톱에서 세션을 삭제하면 CLI에서도 삭제됩니다. 이 작업은 취소할 수 없습니다.
        :::

        세션은 세션 기록에서 즉시 제거되고 기본 세션 레코드가 로컬 저장소에서 삭제됩니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        CLI 명령을 사용하여 세션을 제거할 수 있습니다. 세션 제거에 대한 자세한 지침은 [CLI 명령 문서](/docs/guides/goose-cli-commands#session-remove-options)를 참조하세요.
    </TabItem>
</Tabs>

## 세션 가져오기

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        JSON 파일에서 전체 세션을 가져와 goose 인스턴스 간에 세션을 복원, 공유 또는 마이그레이션합니다. 가져오기는 기존 세션을 덮어쓰지 않고 새 ID로 새 세션을 만듭니다.

        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
        2. 사이드바에서 `History`를 클릭합니다
        3. 오른쪽 상단 모서리에 있는 <Upload className="inline" size={16} /> `Import Session` 버튼을 클릭합니다
        4. 이전에 goose에서 내보낸 `.json` 세션 파일을 선택합니다
        5. 세션이 새 세션 ID로 가져옵니다
        6. 성공 알림이 가져오기를 확인합니다

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        세션 가져오기는 현재 데스크톱 앱을 통해서만 사용할 수 있습니다.
    </TabItem>
</Tabs>

## 세션 내보내기

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        백업, 공유, 마이그레이션 또는 보관을 위해 전체 세션을 JSON 파일로 내보냅니다. 내보낸 파일은 대화 기록, 메타데이터 및 설정을 포함한 모든 세션 데이터를 보존합니다.

        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
        2. 사이드바에서 `History`를 클릭합니다
        3. 내보내려는 세션을 찾습니다
        4. 세션 카드 위에 마우스를 올려 작업 버튼을 표시합니다
        5. 나타나는 <Download className="inline" size={16} /> 버튼을 클릭합니다
        6. 세션이 세션 설명에 따라 이름이 지정된 `.json` 파일로 다운로드됩니다

    </TabItem>
    <TabItem value="cli" label="goose CLI">
        백업, 공유, 마이그레이션 또는 문서화 목적으로 세션을 내보냅니다. JSON 파일로 내보내 대화 기록, 메타데이터 및 설정을 포함한 전체 세션 데이터를 보존하거나 Markdown 파일로 내보내 읽기 쉬운 형식의 대화 버전을 얻을 수 있습니다.

        터미널에서 [`session export`](/docs/guides/goose-cli-commands#session-export-options) 하위 명령을 실행합니다:

        ```bash
        goose session export
        ```

    </TabItem>
</Tabs>
