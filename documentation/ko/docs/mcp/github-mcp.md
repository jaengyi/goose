---
title: GitHub 확장 프로그램
description: GitHub MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/TbmQDv3SQOE" />

이 튜토리얼에서는 파일 작업, 저장소 관리, 검색 기능 등을 활성화하기 위해 [GitHub MCP 서버](https://github.com/github/github-mcp-server)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   [설치 프로그램 실행](goose://extension?type=streamable_http&url=https%3A%2F%2Fapi.githubcopilot.com%2Fmcp%2F&id=github&name=GitHub&description=GitHub%20repository%20management%20and%20operations&header=Authorization%3DBearer%20YOUR_GITHUB_PERSONAL_ACCESS_TOKEN)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  `Remote Extension (Streaming HTTP)` 확장 프로그램 유형을 추가합니다:

  **Endpoint URL**
  ```
  https://api.githubcopilot.com/mcp/
  ```
  </TabItem>
</Tabs>

  **Custom Request Header**
  ```
  Authorization: Bearer <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
  ```
:::

## 구성
이 단계에서는 Remote MCP 서버를 구성합니다. 다른 배포 옵션은 [공식 GitHub MCP 서버 문서](https://github.com/github/github-mcp-server)를 참조하세요.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="github"
      extensionName="GitHub"
      description="GitHub 저장소 관리 및 작업"
      type="http"
      url="https://api.githubcopilot.com/mcp/"
      envVars={[
        { name: "Authorization", label: "Bearer YOUR_GITHUB_PERSONAL_ACCESS_TOKEN" }
      ]}
      apiKeyLink="https://github.com/settings/personal-access-tokens"
      apiKeyLinkText="GitHub Personal Access Token"
    />

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="github"
      description="GitHub 저장소 관리 및 작업"
      type="http"
      url="https://api.githubcopilot.com/mcp/"
      timeout={300}
      envVars={[
        { key: "Authorization", value: "Bearer ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" }
      ]}
      infoNote={
        <>
          <a href="https://github.com/settings/personal-access-tokens" target="_blank" rel="noopener noreferrer">GitHub Personal Access Token</a>을 얻어서 <code>Bearer</code> 토큰으로 붙여넣으세요.
        </>
      }
    />

  </TabItem>
</Tabs>

## 사용 예제

[GitHub Personal Access Token](https://github.com/settings/personal-access-tokens)을 만들 때 다음 액세스 권한을 부여했습니다:
* Repository: [goose-demo](https://github.com/angiejones/goose-demo)
* 메타데이터 읽기 권한
* 코드, 커밋 상태, 이슈, 풀 리퀘스트에 대한 읽기 및 쓰기 권한

### goose 프롬프트
```
create a new branch called hello-world in my angiejones/goose-demo repository. Update the README.md file to say "this was written by goose" and commit it. Open a pull request with your changes.
```

### goose 출력

```
I'll help you create a new branch, update the README, and open a pull request. Let me break this down into steps:

1. First, let's create the new branch called "hello-world" in your repository
2. Then update the README.md file
3. Finally, create a pull request

Let me execute these steps:

1. Creating the new branch:

─── create_branch | github ──────────────────────────
branch: hello-world
owner: angiejones
repo: goose-demo

2. Now, updating the README.md file in the new branch:

─── create_or_update_file | github ──────────────────────────
branch: hello-world
content: this was written by goose
message: Update README.md with goose message
owner: angiejones
path: README.md
repo: goose-demo



3. Finally, creating the pull request:

─── create_pull_request | github ──────────────────────────
base: main
body: ...
head: hello-world
owner: angiejones
repo: goose-demo
title: Update README.md



Great! I've completed all the requested tasks:
1. Created a new branch called "hello-world"
2. Updated the README.md file with the text "this was written by goose"
3. Created a pull request to merge these changes into the main branch

You can view the pull request at: https://github.com/angiejones/goose-demo/pull/1

The pull request is now ready for your review. Would you like me to do anything else with it?
```
