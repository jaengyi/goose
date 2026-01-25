---
title: MongoDB 확장 프로그램
description: MongoDB MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

[MongoDB MCP 서버](https://github.com/mongodb-js/mongodb-mcp-server) 확장 프로그램을 사용하면 goose가 MongoDB 데이터베이스와 직접 상호 작용하여 쿼리, 문서 조작, 컬렉션 관리, 데이터베이스 관리를 포함한 포괄적인 데이터베이스 작업을 활성화할 수 있습니다. 이를 통해 자연어 상호 작용을 통해 MongoDB 데이터베이스와 쉽게 작업할 수 있습니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=mongodb-mcp-server&arg=--connection-string&arg=mongodb://localhost:27017&id=mongodb&name=MongoDB&description=MongoDB%20database%20integration)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y mongodb-mcp-server --connection-string mongodb://localhost:27017
  ```
  </TabItem>
</Tabs>
:::

## 연결 사용자 정의

MongoDB MCP 서버는 연결 문자열을 사용하여 단일 MongoDB 데이터베이스 인스턴스에 연결합니다. 연결 문자열은 `--connection-string` 플래그를 사용하여 지정해야 합니다. 여기서는 로컬 MongoDB 인스턴스에 액세스하기 위해 `mongodb://localhost:27017`을 예제로 사용하고 있지만, 자신의 환경에 맞게 구성할 수 있습니다.

MongoDB 연결 문자열은 다음 형식을 따릅니다:
```
mongodb://username:password@hostname:27017/database
```

각 항목:
- `username`: MongoDB 사용자 이름 (로컬 개발에서는 선택 사항)
- `password`: MongoDB 비밀번호 (로컬 개발에서는 선택 사항)
- `hostname`: MongoDB가 실행되는 호스트 (예: localhost, IP 주소 또는 도메인)
- `27017`: 기본 MongoDB 포트 (다른 포트를 사용하는 경우 변경)
- `database`: 데이터베이스 이름 (선택 사항, 기본값에 연결)

예제:
- 로컬 데이터베이스: `mongodb://localhost:27017`
- 자격 증명이 있는 로컬: `mongodb://myuser:mypass@localhost:27017/mydb`
- MongoDB Atlas: `mongodb+srv://user:pass@cluster.mongodb.net/database`

:::caution
자격 증명이 포함된 연결 문자열은 절대 버전 관리에 커밋하지 마세요! 환경 변수나 안전한 구성 관리를 사용하세요. MongoDB Atlas의 경우 IP 주소가 화이트리스트에 있는지 확인하고 강력한 비밀번호를 사용하세요.
:::

## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다. 또한 실행 중인 MongoDB 인스턴스 또는 MongoDB Atlas에 대한 액세스가 필요합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="mongodb"
      extensionName="MongoDB"
      description="MongoDB 데이터베이스 통합"
      command="npx"
      args={["-y", "mongodb-mcp-server", "--connection-string", "mongodb://localhost:27017"]}
    />

    :::info 연결 문자열 구성
    필요한 경우 [MongoDB 환경](#customizing-your-connection)에 맞게 [확장 프로그램을 업데이트](/docs/getting-started/using-extensions#updating-extension-properties)하세요. 예를 들어 `command` 속성의 연결 문자열을 `mongodb://username:password@hostname:27017/database` 형식으로 변경하세요.
    :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="MongoDB"
      description="MongoDB 데이터베이스 통합"
      command="npx -y mongodb-mcp-server --connection-string mongodb://localhost:27017"
      commandNote={
        <>
          <code>mongodb://localhost:27017</code>을 <a href="#customizing-your-connection">MongoDB 환경</a>에 맞는 실제 MongoDB 연결 문자열로 교체하세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 가능한 작업

MongoDB 확장 프로그램은 자연어 상호 작용을 통해 포괄적인 데이터베이스 관리 기능을 제공합니다. 다음을 포함한 다양한 작업을 수행할 수 있습니다:

### 쿼리 작업
- 복잡한 필터와 정렬을 사용한 **문서 찾기**
- MongoDB의 강력한 집계 파이프라인을 사용한 **데이터 집계**
- 선택적 필터링을 사용한 컬렉션의 **문서 수 세기**

### 문서 관리
- 단일 또는 여러 문서 **삽입**
- 지정된 기준으로 하나 또는 여러 문서 **업데이트**
- 특정 문서 또는 대량 **삭제**

### 컬렉션 및 데이터베이스 관리
- 쿼리 성능 최적화를 위한 **인덱스 생성**
- 더 나은 구성을 위한 **컬렉션 이름 변경**
- 더 이상 필요하지 않을 때 **컬렉션 삭제**
- **전체 데이터베이스 삭제** (주의해서 사용하세요!)

### 스키마 및 분석
- 문서 구조를 이해하기 위한 **컬렉션 스키마 분석**
- 탐색을 위한 **데이터베이스 및 컬렉션 나열**
- **컬렉션 인덱스** 및 구성 보기
- **데이터베이스 통계** 및 컬렉션 저장소 크기 확인

원하는 작업을 자연어로 설명하기만 하면 goose가 적절한 MongoDB 작업을 안전하고 효율적으로 실행하도록 도와줍니다.

## 사용 예제

#### goose 프롬프트
```
Update all users in the premium tier who haven't logged in for more than 90 days - set their status to 'inactive' and add a note about when this change was made.
```

#### goose 출력
```
I've successfully updated inactive premium users using updateMany:

db.users.updateMany(
  {
    subscriptionTier: "premium",
    lastLogin: { $lt: new Date(Date.now() - 90 * 24 * 60 * 60 * 1000) }
  },
  {
    $set: {
      status: "inactive",
      inactiveNote: "Auto-updated to inactive on 2025-05-25 due to 90+ days of inactivity",
      lastModified: new Date()
    }
  }
)

Operation Result:
{
  acknowledged: true,
  matchedCount: 23,
  modifiedCount: 23,
  upsertedId: null,
  upsertedCount: 0
}

Updated 23 premium tier users who hadn't logged in since February 24, 2025. Each document now has:
• status: "inactive"
• inactiveNote: explains the automatic status change
• lastModified: timestamp of this operation

Consider sending re-engagement emails to these users or reviewing their subscription status.
```
