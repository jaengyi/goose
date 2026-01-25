---
title: GitMCP 확장 기능
description: Git MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';


<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/6aV8pinnUS8" />

이 튜토리얼은 [Git MCP 서버](https://github.com/idosal/git-mcp)를 goose 확장 기능으로 추가하여 LLM이 GitHub 저장소에 실시간으로 액세스할 수 있게 하고, 스마트 문서 검색, 코드 탐색 및 정확한 프로젝트 인사이트를 제공하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=mcp-remote&arg=https%3A%2F%2Fgitmcp.io%2Fdocs&id=gitmcp&name=GitMCP&description=Remote%20MCP%20server%20from%20gitmcp.io)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx -y mcp-remote https://gitmcp.io/docs

  ```
  </TabItem>
</Tabs>
:::

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    <GooseDesktopInstaller
        extensionId="Git-mcp"
        extensionName="Git mcp"
        description="최신 문서 및 프로젝트 인사이트를 위한 Git MCP 서버"
        command="npx"
        args={["-y", "mcp-remote" , "https://gitmcp.io/docs"]}
        cliCommand="npx -y mcp-remote https://gitmcp.io/docs"
        timeout={300}
        note="이 명령을 실행하려면 시스템에 Node.js가 설치되어 있어야 합니다(npx 사용)."
    />
 </TabItem>
  <TabItem value="cli" label="goose CLI">
      <CLIExtensionInstructions
        name="Git MCP"
        description="최신 문서 및 프로젝트 인사이트를 위한 Git MCP 서버"
        command="npx -y mcp-remote https://gitmcp.io/docs"
        timeout={300}
      />
  </TabItem>
</Tabs>

## 사용 예시

이 예시에서 goose는 GitMCP를 사용하여 `openai/whisper` GitHub 저장소에서 실시간 문서를 가져오고, 음성-텍스트 모델이 어떻게 작동하는지 탐색하고, 정확한 설정 지침과 명령줄 사용법을 소스에서 직접 표시합니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
   1. goose 데스크톱에서 새 세션을 엽니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. 터미널을 열고 새 goose 세션을 시작합니다:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

### goose 프롬프트
```
  OpenAI Whisper 음성-텍스트 모델이 어떻게 작동하는지 이해하는 데 도움을 줄 수 있나요?

  정확한 정보를 찾기 위해 GitHub 저장소 `openai/whisper`를 사용해주세요.

  다음을 부탁드립니다:
  - 모델이 무엇을 하는지와 주요 기능 요약
  - 명령줄에서 실행하는 방법 예시 표시
  - 시작하는 데 필요한 파일이나 모델 설명
  - 저장소에서 찾은 유용한 코드 스니펫이나 설정 지침 포함

  Markdown 가이드 형식으로 응답해주세요.
```
### goose 출력

:::note 데스크톱

GitHub 저장소를 탐색하여 OpenAI Whisper에 대해 이해하는 데 도움을 드리겠습니다. 문서와 관련 정보를 가져오겠습니다.

`─── owner | repo ──────────────────────────`

특정 설정 및 사용 정보를 검색하겠습니다:

`─── owner | query ──────── repo ────────────`

더 많은 기술 세부 사항과 예시를 검색하겠습니다:

`─── owner | query ──────── repo ────────────`

완벽합니다! 이제 OpenAI Whisper 저장소에서 필요한 모든 정보를 얻었습니다. 포괄적인 Markdown 가이드를 만들어 드리겠습니다.

OpenAI Whisper 음성-텍스트 모델 가이드

개요
Whisper는 OpenAI가 개발한 범용 음성 인식 모델입니다. 다음을 수행할 수 있는 강력한 멀티태스킹 신경망입니다:

- 다국어 음성 인식 (99개 이상의 언어)
- 음성 번역 (외국어 음성을 영어 텍스트로 변환)
- 언어 식별 (어떤 언어가 사용되고 있는지 감지)
- 음성 활동 감지 (음성이 있는 시점 식별)

주요 기능:

- ✅ 견고한 성능: 68만 시간의 다양한 다국어 오디오 데이터로 학습
- ✅ 제로샷 전이: 미세 조정 없이 많은 언어에서 작동
- ✅ 여러 모델 크기: tiny (39M 매개변수)부터 large (1.55B 매개변수)까지
- ✅ 사용하기 쉬움: 간단한 명령줄 인터페이스 및 Python API
- ✅ 오픈 소스: MIT 라이선스로 자유롭게 사용 가능한 모델 가중치

아키텍처
Whisper는 30초 청크로 오디오를 처리하는 Transformer 시퀀스-투-시퀀스 모델을 사용합니다. 오디오를 log-Mel 스펙트로그램으로 변환하고 특수 토큰을 사용하여 작업(전사, 번역, 언어 ID 등)을 지정합니다.

명령줄 사용법
기본 전사

## 시작 체크리스트

1. **✅ Python 3.8-3.11 설치**
2. **✅ FFmpeg 설치** 운영 체제에 맞게
3. **✅ Whisper 설치**: `pip install -U openai-whisper`
4. **✅ 설치 테스트**: `whisper --help`
5. **✅ 기본 전사 시도**: `whisper your_audio_file.mp3`

### 첫 실행

첫 실행 시 Whisper는 모델 가중치를 자동으로 다운로드합니다:
- `tiny`: ~39 MB 다운로드
- `base`: ~142 MB 다운로드
- `small`: ~461 MB 다운로드
- `medium`: ~1.42 GB 다운로드
- `large`: ~2.87 GB 다운로드
- `turbo`: ~1.5 GB 다운로드

모델은 로컬에 캐시되므로 이후 실행은 훨씬 빠릅니다.


## 팁 & 모범 사례

### 🎯 **모델 선택**
- **영어 전용**: `.en` 모델 사용 (`base.en`, `small.en` 등)
- **빠른 전사**: `tiny` 또는 `base` 모델 사용
- **높은 정확도**: `medium` 또는 `large` 모델 사용
- **번역 작업**: `medium` 또는 `large` 사용 (`turbo` 아님)

### 🎵 **오디오 품질**
- 깨끗하고 명확한 오디오가 더 나은 결과 생성
- Whisper는 다양한 오디오 형식 처리 (MP3, WAV, FLAC, M4A 등)
- 배경 소음이 정확도에 영향을 줄 수 있음

### 🌍 **언어 지원**
- 99개 이상의 언어 지원
- 일부 언어는 다른 언어보다 더 잘 수행됨
- 저장소에서 [언어 성능 분석](https://github.com/openai/whisper#available-models-and-languages) 확인

### 💾 **리소스 관리**
- 더 큰 모델은 더 많은 VRAM/RAM 필요
- 모델 선택 시 하드웨어 제한 고려
- 실시간 애플리케이션에는 더 작은 모델 사용

Whisper는 **MIT 라이선스**로 출시되어 개인 및 상업 프로젝트 모두에서 무료로 사용할 수 있습니다.

:::
