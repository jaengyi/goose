---
title: Developer 확장 프로그램
description: Developer MCP 서버를 goose 확장 프로그램으로 사용하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';
import { Tornado } from 'lucide-react';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/on_p-LeIrak" />

Developer 확장 프로그램을 사용하면 goose가 파일 편집, 셸 명령 실행, 프로젝트 설정과 같은 개발자 중심 작업을 자동화할 수 있습니다. 또한 [향상된 코드 편집](/docs/guides/enhanced-code-editing) 및 [코드베이스 분석](/docs/guides/codebase-analysis)을 위한 도구도 제공합니다.

이 튜토리얼에서는 goose에 내장된 확장 프로그램인 Developer MCP 서버를 활성화하고 사용하는 방법을 다룹니다.


## 구성

:::info
Developer 확장 프로그램은 goose가 설치되면 기본적으로 이미 활성화되어 있습니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Developer"
    description="파일 편집 및 셸 명령과 같은 개발자 중심 작업 자동화"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Toggle Extensions` 선택
  ```sh
  ┌   goose-configure
  │
  ◇  What would you like to configure?
  │  Toggle Extensions
  │
  ◆  Enable extensions: (use "space" to toggle and "enter" to submit)
  // highlight-start
  │  ● developer
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose가 Express, Mongoose, Nodemon, Dotenv로 JavaScript 개발 환경을 설정하고 Git을 초기화하도록 자동화합니다.

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::


<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   1. goose Desktop에서 새 세션 열기
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. 터미널을 열고 새 goose 세션 시작:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

### goose 프롬프트
```
set up a new JavaScript project with Express, Mongoose, Nodemon, and Dotenv? Fetch the latest package versions, generate a README with those versions, and initialize Git
```

1분 후, goose가 전체 개발 환경 설정을 완료했습니다! 🎉

### goose 출력

```md
I'll help you set up a new JavaScript project with those dependencies. I'll break this down into steps:

1. First, let's create a new directory and initialize the project
2. Then install the dependencies
3. Create a detailed README
4. Initialize git
5. Create some starter files
6. Let's begin:

[... shell commands, files created ...]

Great! I've set up a complete Express project with MongoDB and Dotenv integrations:

1. Created a new project directory and initialized npm
2. Installed Express, Mongoose, and dotenv dependencies
3. set up the basic project structure with directories for:
    - config/
    - models/
    - routes/
    - controllers/
4. Created essential files:
    - index.js (main server file)
    - .env (environment variables)
    - .ignore (with common node.js ignores)
5. Added a basic user model with basic name and email fields
6. Created a user controller with two methods:
    - getUsers (GET all users)
    - createUser (POST new user)
7. Added user routes under `/api/routes`
8. Updated package.json with start scripts
9. Created a comprehensive README.md with:
    - Installation scripts
    - Project structure
    - Available scripts
    - Environment variables
    - Latest package versions

The project structure is:

my-express-app/
├── config/         # Configuration files
├── models/        # MongoDB models
│   └── User.js    # User model
├── routes/        # API routes
│   └── userRoutes.js
├── controllers/   # Route controllers
│   └── userController.js
├── .env           # Environment variables
├── .gitignore     # Git ignore file
├── index.js       # Entry point
└── package.json   # Project dependencies and scripts

To start developing you can:

1. Run `npm install` to install dependencies
2. start the server with `npm run dev`

The API will be available at:
    - `http://localhost:3000/api/users` (GET to get all users)
    - `http://localhost:3000/api/users` (POST to create a new user)

To test out the API you can use tools like postman or curl, for example, to create a new user:

curl x POST http://localhost:3000/api/users \
-H "Content-Type: application/json" \
-d '{"name": "Ebony Louis" , "email": "ebony@example.com"}'

Would you like me to add any additional features or make other improvements to the setup?
```

## 액세스 제어 구성

기본적으로 goose는 사용자 권한으로 시스템 명령을 실행하고 **승인 없이** 접근 가능한 모든 파일을 편집할 수 있습니다. 이는 goose가 기본적으로 자율 권한 모드로 실행되고 Developer 확장 프로그램의 셸 및 파일 편집 도구에 액세스할 수 있기 때문입니다. 이 구성을 통해 goose가 빠르고 독립적으로 작업할 수 있지만, 예상치 못한 결과가 발생할 가능성이 있습니다. 사용 가능한 액세스 제어 기능을 이해하면 goose를 편안함 수준과 특정 요구 사항에 맞게 구성하는 데 도움이 됩니다.

:::tip
goose의 동작에 대한 더 많은 제어를 구성하는 방법에 대해서는 아래의 [빠른 설정 예제](#quick-setup-example)를 참조하세요.
:::

### Developer 확장 프로그램 도구

Developer 확장 프로그램은 다음 도구를 제공합니다:

| 도구 | 설명 | 사용 사례 | 위험 수준 |
|------|-------------|-----------|------------|
| `shell` | 셸 명령 실행 | 테스트 실행, 패키지 설치, git 작업 | ⚠️ 높음<br />사용자 권한으로 모든 시스템 명령 실행 가능 |
| `text_editor` | 파일 읽기, 쓰기, 편집 | 코드 리팩토링, 파일 생성, 구성 업데이트 | ⚠️ 높음<br />접근 가능한 모든 파일 수정 가능 |
| `analyze` | 코드 구조 분석 | 코드베이스 이해, 종속성 찾기 | ✅ 낮음<br />읽기 전용 코드 분석 |
| `screen_capture` | 스크린샷 촬영 | UI 문제 디버깅, 상태 문서화 | ✅ 낮음<br />시각적 정보만 |
| `image_processor` | 이미지 처리 및 크기 조정 | 에셋 최적화, 형식 변환 | ✅ 낮음<br />이미지 조작만 |

### 액세스 제어 기능

위험 허용 범위와 워크플로우에 맞게 여러 제어를 계층화할 수 있습니다:

- **[goose 권한 모드](/docs/guides/goose-permissions)**는 goose가 승인을 요청하는 시기를 제어합니다:

  | 모드 | 설명 | 사용 사례 |
  |------|-------------|-----------|
  | Autonomous<br />CLI: `auto` | 승인 필요 없음 | 안전한 환경에서 경험 많은 사용자에게 적합 |
  | Manual Approval<br />CLI: `approve` | 모든 작업 검토 | 민감한 작업 또는 최대 제어를 원할 때 권장 |
  | Smart Approval<br />CLI: `smart_approve` | AI가 검토 필요 여부 결정 | 균형 잡힌 접근 방식 |
  | Chat Only<br />CLI: `chat` | 모든 도구 비활성화 | 최대 보안 및 도구 호출을 지원하지 않는 모델용 |

- **[도구 권한](/docs/guides/managing-tools/tool-permissions)**을 사용하면 Manual Approval 또는 Smart Approval 모드에서 개별 확장 프로그램 도구에 대해 `Always allow`, `Ask before`, `Never allow` 권한을 설정할 수 있습니다

- **[.gooseignore 파일](/docs/guides/using-gooseignore)**은 goose가 접근할 수 있는 파일과 디렉토리를 제한합니다 (`.gitignore` 파일이 대체)

:::tip 세션 중 모드 변경
재시작 없이 세션 중에 goose 권한 모드를 변경할 수 있습니다:
- **CLI**: `/mode` 명령 사용 (예: `/mode approve`)
- **Desktop**: 하단 메뉴의 <Tornado className="inline" size={16} /> 모드 선택기 버튼 사용
:::

#### 빠른 설정 예제

민감한 시스템으로 작업하거나, 익숙하지 않은 코드베이스를 탐색하거나, 신뢰할 수 없는 모델을 사용하거나, 단순히 실행 전에 작업을 검토하는 것을 선호할 때 goose의 작업에 대한 더 많은 제어가 필요할 수 있습니다.

다음은 감독을 활성화하는 예제 구성입니다:

1. **[권한 모드](/docs/guides/goose-permissions)**를 Smart Approval 또는 Manual Approval로 설정:
   ```yaml
   # ~/.config/goose/config.yaml
   GOOSE_MODE: smart_approve  # 또는 approve
   ```

2. **프로젝트에 [`.gooseignore` 파일](/docs/guides/using-gooseignore) 생성**하여 민감한 파일 보호:
   ```
   .env*
   secrets.*
   *.key
   *.pem
   .git/
   ```

3. **필요에 따라 [도구 권한](/docs/guides/managing-tools/tool-permissions) 구성**

goose의 동작에 더 익숙해지면 환경에 적절한 보호 장치를 유지하면서 마찰을 줄이도록 이러한 설정을 조정할 수 있습니다.

:::info
goose를 안전하게 사용하는 방법에 대한 정보는 [보안 가이드](/docs/guides/security/)도 참조하세요.
:::
