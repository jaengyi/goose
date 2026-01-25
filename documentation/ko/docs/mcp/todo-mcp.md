---
title: Todo 확장 프로그램
description: 복잡한 작업을 추적 가능한 단계로 분할하여 goose가 체계적으로 유지되도록 도움
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PlatformExtensionNote } from '@site/src/components/PlatformExtensionNote';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';

Todo 확장 프로그램은 복잡한 작업을 일련의 단계로 분할하고 각 단계를 완료할 때마다 업데이트를 제공하여 goose가 체계적으로 유지되도록 도와줍니다.

goose는 여러 파일/컴포넌트와 관련된 작업이나 불확실한 범위의 작업에 대해 자동으로 Todo 확장 프로그램을 사용합니다. 작업 시작 시 goose는 내부 체크리스트를 생성하고, 작업하면서 진행 상황을 읽고 업데이트하며, 모든 작업이 완료되었는지 확인합니다.

:::tip
언제든지 goose에게 "현재 할 일 목록을 보여줘"라고 요청하여 추적 중인 항목을 확인할 수 있습니다.
:::

이 튜토리얼에서는 Todo 확장 프로그램을 활성화하고 사용하는 방법을 다룹니다.

## 구성

<PlatformExtensionNote/>

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseBuiltinInstaller
    extensionName="Todo"
    description="구조화된 체크리스트와 진행 상황 추적으로 복잡한 작업 관리"
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
  │  ● todo
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose가 문서 파일의 서식 불일치를 수정하는 것을 도와줍니다. 이는 체계적인 개선 작업을 수행할 때 Todo 확장 프로그램이 goose가 체계적으로 유지되도록 어떻게 도와주는지 보여줍니다.

### goose 프롬프트

```
두 튜토리얼 파일 간의 서식 불일치를 수정하기 위해 문서를 업데이트해야 합니다. 예상 패턴과 일치하지 않는 여러 섹션이 있습니다. 이러한 문제를 체계적으로 해결하고 진행 상황을 추적하는 것을 도와주실 수 있나요?
```

### goose 출력

```md
문서 불일치를 체계적으로 수정하는 것을 도와드리겠습니다. 체계적인 작업으로 분할하고 진행 상황을 추적하겠습니다.

## 문서 튜토리얼 불일치 수정

### 누락된 컴포넌트 (참조 패턴 대비)
- [x] 상단에 비디오 임베드 추가
- [x] 사용 예제 섹션에 LLM 정보 박스 추가
- [x] 참조 패턴에 없는 추가 섹션 제거

### 구성 섹션 문제
- [x] 참조 형식과 일치하도록 용어 변경
- [x] CLI 구성 섹션 형식 수정
  - [x] 정확한 명령 흐름 표시
  - [x] 구문 강조가 포함된 적절한 메뉴 출력 포함
  - [x] 모호한 지침 제거

### 소개/설명 문제
- [x] 참조 스타일에 맞게 소개 단락 단순화
- [x] 패턴과 일치하도록 튜토리얼 설명 줄 수정
- [x] 전체적으로 대소문자 일관성 수정

### 예제 구조 문제
- [x] 참조 형식에 맞게 예제 단순화
  - [x] 명확한 "Prompt"와 "Output" 섹션 사용
  - [x] 타이밍 정보 추가
  - [x] 적절한 코드 블록에 실제 응답 표시
- [x] 집중적이고 달성 가능한 예제 사용
- [x] 명확한 이전/이후 상태 표시

### 서식 일관성
- [x] 코드 블록이 적절한 구문 강조를 사용하는지 확인
- [x] 참조의 톤과 구조 일치
- [x] 전체적으로 일관된 용어 사용
- [x] 대소문자 문제 수정

## 문서 업데이트 완료! ✅

모든 튜토리얼 불일치가 해결되었습니다:

✅ **구조 정렬** - 참조 패턴과 정확히 일치
✅ **컴포넌트 추가** - 비디오 임베드, 정보 박스, 적절한 탭
✅ **용어 수정** - 전체적으로 일관된 언어
✅ **예제 업데이트** - 간단하고 집중적이며 명확한 타이밍
✅ **CLI 지침** - 강조가 포함된 정확한 명령 흐름
✅ **서식 일관성** - 적절한 코드 블록과 구문

이제 문서가 일관된 패턴을 따르고 사용자에게 명확하고 체계적인 경험을 제공합니다.
```
