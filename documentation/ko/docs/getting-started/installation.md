---
sidebar_position: 1
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { RateLimits } from '@site/src/components/RateLimits';
import { OnboardingProviderSetup } from '@site/src/components/OnboardingProviderSetup';
import { ModelSelectionTip } from '@site/src/components/ModelSelectionTip';
import MacDesktopInstallButtons from '@site/src/components/MacDesktopInstallButtons';
import WindowsDesktopInstallButtons from '@site/src/components/WindowsDesktopInstallButtons';
import LinuxDesktopInstallButtons from '@site/src/components/LinuxDesktopInstallButtons';
import { PanelLeft } from 'lucide-react';

# goose 설치

<Tabs>
  <TabItem value="mac" label="macOS" default>
    goose 데스크톱 및/또는 CLI 버전 중 설치할 버전을 선택하세요:

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        브라우저에서 직접 또는 [Homebrew](https://brew.sh/)로 goose 데스크톱을 설치하세요.

        <h3 style={{ marginTop: '1rem' }}>옵션 1: 다운로드로 설치</h3>
        <MacDesktopInstallButtons/>

        <div style={{ marginTop: '1rem' }}>
          1. 다운로드한 zip 파일의 압축을 해제합니다.
          2. 실행 파일을 실행하여 goose 데스크톱 애플리케이션을 시작합니다.

          :::tip goose 업데이트
          주기적으로 [goose를 업데이트](/docs/guides/updating-goose)하는 것이 좋습니다.
          :::
        </div>
        <h3>옵션 2: Homebrew로 설치</h3>
        Homebrew는 [동일한 앱](https://github.com/Homebrew/homebrew-cask/blob/master/Casks/b/block-goose.rb)을 다운로드하지만 업데이트도 관리해줍니다.
        ```bash
          brew install --cask block-goose
        ```
        ---
        <div style={{ marginTop: '1rem' }}>
          :::info 권한
          Apple Mac M3에서 goose 데스크톱 앱이 시작 시 창을 표시하지 않으면 다음을 확인하고 업데이트하세요:

          `~/.config` 디렉토리에 읽기 및 쓰기 권한이 있는지 확인하세요.

          goose는 로그 디렉토리와 파일을 생성하기 위해 이 권한이 필요합니다. 권한이 부여되면 앱이 올바르게 로드됩니다. 방법에 대한 단계는 [알려진 문제 가이드](/docs/troubleshooting/known-issues#macos-permission-issues)를 참조하세요.
          :::
        </div>
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        브라우저에서 직접 또는 [Homebrew](https://brew.sh/)로 goose를 설치하세요.

        <h3 style={{ marginTop: '1rem' }}>옵션 1: 다운로드 스크립트로 설치</h3>
        다음 명령어를 실행하여 macOS에 최신 버전의 goose를 설치하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
        ```
        이 스크립트는 최신 버전의 goose를 가져와 시스템에 설정합니다.

        대화형 구성 없이 설치하려면 `CONFIGURE`를 비활성화하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

        :::tip goose 업데이트
        goose를 최신 상태로 유지하는 것이 좋습니다. goose를 업데이트하려면 다음을 실행하세요:
        ```sh
        goose update
        ```
        :::

        <h3>옵션 2: Homebrew로 설치</h3>
        Homebrew는 [사전 컴파일된 CLI 도구](https://github.com/Homebrew/homebrew-core/blob/master/Formula/b/block-goose-cli.rb)를 다운로드하고 업데이트를 관리할 수 있습니다.
        ```bash
        brew install block-goose-cli
        ```
      </TabItem>
    </Tabs>
  </TabItem>

  <TabItem value="linux" label="Linux">
    goose 데스크톱 및/또는 CLI 버전 중 설치할 버전을 선택하세요:

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        브라우저에서 직접 goose 데스크톱을 설치하세요.

        <h3 style={{ marginTop: '1rem' }}>다운로드로 설치</h3>
        <LinuxDesktopInstallButtons/>

        <div style={{ marginTop: '1rem' }}>
          **Debian/Ubuntu 기반 배포판의 경우:**
          1. DEB 파일을 다운로드합니다
          2. 터미널에서 저장된 디렉토리로 이동합니다
          3. `sudo dpkg -i (파일명).deb`를 실행합니다
          4. 앱 메뉴에서 goose를 실행합니다

          :::tip goose 업데이트
          주기적으로 [goose를 업데이트](/docs/guides/updating-goose)하는 것이 좋습니다.
          :::
        </div>
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        다음 명령어를 실행하여 Linux에 goose CLI를 설치하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
        ```
        이 스크립트는 최신 버전의 goose를 가져와 시스템에 설정합니다.

        대화형 구성 없이 설치하려면 `CONFIGURE`를 비활성화하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

        :::tip goose 업데이트
        goose를 최신 상태로 유지하는 것이 좋습니다. goose를 업데이트하려면 다음을 실행하세요:
        ```sh
        goose update
        ```
        :::
      </TabItem>
    </Tabs>
  </TabItem>

  <TabItem value="windows" label="Windows">
    goose 데스크톱 및/또는 CLI 버전 중 설치할 버전을 선택하세요:

    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        브라우저에서 직접 goose 데스크톱을 설치하세요.

        <h3 style={{ marginTop: '1rem' }}>다운로드로 설치</h3>
        <WindowsDesktopInstallButtons/>

        <div style={{ marginTop: '1rem' }}>
          1. 다운로드한 zip 파일의 압축을 해제합니다.
          2. 실행 파일을 실행하여 goose 데스크톱 애플리케이션을 시작합니다.

          :::tip goose 업데이트
          주기적으로 [goose를 업데이트](/docs/guides/updating-goose)하는 것이 좋습니다.
          :::
        </div>
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        Windows에서 goose를 네이티브로 설치하려면 다음 환경 중 하나가 필요합니다:
        - **Git Bash** (권장): [Git for Windows](https://git-scm.com/download/win)와 함께 제공됨
        - **MSYS2**: [msys2.org](https://www.msys2.org/)에서 사용 가능
        - **PowerShell**: Windows 10/11에서 기본 제공

        선택한 환경에서 설치 명령어를 실행하세요:

        ```bash
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
        ```

        대화형 구성 없이 설치하려면 `CONFIGURE`를 비활성화하세요:

        ```bash
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

        **PowerShell 설치:**
        현재 디렉토리에 PowerShell 설치 스크립트를 다운로드합니다.

        ```powershell
        Invoke-WebRequest -Uri "https://raw.githubusercontent.com/block/goose/main/download_cli.ps1" -OutFile "download_cli.ps1";
        ```
        그런 다음 스크립트를 실행하여 goose를 설치합니다:
        ```powershell
        .\download_cli.ps1
        ```

        :::info Windows PATH 설정
        goose가 PATH에 없다는 경고가 표시되면 goose를 PATH에 추가해야 합니다:

        <details>
          <summary>Git Bash/MSYS2의 경우</summary>
          ```bash
          echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
          source ~/.bashrc
          ```
        </details>

        <details>
          <summary>PowerShell의 경우</summary>
          ```powershell
          # PowerShell 프로필에 추가
          $profilePath = $PROFILE
          if (!(Test-Path $profilePath)) { New-Item -Path $profilePath -ItemType File -Force }
          Add-Content -Path $profilePath -Value '$env:PATH = "$env:USERPROFILE\.local\bin;$env:PATH"'
          # 프로필 다시 로드 또는 PowerShell 재시작
          . $PROFILE
          ```
        </details>

        PATH를 업데이트한 후 모든 디렉토리에서 `goose` 명령을 실행할 수 있습니다.
        :::

        <details>
        <summary>Windows Subsystem for Linux (WSL)을 통한 설치</summary>

          goose CLI를 Windows에서 네이티브로 실행하는 것을 권장하지만, Linux와 유사한 환경을 선호하는 경우 WSL을 사용할 수 있습니다.

          1. 관리자 권한으로 [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows)을 열고 WSL과 기본 Ubuntu 배포판을 설치합니다:

          ```bash
          wsl --install
          ```

          2. 메시지가 표시되면 컴퓨터를 다시 시작하여 WSL 설치를 완료합니다. 다시 시작한 후 또는 WSL이 이미 설치되어 있는 경우 다음을 실행하여 Ubuntu 셸을 시작합니다:

          ```bash
          wsl -d Ubuntu
          ```

          3. goose 설치 스크립트를 실행합니다:
          ```bash
          curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | bash
          ```
          :::tip
            다운로드 중 문제가 발생하면 다운로드한 파일을 추출하기 위해 `bzip2`를 설치해야 할 수 있습니다:

            ```bash
            sudo apt update && sudo apt install bzip2 -y
            ```
          :::

          대화형 구성 없이 설치하려면 `CONFIGURE`를 비활성화하세요:

          ```sh
          curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
          ```

          필요한 경우 goose를 경로에 추가합니다:

          ```
          echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
          echo 'export OPENAI_API_KEY=your_api_key' >> ~/.bashrc
          source ~/.bashrc
          ```

        </details>
      </TabItem>
    </Tabs>
  </TabItem>
</Tabs>

## LLM 공급자 설정
goose는 요청을 이해하는 데 필요한 AI 지능을 제공하는 [지원되는 LLM 공급자][providers]와 함께 작동합니다. 처음 사용 시 선호하는 공급자를 구성하라는 메시지가 표시됩니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    시작 화면에서 공급자를 구성하는 방법을 선택하세요:
    <OnboardingProviderSetup />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    CLI는 공급자를 구성하는 방법을 선택할 수 있는 구성 모드에 자동으로 들어갑니다:

    <OnboardingProviderSetup />

    구성 흐름 예시:

    ```
    ┌   goose-configure
    │
    ◇ How would you like to set up your provider?
    │ Tetrate Agent Router Service Login
    │
    Opening browser for Tetrate Agent Router Service authentication...
    [goose가 브라우저를 열고 세부 정보를 출력함]

    Authentication complete!

    Configuring Tetrate Agent Router Service...
    ✓ Tetrate Agent Router Service configuration complete
    ✓ Models configured successfully

    Testing configuration...
    ✓ Configuration test passed!
    ✓ Developer extension enabled!
    └ Tetrate Agent Router Service setup complete! You can now use goose.
  ```

  :::info Windows 사용자
  공급자를 수동으로 구성하는 경우, 구성 중에 키링에 저장하지 않도록 선택하세요. API 키 설정 시 키링 오류가 발생하면 대신 환경 변수를 수동으로 설정할 수 있습니다:

  ```bash
  export OPENAI_API_KEY={your_api_key}
  ```

  그런 다음 `goose configure`를 다시 실행하세요. goose가 환경 변수를 감지하고 다음을 표시합니다:

  ```
  ● OPENAI_API_KEY is set via environment variable
  ```

  세션 간에 API 키를 유지하려면 셸 프로필에 추가하세요:
  ```bash
  echo 'export OPENAI_API_KEY=your_api_key' >> ~/.bashrc
  source ~/.bashrc
  ```
  :::
  </TabItem>
</Tabs>

:::tip
<ModelSelectionTip />
:::

## 공급자 업데이트
언제든지 LLM 공급자 및/또는 모델을 변경하거나 API 키를 업데이트할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
    2. 사이드바에서 `Settings` 버튼을 클릭합니다.
    3. `Models` 탭을 클릭합니다.
    4. 공급자를 업데이트하거나 모델을 전환하거나 `Reset Provider and Model`을 클릭하여 설정을 지우고 시작 화면으로 돌아갑니다. 이러한 [구성 옵션](/docs/getting-started/providers#configure-provider-and-model)에 대한 자세한 내용을 참조하세요.
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    1. 다음 명령어를 실행합니다:
    ```sh
    goose configure
    ```
    2. 메뉴에서 `Configure Providers`를 선택합니다.
    3. 프롬프트에 따라 LLM 공급자를 선택하고 API 키를 입력하거나 업데이트합니다.

    **예시:**

    구성 중에 옵션을 선택하려면 위아래 화살표를 사용하여 선택을 강조 표시한 다음 Enter를 누릅니다.

    ```
    ┌   goose-configure
    │
    ◇ What would you like to configure?
    │ Configure Providers
    │
    ◇ Which model provider should we use?
    │ Google Gemini
    │
    ◇ Provider Google Gemini requires GOOGLE_API_KEY, please enter a value
    │▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪
    │
    ◇ Enter a model from that provider:
    │ gemini-2.0-flash-exp
    │
    ◇  Hello there! You're all set to use me, so please ask away!
    │
    └  Configuration saved successfully
    ```
  </TabItem>
</Tabs>

<RateLimits />

## goose 실행

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        goose 데스크톱에서 세션을 시작하는 것은 간단합니다. 공급자를 선택하면 세션 인터페이스가 사용할 준비가 됩니다.

        입력 필드에 질문, 작업 또는 지침을 직접 입력하면 goose가 즉시 작업을 시작합니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        터미널에서 시작하려는 디렉토리로 이동하고 다음을 실행합니다:
        ```sh
        goose session
        ```
    </TabItem>
</Tabs>

## 공유 구성 설정

goose CLI와 데스크톱 UI는 LLM 공급자 설정, 모델 선택 및 확장 구성을 포함한 모든 핵심 구성을 공유합니다. 어느 인터페이스에서든 확장을 설치하거나 구성하면 설정이 중앙 위치에 저장되어 데스크톱 애플리케이션과 CLI 모두에서 사용할 수 있습니다. 따라서 일관된 설정을 유지하면서 인터페이스 간에 편리하게 전환할 수 있습니다. 자세한 내용은 [구성 파일][config-files] 가이드를 참조하세요.

:::info
핵심 구성은 인터페이스 간에 공유되지만 확장은 인증 자격 증명을 저장하는 방법에 유연성이 있습니다. 일부 확장은 공유 구성 파일을 사용하고 다른 확장은 자체 저장 방법을 구현할 수 있습니다.
:::

<Tabs groupId="interface">
    <TabItem value="ui" label="goose 데스크톱" default>
        공유 구성을 탐색하려면:
        1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
        2. 사이드바에서 `Settings` 버튼을 클릭합니다.
    </TabItem>
    <TabItem value="cli" label="goose CLI">
        공유 구성을 관리하려면 다음 명령어를 사용합니다:
        ```sh
        goose configure
        ```
    </TabItem>
</Tabs>

## 추가 리소스

새 확장 추가 또는 토글을 포함하여 goose의 기능을 확장하도록 확장을 구성할 수도 있습니다. 자세한 지침은 [확장 사용 가이드][using-extensions]를 참조하세요.

[using-extensions]: /docs/getting-started/using-extensions
[providers]: /docs/getting-started/providers
[handling-rate-limits]: /docs/guides/handling-llm-rate-limits-with-goose
[mcp]: https://www.anthropic.com/news/model-context-protocol
[config-files]: /docs/guides/config-files.md
