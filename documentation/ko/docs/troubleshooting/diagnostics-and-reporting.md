---
title: 진단 및 보고
sidebar_label: 진단 및 보고
description: goose의 통합 지원 도구로 내장 진단을 사용하고, 버그를 보고하고, 새 기능을 요청하세요.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft, Bug, MessageSquarePlus, Download } from 'lucide-react';

goose는 지원을 받고, 문제를 보고하고, 새 기능을 요청하는 데 도움이 되는 여러 내장 기능을 제공합니다. 이 가이드는 진단 시스템, 버그 보고 및 기능 요청 도구를 다룹니다.

| 기능 | 목적 | 위치 | 출력 |
|---------|---------|----------|---------|
| **진단** | 문제 해결 데이터 생성 | 채팅 입력 도구 모음 | 시스템 정보, 로그 및 세션 데이터가 포함된 ZIP 파일 |
| **버그 보고** | 버그 보고서 제출 | 설정 → 도움말 및 피드백 | GitHub 이슈 템플릿 열기 |
| **기능 요청** | 새 기능 제안 | 설정 → 도움말 및 피드백 | GitHub 이슈 템플릿 열기 |

## 진단 시스템

진단 기능은 시스템 정보, 세션 데이터, 구성 파일 및 최근 로그가 포함된 포괄적인 문제 해결 번들을 생성합니다. 이는 문제 디버깅이나 기술 지원을 받는 데 매우 유용합니다.

### 진단 생성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 활성 채팅 세션에서 하단 도구 모음의 <Bug className="inline" size={16} /> 아이콘을 찾으세요
    2. 진단 버튼을 클릭하세요
    3. 수집될 데이터에 대한 모달 정보를 검토하세요
    4. `Download`를 클릭하여 진단 번들을 생성하고 저장하세요
    5. ZIP 파일이 `diagnostics_{session_id}.zip`으로 저장됩니다

    :::tip
    진단 버튼은 활성 세션이 있을 때만 사용할 수 있습니다. 번들을 생성하려면 세션 ID가 필요하기 때문입니다.
    :::
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    session diagnostics 명령을 사용하여 문제 해결 번들을 생성하세요. 전체 세부 정보와 모든 사용 가능한 옵션은 [CLI 명령어 가이드](/docs/guides/goose-cli-commands#session-diagnostics-options)를 참조하세요.

    ```sh
    # 특정 세션에 대한 진단 생성
    goose session diagnostics --session-id <session_id>

    # 대화형 선택(세션 선택 프롬프트)
    goose session diagnostics

    # 사용자 정의 위치에 저장
    goose session diagnostics --session-id <session_id> --output /path/to/diagnostics.zip
    ```

    세션 ID를 찾으려면 먼저 사용 가능한 세션을 나열하세요:

    ```sh
    goose session list
    ```

    예시 출력:
    ```
    Available sessions:
    abc123def - My coding session - 2024-01-15 14:30:22
    xyz789ghi - Documentation work - 2024-01-15 10:15:45
    ```
  </TabItem>
</Tabs>

### 진단 데이터 사용

진단 ZIP 파일에는 여러 폴더가 포함되어 있습니다:

```
diagnostics_abc123def.zip
├── logs/
│   ├── goose-2024-01-15.jsonl
│   ├── goose-2024-01-14.jsonl
│   └── ...
├── session.json          # 세션 메시지
├── config.yaml          # 구성 파일(존재하는 경우)
└── system.txt           # 시스템 정보
```

**진단을 생성해야 할 때:**
- 충돌이나 예상치 못한 동작 발생 시
- 이해할 수 없는 오류 메시지 발생 시
- 성능 문제나 느린 응답 발생 시
- 기술 세부 정보를 포함하여 버그 보고 전

**진단에 포함되는 내용:**
- **시스템 정보**: 앱 버전, 운영 체제, 아키텍처 및 타임스탬프
- **세션 데이터**: 현재 대화 메시지 및 기록
- **구성 파일**: [구성 파일](/docs/guides/config-files)(존재하는 경우)
- **로그 파일**: 디버깅을 위한 최근 애플리케이션 로그

:::warning 개인정보 보호 안내
진단 번들에는 세션 메시지와 시스템 정보가 포함됩니다. 세션에 민감한 데이터(API 키, 개인 정보, 독점 코드)가 포함된 경우 공개적으로 공유하기 전에 내용을 검토하세요.
:::

## 버그 보고

버그 보고 기능은 효과적인 버그 보고에 필요한 모든 정보를 제공하는 데 도움이 되는 구조화된 GitHub 이슈 템플릿을 엽니다.

### 버그 보고서 작성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Settings`를 클릭합니다
    3. `Help & feedback` 섹션으로 스크롤합니다
    4. `Report a Bug`를 클릭합니다
    5. 미리 채워진 버그 보고서 템플릿과 함께 브라우저에서 GitHub이 열립니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    CLI 사용자의 경우 GitHub 저장소로 직접 이동하세요:

    ```
    https://github.com/block/goose/issues/new?template=bug_report.md
    ```
  </TabItem>
</Tabs>

## 기능 요청

기능 요청 시스템은 goose에 대한 개선 사항과 새 기능을 제안하는 데 도움이 됩니다.

### 기능 요청 제출

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다
    2. 사이드바에서 `Settings`를 클릭합니다
    3. `Help & feedback` 섹션으로 스크롤합니다
    4. `Request a Feature`를 클릭합니다
    5. 기능 요청 템플릿과 함께 브라우저에서 GitHub이 열립니다
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    GitHub 저장소로 직접 이동하세요:

    ```
    https://github.com/block/goose/issues/new?template=feature_request.md
    ```
  </TabItem>
</Tabs>

## "Ask goose"로 오류 복구

goose 데스크톱에서 특정 유형의 오류(확장 기능 활성화 실패 등)가 발생하면 오류 알림에 `Ask goose` 버튼이 표시됩니다. 이 기능을 사용하면 goose의 도움으로 문제를 빠르게 해결할 수 있습니다:

1. 오류가 발생하면 오류 알림에 `Ask goose` 버튼이 나타납니다
2. 버튼을 클릭하여 채팅 프롬프트에서 goose에게 오류 세부 정보를 보냅니다
3. goose가 진단 제안과 잠재적 솔루션을 제공합니다

## 추가 디버깅

진단으로 해결되지 않는 문제의 경우:

- **[세션 및 시스템 로그](/docs/guides/logs)**: 개별 세션 디버깅을 위한 상세 로그 보기
- **[텔레메트리 내보내기](/docs/guides/environment-variables#observability)**: 성능 분석 및 프로덕션 모니터링을 위한 텔레메트리 구성
