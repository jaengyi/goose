---
title: ElevenLabs 확장 프로그램
description: ElevenLabs MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/1Z8XtjQ9El0" />


이 튜토리얼에서는 AI 기반 음성 생성, 음성 복제, 오디오 편집, 음성-텍스트 변환 기능을 활성화하기 위해 [ElevenLabs MCP 서버](https://github.com/elevenlabs/elevenlabs-mcp)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=elevenlabs-mcp&id=elevenlabs&name=ElevenLabs&description=ElevenLabs%20voice%20synthesis%20server&env=ELEVENLABS_API_KEY)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  uvx elevenlabs-mcp
  ```
  </TabItem>
</Tabs>

  **환경 변수**
  ```
  ELEVENLABS_API_KEY: <YOUR_API_KEY>
  ```
:::

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="elevenlabs"
      extensionName="ElevenLabs"
      description="ElevenLabs 음성 합성 서버"
      command="uvx"
      args={["elevenlabs-mcp"]}
      envVars={[
        { name: "ELEVENLABS_API_KEY", label: "ElevenLabs API Key" }
      ]}
      apiKeyLink="https://elevenlabs.io/app/settings/api-keys"
      apiKeyLinkText="ElevenLabs API Key"
    />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="ElevenLabs"
      description="ElevenLabs 음성 합성 서버"
      command="uvx elevenlabs-mcp"
      envVars={[
        { key: "ELEVENLABS_API_KEY", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          <a href="https://elevenlabs.io/app/settings/api-keys" target="_blank" rel="noopener noreferrer">ElevenLabs API Key</a>를 얻어서 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose와 ElevenLabs 확장 프로그램을 사용하여 YouTube Short용 AI 생성 나레이션을 만드는 방법을 보여드리겠습니다. goose가 제공한 샘플 스크립트를 가져와 다른 AI 음성을 사용하여 나레이션 버전을 생성하고, 콘텐츠 흐름에 맞게 스크립트 중간에 톤을 자연스럽게 전환합니다.

ElevenLabs MCP 서버에 연결하면 goose가 일반 텍스트를 자연스러운 소리의 음성으로 변환하여 여러 음성 스타일과 캐릭터 옵션을 제공합니다. 수동 녹음이나 편집 없이 모두 가능합니다.

### goose 프롬프트

> Hey goose, create a script for me for my youtube short video, I want there to be two different voices. The first voice should cut me off and be a human narrator style and then switch to a cassual AI tone after I read the prompt. Here's an example of a YT short script I've done in the past:

Wait… Within Seconds, goose performed Security Audits Across Multiple Projects?! 🔥

Lets, plug & play to find out how

Let's provide goose with the command it needs to connect to the Filesystem MCP server extension…

Now lets play
propmt:  "Hey goose, I need to perform a security audit across multiple projects. Let's check for…🔹 Hardcoded Credentials – API keys, passwords, and secrets left in the code.🔹 SQL Injection Risks – Unsafe queries that could expose data.🔹 Insecure Cryptographic Practices – Weak encryption methods that put data at risk.AND🔹 Exposed Config Files – Sensitive information that shouldn't be public.🔹 Outdated Dependencies – Security vulnerabilities in third-party libraries."

Go goose, go goose!

✅ goose scanned the entire codebase across 3 different projects, identified security risks, generated a detailed report with fixes and provided me with step by step instructions on how I can test and verify these code fixes!

If that's not amazing idk what is …

🚀 to get started visit block.github.io/goose_


### goose 출력

:::note Desktop

I'll create your YouTube script for you using the given script as reference.

───────────── Text To Speech ─────────────

───────────── Text To Speech ─────────────

───────────── Text To Speech ─────────────

───────────── Text To Speech ─────────────

───────────── Text To Speech ─────────────

───────────── Play Audio  ────────────────

───────────── Play Audio  ────────────────

The script has been created and read aloud using the specified voices and style. The audio files have been saved to your desktop.

Press play and hear it for yourself! 🔊

<audio controls>
  <source src="/goose/audio/elevenlabs-mcp-demo.mp3" type="audio/mpeg" />
  Your browser does not support the audio element.
</audio>

:::
