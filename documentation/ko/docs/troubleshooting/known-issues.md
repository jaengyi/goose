---
title: 알려진 문제
sidebar_label: 알려진 문제
description: 일반적인 goose 문제에 대한 단계별 솔루션이 포함된 포괄적인 문제 해결 가이드.
---

goose는 다른 시스템과 마찬가지로 가끔 문제가 발생할 수 있습니다. 이 가이드는 일반적인 문제에 대한 솔루션을 제공합니다.

:::tip 여기에 나열되지 않은 문제에 대한 도움이 필요하신가요?
[Discord 커뮤니티](https://discord.gg/goose-oss)에서 도움을 받으세요! 가장 빠른 지원을 위해 [진단 보고서](/docs/troubleshooting/diagnostics-and-reporting)를 생성하는 것을 고려하세요 - 설정을 빠르게 이해하는 데 도움이 됩니다.
:::

### goose가 파일을 편집함
goose는 워크플로우의 일부로 파일을 편집할 수 있습니다. 개인 변경 사항을 잃지 않으려면 버전 관리를 사용하여 개인 편집을 스테이징하세요. goose 편집은 검토할 때까지 스테이징하지 않은 상태로 두세요. 필요한 경우 쉽게 되돌릴 수 있도록 goose의 편집을 위한 별도의 커밋을 고려하세요.

---

### goose 중단하기
goose가 잘못된 방향으로 가거나 멈춘 경우 [중단](/docs/guides/sessions/in-session-actions#interrupt-task)하여 작업을 수정하거나 추가 정보를 제공할 수 있습니다.

---

### 루프에 빠지거나 응답 없음
드문 경우에 goose가 "파멸 나선"에 빠지거나 긴 세션 동안 응답하지 않을 수 있습니다. 이는 현재 세션을 종료하고 새 세션을 시작하면 해결되는 경우가 많습니다.

1. `Ctrl+C`를 길게 눌러 취소
2. 새 세션 시작:
  ```sh
  goose session
  ```
:::tip
특히 크거나 복잡한 작업의 경우 더 작은 세션으로 나누는 것을 고려하세요.
:::

---

### 장시간 실행 명령 방지

goose CLI를 사용하고 웹 개발 프로젝트로 작업하는 경우 goose가 무한정 멈추게 하는 명령을 만날 수 있습니다. `npm run dev`, `python -m http.server` 또는 `webpack serve`와 같은 명령은 자체적으로 종료되지 않는 개발 서버를 시작합니다.

goose가 이러한 명령을 실행할 때 다르게 처리하도록 셸을 사용자 정의하여 이러한 문제를 방지할 수 있습니다. `GOOSE_TERMINAL` 환경 변수 사용에 대한 자세한 내용은 [셸 동작 사용자 정의](/docs/guides/environment-variables#customizing-shell-behavior)를 참조하세요.

---



### 컨텍스트 길이 초과 오류

이 오류는 goose에 제공된 입력이 사용 중인 LLM의 최대 토큰 제한을 초과할 때 발생합니다. 이를 해결하려면 입력을 더 작은 부분으로 나누세요. [`.goosehints`][goosehints]를 사용하여 goose에 상세한 컨텍스트를 제공하고 goose 데스크톱에서 [메시지 큐](/docs/guides/sessions/in-session-actions#queue-messages)를 사용할 수도 있습니다.

---

### Ollama 공급자 사용

Ollama는 로컬 LLM을 제공하므로 goose와 함께 이 공급자를 사용하기 전에 먼저 [Ollama를 다운로드하고 모델을 실행](/docs/getting-started/providers#local-llms)해야 합니다. 모델을 다운로드하지 않으면 다음 오류가 발생합니다:

> ExecutionError("error sending request for url (http://localhost:11434/v1/chat/completions)")


또한 DeepSeek 모델은 도구 호출을 지원하지 않으므로 이러한 모델 중 하나를 사용하려면 모든 goose [확장 기능을 비활성화](/docs/getting-started/using-extensions#enablingdisabling-extensions)해야 합니다. 안타깝게도 도구 사용 없이는 DeepSeek을 사용하는 경우 goose가 자율적으로 할 수 있는 일이 많지 않습니다. 그러나 `qwen2.5`와 같은 Ollama의 다른 모델은 도구 호출을 지원하며 goose 확장 기능과 함께 사용할 수 있습니다.

---

### 속도 제한 오류 처리
goose는 LLM 공급자와 상호작용할 때 `429 오류`(속도 제한 초과)를 만날 수 있습니다. 권장되는 솔루션은 내장 속도 제한을 제공하는 공급자를 사용하는 것입니다. 자세한 내용은 [LLM 속도 제한 처리][handling-rate-limits]를 참조하세요.

---

### Hermit 오류

앱에서 확장 기능을 설치할 때 "hermit:fatal"이라는 문제가 표시되면 hermit 캐시를 재설정해야 할 수 있습니다. npx와 uvx가 일관되게 사용 가능하도록 hermit 사본을 사용합니다. 이미 이전 버전의 hermit을 사용한 경우 캐시를 정리해야 할 수 있습니다 - Mac에서 이 캐시는 다음 위치에 있습니다

```
sudo rm -rf ~/Library/Caches/hermit
```

---

### API 오류

사용자는 크레딧이 부족하거나 구성이 잘못된 경우와 같이 LLM API 토큰에 문제가 있을 때 아래와 같은 오류를 만날 수 있습니다:

```sh
Traceback (most recent call last):
  File "/Users/admin/.local/pipx/venvs/goose-ai/lib/python3.13/site-packages/exchange/providers/utils.py",
line 30, in raise_for_status
    response.raise_for_status()
    ~~~~~~~~~~~~~~~~~~~~~~~~~^^
  File "/Users/admin/.local/pipx/venvs/goose-ai/lib/python3.13/site-packages/httpx/_models.py",
line 829, in raise_for_status
    raise HTTPStatusError(message, request=request, response=self)
httpx.HTTPStatusError: Client error '404 Not Found' for url
'https://api.openai.com/v1/chat/completions'

...
```
이 오류는 일반적으로 LLM API 크레딧이 소진되었거나 API 키가 유효하지 않을 때 발생합니다. 이 문제를 해결하려면:

1. API 크레딧 확인:
    - LLM 공급자의 대시보드에 로그인
    - 충분한 크레딧이 있는지 확인. 부족하면 충전
2. API 키 확인:
    - 다음 명령을 실행하여 API 키를 재구성:
    ```sh
    goose configure
    ```
LLM 공급자 업데이트에 대한 자세한 단계는 [설치][installation] 가이드를 참조하세요.

---

### GitHub Copilot 공급자 구성

GitHub Copilot을 공급자로 구성할 때 오류가 발생하면 일반적인 시나리오에 대해 이러한 해결 방법을 시도하세요.

#### 리드/워커 모델의 OAuth 오류

환경에 [리드/워커 모델](/docs/tutorials/lead-worker) 기능이 구성된 경우 GitHub Copilot 설정 중에 다음 오류가 표시될 수 있습니다. 이 기능은 공급자에 연결하기 위한 OAuth 플로우와 충돌합니다.
```
Failed to authenticate: Execution error: OAuth configuration not supported by this provider
```

해결 방법:
1. 메인 구성 파일(`~/.config/goose/config.yaml`)에서 리드/워커 모델 변수를 일시적으로 주석 처리하거나 제거:
   ```yaml
   # GOOSE_LEAD_MODEL: your-model
   # GOOSE_WORKER_MODEL: your-model
   ```
2. `goose configure`를 다시 실행하여 GitHub Copilot 설정
3. OAuth 인증 플로우 완료
4. 필요에 따라 리드/워커 모델 설정을 다시 활성화

#### 컨테이너 및 키링 문제

Docker 컨테이너나 키링 지원이 없는 Linux 환경에서 goose를 실행하는 경우 다음과 같은 키링 오류로 인증이 실패할 수 있습니다:
```
Failed to save token: Failed to access keyring: Platform secure storage failure: DBus error: Using X11 for dbus-daemon autolaunch was disabled at compile time
```

goose는 GitHub 토큰을 안전하게 저장하기 위해 시스템 키링(DBus 및 X11 필요)을 사용하려고 하지만 컨테이너화되거나 헤드리스 환경에서는 사용할 수 없습니다.

해결 방법:

`GOOSE_DISABLE_KEYRING` 환경 변수를 사용하여 goose가 파일에 시크릿을 저장하도록 합니다. 이 예시는 `goose configure` 명령을 실행하는 동안에만 변수를 설정합니다:

```bash
GOOSE_DISABLE_KEYRING=1 goose configure
```

키링 대안에 대한 자세한 내용은 [키체인/키링 오류](#키체인키링-오류)를 참조하세요.

---

### 새 레시피 경고

goose 데스크톱에서 특정 레시피를 처음 실행하면 레시피의 제목, 설명 및 지침을 검토할 수 있는 `New Recipe Warning` 대화 상자가 표시됩니다. 레시피를 신뢰하면 `Trust and Execute`를 클릭하여 계속하세요. 레시피가 변경되지 않는 한 같은 레시피에 대해 다시 프롬프트가 표시되지 않습니다.

이 경고는 잠재적으로 유해한 레시피 코드를 실수로 실행하는 것을 방지하는 데 도움이 됩니다.

---
### goose 제거 또는 캐시된 데이터 제거

goose를 제거하거나 재설치하기 전에 기존 데이터를 지워야 할 수 있습니다. goose는 운영 체제에 따라 다른 위치에 데이터를 저장합니다. API 키와 같은 시크릿은 시스템 키체인/키링에만 저장됩니다.

#### macOS

**데이터 위치**

- **로그 및 구성**: `~/.config/goose`
- **애플리케이션 데이터**: `~/Library/Application Support/Goose`
- **시크릿**: macOS 키체인("goose"라는 자격 증명).

#### 제거 단계

1. 실행 중인 모든 goose 복사본 중지(CLI 또는 GUI)

  - 활동 모니터를 통해 모두 중지했는지 확인하는 것을 고려

2. 키체인 접근을 열고 goose가 저장한 모든 시크릿이 포함된 "goose"라는 자격 증명을 삭제
3. 데이터 디렉토리 제거:

```
rm -rf ~/.config/goose
rm -rf ~/Library/Application\ Support/goose
```
4. 응용 프로그램 폴더에서 "goose" 앱 삭제(goose 데스크톱을 사용하는 경우).

#### Linux
**데이터 위치**

- **데이터/세션**: `~/.local/share/goose/`
- **로그**: `~/.local/state/goose/`
- **구성**: `~/.config/goose/`
- **시크릿**: 시스템 키링(사용 가능한 경우)

#### 제거 단계

- 실행 중인 모든 goose 복사본 중지(CLI 또는 GUI)
- 시스템 키링에서 시크릿 지우기(해당하는 경우)
- 데이터 디렉토리 제거:

```
rm -rf ~/.local/share/goose/
rm -rf ~/.local/state/goose/
rm -rf ~/.config/goose/
```
#### Windows

**데이터 위치**
- **구성 및 데이터**: `%APPDATA%\Block\goose\`
- **로컬 애플리케이션 데이터**: `%LOCALAPPDATA%\Block\goose\`
- **시크릿**: Windows 자격 증명 관리자

#### 제거 단계

1. 실행 중인 모든 goose 복사본 중지(CLI 또는 GUI)

  - 작업 관리자를 확인하여 모든 인스턴스가 닫혔는지 확인

2. Windows 자격 증명 관리자를 열고 "goose"와 관련된 자격 증명 삭제
3. 데이터 디렉토리 제거:
```
rmdir /s /q "%APPDATA%\Block\goose"
rmdir /s /q "%LOCALAPPDATA%\Block\goose"
```
4. 설정 > 앱에서 goose 데스크톱 앱 제거(해당하는 경우)

> 이 정리 후 goose의 새로운 설치를 시도하려면 이제 일반적인 설치 지침부터 시작할 수 있습니다.
---

### 키체인/키링 오류

goose는 시크릿을 저장하기 위해 시스템 키링을 사용하려고 합니다. 키링 지원이 없는 환경에서는 다음과 같은 오류가 표시될 수 있습니다:

```bash
Error Failed to access secure storage (keyring): Platform secure storage failure: DBus error: The name org.freedesktop.secrets was not provided by any .service files
Please check your system keychain and run 'goose configure' again.
If your system is unable to use the keyring, please try setting secret key(s) via environment variables.
```

이 경우 [지원되는 LLM 공급자][configure-llm-provider]에서 찾을 수 있는 공급자별 환경 변수를 설정해야 합니다.

다음 중 하나로 설정할 수 있습니다:
* `export GOOGLE_API_KEY=$YOUR_KEY_HERE` - 세션 동안
* `~/.bashrc` 또는 `~/.zshrc`(또는 동등물)에서 - 새 셸 세션마다 지속되도록

그런 다음 키링에 값을 저장할지 묻는 프롬프트에서 `No` 옵션을 선택하세요.

```bash
$ goose configure

Welcome to goose! Let's get you set up with a provider.
  you can rerun this command later to update your configuration

┌   goose-configure
│
◇  Which model provider should we use?
│  Google Gemini
│
◇  GOOGLE_API_KEY is set via environment variable
│
◇  Would you like to save this value to your keyring?
│  No
│
◇  Enter a model from that provider:
│  gemini-2.0-flash-exp
```

`GOOSE_DISABLE_KEYRING` 환경 변수를 사용할 수도 있습니다. 이는 시크릿 저장을 위한 시스템 키링을 비활성화합니다. 비활성화하려면 아무 값이나 설정하세요(예: "1", "true", "yes"). 실제 값은 중요하지 않으며 변수가 설정되었는지 여부만 중요합니다.

키링이 비활성화되면 시크릿은 다음 위치에 저장됩니다:

* macOS/Linux: `~/.config/goose/secrets.yaml`
* Windows: `%APPDATA%\Block\goose\config\secrets.yaml`

---

### 패키지 러너

많은 외부 확장 기능에는 패키지 러너가 필요합니다. 예를 들어 다음과 같은 오류가 발생하면:

```
Failed to start extension `{extension name}`: Could not run extension command (`{extension command}`): No such file or directory (os error 2)
Please check extension configuration for {extension name}.
```

... 확장 기능이 설치되지 않았을 수 있으며 설치하려면 패키지 러너가 필요하다는 신호입니다.

예를 들어 GitHub 확장 기능의 명령은 `npx -y @modelcontextprotocol/server-github`입니다. `npx`를 사용하므로 이 명령을 실행하려면 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.

---

### Windows에서 Node.js 확장 기능이 활성화되지 않음

Windows에서 Node.js 기반 확장 기능을 활성화하려고 할 때 `Node.js installer script not found` 오류가 발생하면 goose가 예상 시스템 경로에서 Node.js를 찾지 못하기 때문일 수 있습니다.

#### 증상:
- Node.js가 설치되어 있고 작동함(`node -v` 및 `npm -v`로 확인)
- 다른 확장 기능(Python 기반 등)은 정상 작동
- Node.js 확장 기능을 활성화할 때만 오류 발생

#### 솔루션:
이 문제는 일반적으로 Node.js가 비표준 위치에 설치된 경우 발생합니다. goose는 `C:\Program Files\nodejs\`에서 Node.js를 찾을 것으로 예상하지만 다른 곳(예: `D:\Program Files\nodejs\`)에 설치되었을 수 있습니다.

1. **Node.js 설치 경로 확인:**
   ```powershell
   where.exe node
   ```

2. **Node.js가 `C:\Program Files\nodejs\`에 없으면 심볼릭 링크 생성:**
   - 관리자로 PowerShell 열기
   - goose를 실제 Node.js 설치로 리디렉션하는 심볼릭 링크 생성:
   ```powershell
   mklink /D "C:\Program Files\nodejs" "D:\Program Files\nodejs"
   ```
   (`D:\Program Files\nodejs`를 실제 Node.js 설치 경로로 대체)

3. **goose 다시 시작**하고 확장 기능 활성화 재시도.

이렇게 하면 실제 설치를 그대로 유지하면서 goose가 예상 위치에서 Node.js를 찾을 수 있는 심볼릭 링크가 생성됩니다.

---

### 악성 패키지 감지

확장 기능을 사용하려고 할 때 "blocked malicious package"에 대한 오류가 표시되면 확장 기능에서 사용하는 패키지에서 맬웨어가 감지되어 확장 기능이 차단되었음을 의미합니다. 오류 메시지에는 패키지에 대한 세부 정보가 포함됩니다. 예:

```
Blocked malicious package: package-name@1.0.0 (npm). OSV MAL advisories: MAL-2024-1234
```

해결 단계:
1. **대안 찾기**: [확장 기능 디렉토리][extensions-directory] 또는 [PulseMCP](https://www.pulsemcp.com/servers)에서 유사한 확장 기능 찾기
2. **선택적 확인**: 차단된 확장 기능 또는 패키지 이름/게시자의 소스 확인
3. **오탐 보고**: 이것이 오류라고 생각되면 [이슈 열기](https://github.com/block/goose/issues)

이 보안 검사는 PyPI(`uvx`) 또는 NPM(`npx`)을 사용하는 로컬 실행 외부 확장 기능에만 적용됩니다. 검사는 OSV 데이터베이스의 실시간 데이터를 사용합니다. 보안 서비스를 사용할 수 없는 경우 확장 기능은 정상적으로 설치됩니다.

모범 사례로 신뢰할 수 있는 공식 소스의 확장 기능만 설치하세요.

---

### macOS 권한 문제

goose 데스크톱 앱이 실행 시 창이 표시되지 않는 문제가 발생하면 파일 및 폴더 권한 때문일 수 있습니다. 이는 일반적으로 goose가 로그 디렉토리와 파일을 만들기 위해 `~/.config` 디렉토리에 대한 읽기 및 쓰기 액세스가 필요하기 때문에 발생합니다.
마찬가지로 도구가 사용 중에 파일이나 디렉토리를 만들지 못하면 같은 권한 문제로 인해 발생할 수 있습니다.

#### 권한 확인 및 수정 방법:

1. 터미널을 엽니다.
2. ~/.config에 대한 현재 권한을 확인하려면 다음 명령을 실행합니다:
  ```sh
  ls -ld ~/.config
  ```
**예시 출력:**
  ```sh
  drwx------  7 yourusername  staff  224 Jan 15 12:00 /Users/yourusername/.config
  ```
`rwx`는 사용자에게 읽기(r), 쓰기(w) 및 실행(x) 권한이 있음을 나타냅니다. 사용자에게 `rwx`가 표시되지 않으면 아래 단계를 따르세요.

#### 읽기 및 쓰기 권한 부여 방법:

1. 올바른 권한을 추가하려면 다음 명령을 실행합니다:
    ```sh
    chmod u+rw ~/.config
    ```
    ~/.config 디렉토리가 없으면 만든 다음 권한을 할당합니다:
      ```sh
      mkdir -p ~/.config
      chmod u+rw ~/.config
      ```
2. 변경 확인:
    ```sh
    ls -ld ~/.config
    ```

권한을 수정한 후에도 여전히 문제가 발생하면 수퍼유저(관리자) 권한으로 goose를 실행해 보세요:
```sh
sudo /Applications/Goose.app/Contents/MacOS/Goose
```

:::note
sudo로 goose를 실행하면 root가 소유한 파일이 생성될 수 있으며, 이로 인해 추가 권한 문제가 발생할 수 있습니다. 이것은 영구적인 수정이 아닌 문제 해결 단계로 사용하세요.
:::

#### 시스템 설정에서 권한 업데이트(macOS)
1. `시스템 설정` -> `개인정보 보호 및 보안` -> `파일 및 폴더`로 이동
2. goose에 액세스 권한 부여

---

### WSL에서 Ollama 공급자와의 연결 오류

goose에서 Ollama를 공급자로 설정할 때 다음과 같은 오류가 발생하면:
    ```
    Execution error: error sending request for url (http://localhost:11434/v1/chat/completions)
    ```
이는 WSL에서 로컬 호스트 주소에 액세스할 수 없음을 의미할 수 있습니다.
1. 서비스가 실행 중인지 확인:
    ```
    curl http://localhost:11434/api/tags
    ```
    `failed to connect` 오류가 발생하면 WSL이 localhost에 다른 IP를 사용하고 있을 수 있습니다. 이 경우 다음 명령을 실행하여 WSL의 올바른 IP 주소를 찾습니다:
    ```
    ip route show | grep -i default | awk '{ print $3 }'
    ```
2. IP 주소를 얻으면 localhost 대신 goose 구성에서 사용합니다. 예:
    ```
    http://172.24.80.1:11434
    ```

여전히 `failed to connect` 오류가 발생하면 Windows 11 22H2 이상을 사용하는 경우 WSL의 [미러링 네트워킹](https://learn.microsoft.com/en-us/windows/wsl/networking#mirrored-mode-networking) 설정을 사용해 볼 수 있습니다

---

### 에어갭/오프라인 환경 문제

에어갭, 오프라인 또는 기업 제한 환경에서 작업하는 경우 MCP 서버 확장 기능이 활성화되지 않거나 런타임 종속성을 다운로드하지 못하는 문제가 발생할 수 있습니다.

#### 증상:
- 런타임 환경 누락에 대한 오류 메시지와 함께 확장 기능 활성화 실패
- "hermit:fatal" 또는 실패한 인터넷 다운로드를 포함하는 오류
- 개인 컴퓨터에서는 확장 기능이 작동하지만 기업/제한 네트워크에서는 실패
- 다음과 같은 오류 메시지: `Failed to start extension: Could not run extension command`

#### 솔루션:
goose 데스크톱은 Hermit을 통해 런타임 환경을 자동으로 다운로드하는 **"shim"**(패키지된 `npx` 및 `uvx` 버전)을 사용합니다. 제한된 네트워크에서는 이러한 다운로드가 실패합니다.

**해결 방법 - 사용자 정의 명령 이름 사용:**

1. **시스템에 패키지 러너의 대체 이름 버전 생성:**
   ```bash
   # uvx(Python 패키지)용
   ln -s /usr/local/bin/uvx /usr/local/bin/runuv

   # npx(Node.js 패키지)용
   ln -s /usr/local/bin/npx /usr/local/bin/runnpx
   ```

2. **사용자 정의 이름을 사용하도록 MCP 서버 구성 업데이트:**

   대신:
   ```yaml
   extensions:
     example:
       cmd: uvx
       args: [mcp-server-example]
   ```

   사용:
   ```yaml
   extensions:
     example:
       cmd: runuv  # goose의 shim을 우회
       args: [mcp-server-example]
   ```

3. **작동하는 이유:** goose는 알려진 명령 이름(`npx`, `uvx`, `jbang` 등)만 패키지된 shim으로 대체합니다. 사용자 정의 이름은 시스템의 실제 실행 파일로 변경 없이 전달됩니다.

4. **더 많은 변경이 필요한 경우**: 위의 방법이 작동하지 않는 기업 프록시 환경이나 에어갭 환경에서는 네트워크 제약 조건(예: TLS 인증서 제한, [프록시 구성](/docs/guides/environment-variables#network-configuration), 필요한 콘텐츠를 다운로드할 수 없음 등)에 맞게 작동하는 shim/구성으로 goose 데스크톱을 사용자 정의하고 패키징하는 것이 좋습니다.

---
### 추가 도움이 필요하신가요?

여전히 문제가 발생하나요? 도움을 드리겠습니다! goose 팀과 커뮤니티 멤버가 기꺼이 도와드리는 [Discord 커뮤니티][discord]에 가입하세요.

:::tip
질문과 함께 [진단 보고서](/docs/troubleshooting/diagnostics-and-reporting#diagnostics-system)를 공유하면 설정을 이해하고 더 적절한 솔루션을 제공하는 데 도움이 됩니다.
:::



[handling-rate-limits]: /docs/guides/handling-llm-rate-limits-with-goose
[installation]: /docs/getting-started/installation
[discord]: https://discord.gg/goose-oss
[goosehints]: /docs/guides/context-engineering/using-goosehints
[configure-llm-provider]: /docs/getting-started/providers
[extensions-directory]: /extensions
