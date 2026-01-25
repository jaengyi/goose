---
sidebar_position: 25
title: goose 업데이트
sidebar_label: goose 업데이트
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { DesktopAutoUpdateSteps } from '@site/src/components/DesktopAutoUpdateSteps';
import MacDesktopInstallButtons from '@site/src/components/MacDesktopInstallButtons';
import WindowsDesktopInstallButtons from '@site/src/components/WindowsDesktopInstallButtons';
import LinuxDesktopInstallButtons from '@site/src/components/LinuxDesktopInstallButtons';

goose CLI와 데스크톱 앱은 활발하고 지속적인 개발 중입니다. 최신 기능과 수정 사항을 얻으려면 다음 지침을 사용하여 goose 클라이언트를 주기적으로 업데이트해야 합니다.

<Tabs>
  <TabItem value="mac" label="macOS" default>
    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        goose를 최신 안정 버전으로 업데이트하세요.

        <DesktopAutoUpdateSteps />

        **수동으로 업데이트를 다운로드하고 설치하려면:**
        1. <MacDesktopInstallButtons/>
        2. 다운로드한 zip 파일 압축 해제
        3. 압축 해제된 `Goose.app` 파일을 `Applications` 폴더로 드래그하여 현재 버전 덮어쓰기
        4. goose 데스크톱 실행

      </TabItem>
      <TabItem value="cli" label="goose CLI">
        다음을 실행하여 goose를 업데이트할 수 있습니다:

        ```sh
        goose update
        ```

        추가 [옵션](/docs/guides/goose-cli-commands#update-options):

        ```sh
        # 최신 카나리(개발) 버전으로 업데이트
        goose update --canary

        # 업데이트 및 설정 재구성
        goose update --reconfigure
        ```

        또는 [설치](/docs/getting-started/installation) 스크립트를 다시 실행할 수 있습니다:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

        현재 goose 버전을 확인하려면 다음 명령을 사용하세요:

        ```sh
        goose --version
        ```
      </TabItem>
    </Tabs>
  </TabItem>

  <TabItem value="linux" label="Linux">
    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        goose를 최신 안정 버전으로 업데이트하세요.

        <DesktopAutoUpdateSteps />

        **수동으로 업데이트를 다운로드하고 설치하려면:**
        1. <LinuxDesktopInstallButtons/>

        #### Debian/Ubuntu 기반 배포판의 경우
        2. 터미널에서 다운로드한 DEB 파일로 이동
        3. `sudo dpkg -i (filename).deb` 실행
        4. 앱 메뉴에서 goose 실행
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        다음을 실행하여 goose를 업데이트할 수 있습니다:

        ```sh
        goose update
        ```

        추가 [옵션](/docs/guides/goose-cli-commands#update-options):

        ```sh
        # 최신 카나리(개발) 버전으로 업데이트
        goose update --canary

        # 업데이트 및 설정 재구성
        goose update --reconfigure
        ```

        또는 [설치](/docs/getting-started/installation) 스크립트를 다시 실행할 수 있습니다:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

        현재 goose 버전을 확인하려면 다음 명령을 사용하세요:

        ```sh
        goose --version
        ```
      </TabItem>
    </Tabs>
  </TabItem>

  <TabItem value="windows" label="Windows">
    <Tabs groupId="interface">
      <TabItem value="ui" label="goose 데스크톱" default>
        goose를 최신 안정 버전으로 업데이트하세요.

        <DesktopAutoUpdateSteps />

        **수동으로 업데이트를 다운로드하고 설치하려면:**
        1. <WindowsDesktopInstallButtons/>
        2. 다운로드한 zip 파일 압축 해제
        3. 실행 파일을 실행하여 goose 데스크톱 앱 시작
      </TabItem>
      <TabItem value="cli" label="goose CLI">
        다음을 실행하여 goose를 업데이트할 수 있습니다:

        ```sh
        goose update
        ```

        추가 [옵션](/docs/guides/goose-cli-commands#update-options):

        ```sh
        # 최신 카나리(개발) 버전으로 업데이트
        goose update --canary

        # 업데이트 및 설정 재구성
        goose update --reconfigure
        ```

        또는 **Git Bash**, **MSYS2** 또는 **PowerShell**에서 [설치](/docs/getting-started/installation) 스크립트를 다시 실행하여 Windows에서 기본적으로 goose CLI를 업데이트할 수 있습니다:

        ```bash
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

        현재 goose 버전을 확인하려면 다음 명령을 사용하세요:

        ```sh
        goose --version
        ```

        <details>
        <summary>Windows Subsystem for Linux(WSL)을 통한 업데이트</summary>

        WSL 설치를 업데이트하려면 `goose update`를 사용하거나 WSL을 통해 설치 스크립트를 다시 실행하세요:

        ```sh
        curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh | CONFIGURE=false bash
        ```

       </details>
      </TabItem>
    </Tabs>
  </TabItem>
</Tabs>
