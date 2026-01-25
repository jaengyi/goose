---
title: goose 빠른 팁
sidebar_position: 30
sidebar_label: 빠른 팁
description: goose 사용 모범 사례
---

### goose는 사용자를 대신하여 작업합니다
goose는 AI 에이전트이므로 애플리케이션 열기, 셸 명령 실행, 워크플로우 자동화, 코드 작성, 웹 브라우징 등의 작업을 수행하도록 goose에게 프롬프트할 수 있습니다.

### 자연어로 goose에게 프롬프트하세요
goose에게 프롬프트하는 데 특별한 언어나 특수 구문이 필요하지 않습니다. 친구에게 말하듯이 goose와 대화하세요. 속어를 사용하거나 부탁과 감사의 말을 해도 goose는 이해합니다.

### goose의 기능을 모든 애플리케이션으로 확장하세요
goose의 기능은 확장 가능합니다. [MCP](https://modelcontextprotocol.io/) 클라이언트로서 goose는 [확장 기능](/extensions)을 통해 앱과 서비스에 연결하여 전체 워크플로우에서 작업할 수 있습니다.

### goose가 가진 제어 수준을 선택하세요
goose에게 필요한 [감독](/docs/guides/goose-permissions) 수준을 사용자 정의할 수 있습니다. 완전한 자율성, 작업 전 승인 필요 또는 작업 없이 단순히 채팅 중에서 선택하세요.

### 올바른 LLM을 선택하세요
goose와의 경험은 [LLM 선택](/blog/2025/03/31/goose-benchmark)에 의해 형성됩니다. LLM이 모든 계획을 처리하고 goose가 실행을 관리하기 때문입니다. LLM을 선택할 때 도구 지원, 특정 기능 및 관련 비용을 고려하세요.

### 세션을 짧게 유지하세요
LLM에는 대화 기록을 얼마나 유지할 수 있는지에 대한 제한인 컨텍스트 윈도우가 있습니다. 초과하면 대화의 이전 부분을 잊을 수 있습니다. 토큰 사용량을 모니터링하고 필요에 따라 [새 세션을 시작](/docs/guides/sessions/session-management)하세요.

### 빠른 런처를 사용하여 더 빠른 세션 시작
`Cmd+Option+Shift+G`(macOS) 또는 `Ctrl+Alt+Shift+G`(Windows/Linux)를 누르고 프롬프트를 보내 즉시 새 세션을 시작하세요.

### 불필요한 확장 기능이나 도구를 끄세요
너무 많은 확장 기능을 켜면 성능이 저하될 수 있습니다. 도구 선택 정확도를 높이고, 컨텍스트 윈도우 공간을 절약하고, 공급자 도구 제한 내에 머물기 위해 필수 [확장 기능 및 도구](/docs/guides/managing-tools/tool-permissions)만 활성화하세요.

:::tip 많은 확장 기능을 위한 코드 실행
필요에 따라 도구를 검색하는 도구 호출의 대안적 접근 방식인 [코드 모드](/docs/guides/managing-tools/code-mode) 활성화를 고려하세요.
:::

### goose에게 선호도를 가르치세요
[`.goosehints` 또는 기타 컨텍스트 파일](/docs/guides/context-engineering/using-goosehints)이나 영구 프로젝트 선호도를 위한 [skills](/docs/guides/context-engineering/using-skills) 및 나중에 goose가 동적으로 기억하길 원하는 것들을 위한 [Memory 확장 기능](/docs/mcp/memory-mcp)을 사용하여 goose가 작업 방식을 기억하도록 도와주세요. 둘 다 선호도를 사용 가능하게 유지하면서 귀중한 컨텍스트 윈도우 공간을 절약하는 데 도움이 됩니다.

### 민감한 파일을 보호하세요
goose는 종종 변경하려고 합니다. [.gooseignore](/docs/guides/using-gooseignore) 파일을 만들어 특정 파일을 변경하지 못하게 할 수 있습니다. 이 파일에서 피하길 원하는 모든 파일 경로를 나열할 수 있습니다.

### 버전 관리
코드 변경 사항을 일찍 그리고 자주 커밋하세요. 이렇게 하면 예상치 못한 변경 사항을 롤백할 수 있습니다.

### goose가 사용할 수 있는 확장 기능을 제어하세요
관리자는 [허용 목록](/docs/guides/allowlist)을 사용하여 goose를 승인된 확장 기능으로만 제한할 수 있습니다. 이는 알려지지 않은 MCP 서버에서 위험한 설치를 방지하는 데 도움이 됩니다.

### 스타터 템플릿 설정
성공적인 세션을 다른 사람과 공유하거나 나중에 다시 사용할 수 있는 재사용 가능한 "[레시피](/docs/guides/recipes/session-recipes)"로 전환할 수 있습니다—처음부터 시작할 필요가 없습니다.

### 실험적 마인드셋을 수용하세요
처음부터 올바르게 할 필요가 없습니다. 프롬프트와 도구를 반복하는 것이 워크플로우의 일부입니다.

### goose를 최신 상태로 유지하세요
최신 기능, 버그 수정 및 성능 개선의 이점을 얻기 위해 정기적으로 goose를 [업데이트](/docs/guides/updating-goose)하세요.

### 두 모델을 페어링하여 비용 절감
[리드/워커 모델](/docs/tutorials/lead-worker/)을 사용하여 goose가 초기 계획에 "리드" 모델을 사용한 다음 실행을 위해 더 저렴한 "워커" 모델에 작업을 전달하게 하세요.

### 레시피를 다시 실행해도 안전하게 만들기
작업하기 전에 현재 상태를 확인하는 [레시피](/docs/guides/recipes/session-recipes)를 작성하여 오류나 중복 없이 여러 번 실행할 수 있게 하세요.

### 레시피에 로깅 추가
무언가 실패할 경우 디버깅과 문제 해결을 쉽게 하기 위해 각 주요 단계에 대한 정보 로그 메시지를 레시피에 포함하세요.
