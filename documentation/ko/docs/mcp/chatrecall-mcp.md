---
title: Chat Recall 확장 프로그램
description: 모든 goose 세션에서 대화 기록 검색 및 세션 요약 로드
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PlatformExtensionNote } from '@site/src/components/PlatformExtensionNote';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

Chat Recall 확장 프로그램은 세션 기록을 검색하여 goose가 과거 대화를 기억하도록 도와줍니다. 이전 작업의 컨텍스트가 필요할 때 goose가 관련 대화를 검색하거나 특정 세션의 요약을 로드할 수 있습니다.

goose는 과거 작업을 참조하거나 과거 컨텍스트가 필요한 질문을 할 때 자동으로 Chat Recall을 사용합니다.

## 구성

<PlatformExtensionNote defaultEnabled={false} />

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Chatrecall"
    description="모든 세션에서 대화 기록 검색"
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
  │  ● chatrecall
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예제

Chat Recall은 키워드를 사용하여 모든 세션을 검색할 수 있습니다. 결과는 세션별로 그룹화되고 최신순으로 정렬되며 날짜로 필터링할 수 있습니다. 또는 [세션 ID](/docs/guides/goose-cli-commands#session-list-options)를 제공하여 특정 세션의 요약을 로드할 수 있습니다.

goose에게 특정 주제에 대한 최근 대화를 찾아달라고 요청해 보겠습니다:

### goose 프롬프트

```
지난주에 데이터베이스 마이그레이션에 대해 무엇을 논의했나요?
```

### goose 출력

:::note Desktop
최근 세션에서 데이터베이스 마이그레이션에 대한 논의를 찾았습니다. 다음은 논의한 내용입니다:

**데이터베이스 마이그레이션 논의 - 지난주**

#### 세션: 마이그레이션 스크립트 수정 (2024년 11월 26일)
**작업 디렉토리:** /projects/myapp

"column already exists" 오류로 실패하는 프로덕션 데이터베이스 마이그레이션을 디버깅했습니다. 주요 포인트는:

- 마이그레이션이 일부 환경에서 이미 존재하는 컬럼을 추가하려고 했습니다
- 컬럼을 추가하기 전에 컬럼이 존재하는지 확인하는 조건부 로직을 추가했습니다
- 로컬에서 수정 사항을 테스트하고 변경 사항을 커밋했습니다

#### 세션: 데이터베이스 스키마 업데이트 (2024년 11월 27일)
**작업 디렉토리:** /projects/myapp

이메일 인증 기능 추가에 대해 논의했습니다:

- users 테이블에 새 `email_verified` 컬럼 추가 계획
- nullable로 만들지 기본값을 false로 할지 논의
- 새 컬럼에 대한 마이그레이션 스크립트 생성

두 세션 모두 프로덕션에서 데이터베이스 스키마를 안전하게 업데이트하는 데 집중했습니다. 이 세션 중 하나에서 전체 세부 정보를 로드하시겠습니까?
:::

:::info
v1.14.0 이전의 goose 버전에서 압축된 세션은 대화 기록 저장 방식 변경으로 인해 검색할 수 없을 수 있습니다.
:::
