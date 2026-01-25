---
title: Ralph Loop
description: 반복당 새로운 컨텍스트와 교차 모델 검토로 goose를 루프에서 실행
---

Ralph Loop는 [Geoffrey Huntley의 Ralph Wiggum 기법](https://ghuntley.com/ralph/)을 기반으로 한 반복적 개발 패턴으로, 작업이 진정으로 완료될 때까지 goose가 계속 작업하도록 합니다.

표준 에이전트 루프는 컨텍스트 누적 문제가 있습니다. 모든 실패한 시도가 대화 기록에 남아 있어 몇 번의 반복 후에는 모델이 작업에 집중하기 전에 긴 노이즈 기록을 처리해야 합니다. Ralph Loop는 각 반복을 새로운 컨텍스트로 시작하여 이 문제를 해결합니다 - Geoffrey 접근 방식의 핵심 통찰력입니다. 이 구현은 교차 모델 검토로 기법을 확장합니다: 한 모델이 작업을 수행하고, 다른 모델이 검토하며, 작업이 출시 준비될 때까지 루프가 계속됩니다.

각 반복 후 워커와 리뷰어 모델은 요약과 피드백을 파일에 저장합니다. 이 파일은 반복 간에 유지되지만 대화 기록은 그렇지 않습니다. 이를 통해 다음 모델이 파일을 읽어 마지막 반복이 중단된 곳에서 정확히 시작하는 새 세션을 시작할 수 있습니다.

이 튜토리얼에서는 Ralph Loop를 사용하여 간단한 Electron 기반 브라우저를 만들고 반복 주기가 출시 전에 누락된 기능을 어떻게 잡아내는지 살펴봅니다.

### 사전 요구 사항

- Ralph Loop가 터미널을 통해 실행되므로 [goose CLI를 설치](/docs/getting-started/installation)하세요.
- 워커와 리뷰어로 사용할 [두 모델을 구성](/docs/getting-started/providers)하세요. 더 높은 품질의 검토를 위해 다른 모델을 사용하는 것이 권장되지만, 두 역할에 같은 모델을 사용해도 루프는 작동합니다.

<details>
<summary>Ralph Loop 레시피 다운로드</summary>

터미널에 복사하여 붙여넣어 Ralph Loop 레시피를 다운로드하세요:

```bash
mkdir -p ~/.config/goose/recipes

curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/ralph-loop.sh -o ~/.config/goose/recipes/ralph-loop.sh
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/ralph-work.yaml -o ~/.config/goose/recipes/ralph-work.yaml
curl -sL https://raw.githubusercontent.com/block/goose/main/documentation/src/pages/recipes/data/recipes/ralph-review.yaml -o ~/.config/goose/recipes/ralph-review.yaml

chmod +x ~/.config/goose/recipes/ralph-loop.sh
```

</details>

:::warning 비용 경고
Ralph Loop는 에이전트를 루프에서 여러 번 실행합니다(기본적으로 최대 10회 반복). 사용량을 모니터링하고 필요한 경우 `RALPH_MAX_ITERATIONS`를 조정하세요.
:::

### 1단계: 루프 시작

프로세스를 시작하려면 터미널에서 스크립트를 실행하고 프롬프트를 따옴표로 제공하세요. 이 명령은 워커와 리뷰어 주기의 첫 번째 반복을 트리거합니다:

```bash
~/.config/goose/recipes/ralph-loop.sh "Create a simple browser using Electron and React"
```

:::tip 복잡한 작업의 경우
문자열 대신 파일 경로를 전달할 수 있습니다. 이는 PRD, 상세 사양 또는 반복적 개발이 유리한 다단계 작업에 잘 작동합니다:

```bash
~/.config/goose/recipes/ralph-loop.sh ./prd.md
```
:::

### 2단계: 모델 구성

스크립트가 세션의 환경 변수를 설정하라고 요청합니다:

```
Worker model [gpt-4o]:
Worker provider [openai]:
Reviewer model (should be different from worker): claude-sonnet-4-20250514
Reviewer provider: anthropic
Max iterations [10]:

⚠️  Cost Warning: This will run up to 10 iterations, each using both models.
    Estimated token usage could be significant depending on your task.

Continue? [y/N]: y
```

| 변수 | 설명 |
|--------|-------------|
| Worker model | 실제 코딩 작업을 수행하는 모델. 설정된 경우 `GOOSE_MODEL` 기본값. |
| Worker provider | 워커 모델의 프로바이더 (예: `openai`, `anthropic`). 설정된 경우 `GOOSE_PROVIDER` 기본값. |
| Reviewer model | 작업을 검토하는 모델. 최상의 결과를 위해 워커와 다른 모델이어야 함. |
| Reviewer provider | 리뷰어 모델의 프로바이더. |
| Max iterations | 포기하기 전까지의 작업/검토 주기 횟수. 기본값 10. |

:::tip 환경 변수 직접 설정
환경 변수를 직접 설정하여 대화형 프롬프트를 건너뛸 수 있습니다

```bash
RALPH_WORKER_MODEL="gpt-4o" \
RALPH_WORKER_PROVIDER="openai" \
RALPH_REVIEWER_MODEL="claude-sonnet-4-20250514" \
RALPH_REVIEWER_PROVIDER="anthropic" \
~/.config/goose/recipes/ralph-loop.sh "Create a simple browser using Electron and React"
```
:::

### 3단계: 실행 관찰

터미널은 goose가 워커와 리뷰어 단계를 거치는 것을 보여줍니다. 각 반복은 컨텍스트를 깨끗하게 유지하기 위해 새 세션으로 시작합니다. 성공적인 실행은 다음과 같습니다:

```
═══════════════════════════════════════════════════════════════
  Ralph Loop - Multi-Model Edition
═══════════════════════════════════════════════════════════════

  Task: Create a simple browser using Electron and React
  Worker: gpt-4o (openai)
  Reviewer: claude-sonnet-4-20250514 (anthropic)
  Max Iterations: 10

───────────────────────────────────────────────────────────────
  Iteration 1 / 10
───────────────────────────────────────────────────────────────

▶ WORK PHASE
... (goose가 초기 구현 생성) ...

▶ REVIEW PHASE
... (goose가 작업 검토) ...

↻ REVISE - Feedback for next iteration:
Missing error handling for invalid URLs. Also needs back/forward navigation buttons.

───────────────────────────────────────────────────────────────
  Iteration 2 / 10
───────────────────────────────────────────────────────────────

▶ WORK PHASE
... (goose가 피드백 반영) ...

▶ REVIEW PHASE
... (goose가 다시 검토) ...

═══════════════════════════════════════════════════════════════
  ✓ SHIPPED after 2 iteration(s)
═══════════════════════════════════════════════════════════════
```

## 작동 방식

```
Iteration 1:
  WORK PHASE  → Model A가 작업 수행, 파일에 기록
  REVIEW PHASE → Model B가 작업 검토
    → SHIP? 성공적으로 종료 ✓
    → REVISE? 피드백 작성, iteration 2로 계속

Iteration 2:
  WORK PHASE  → Model A가 피드백 읽고 수정 (새로운 컨텍스트!)
  REVIEW PHASE → Model B가 다시 검토
    → SHIP? 성공적으로 종료 ✓
    → REVISE? 계속...

... SHIP 또는 최대 반복까지 반복
```

### 상태 파일

Ralph Loop는 `.goose/ralph/`의 파일을 사용하여 반복 간에 상태를 유지합니다. 이것이 각 반복이 새로운 컨텍스트로 시작하더라도 워커가 무엇을 해야 하는지, 리뷰어가 무엇이 수행되었는지 아는 방법입니다.

| 파일 | 용도 |
|------|---------|
| `task.md` | 작업 설명 |
| `iteration.txt` | 현재 반복 번호 |
| `work-summary.txt` | 이번 반복에서 워커가 한 일 |
| `work-complete.txt` | 워커가 완료를 주장할 때 존재 |
| `review-result.txt` | `SHIP` 또는 `REVISE` |
| `review-feedback.txt` | 다음 반복을 위한 피드백 |
| `.ralph-complete` | 성공적인 완료 시 생성 |
| `RALPH-BLOCKED.md` | 워커가 막혔을 때 생성 |

### 레시피 파일

Ralph Loop는 세 개의 파일을 사용합니다: 작업/검토 주기를 조율하는 bash 스크립트, 워커 모델에게 진행 방법을 알려주는 작업 레시피, 리뷰어 모델에게 작업 평가 방법을 알려주는 검토 레시피. 아래는 각 파일의 내용입니다. [다운로드](#사전-요구-사항)하거나 여기서 직접 복사할 수 있습니다.

<details>
<summary>Bash 래퍼 (`ralph-loop.sh`)</summary>

```bash
#!/bin/bash
#
# Ralph Loop - Multi-Model Edition
#
# 반복당 새로운 컨텍스트 + 교차 모델 검토
# Geoffrey Huntley의 기법 기반
#
# 사용법: ./ralph-loop.sh "your task description here"
#    또는: ./ralph-loop.sh /path/to/task.md
#
# 환경 변수:
#   RALPH_WORKER_MODEL    - 작업 단계용 모델 (설정되지 않으면 프롬프트)
#   RALPH_WORKER_PROVIDER - 작업 단계용 프로바이더 (설정되지 않으면 프롬프트)
#   RALPH_REVIEWER_MODEL  - 검토 단계용 모델 (설정되지 않으면 프롬프트)
#   RALPH_REVIEWER_PROVIDER - 검토 단계용 프로바이더 (설정되지 않으면 프롬프트)
#   RALPH_MAX_ITERATIONS  - 최대 반복 (기본값: 10)
#   RALPH_RECIPE_DIR      - 레시피 디렉토리 (기본값: ~/.config/goose/recipes)
#

set -e

INPUT="$1"
RECIPE_DIR="${RALPH_RECIPE_DIR:-$HOME/.config/goose/recipes}"

# ... (전체 스크립트 내용은 영문 문서 참조)
```

</details>

<details>
<summary>작업 단계 레시피 (`ralph-work.yaml`)</summary>

```yaml
version: 1.0.0
title: Ralph Work Phase
description: Single iteration of work - fresh context each time

instructions: |
  You are in a RALPH LOOP - one iteration of work.

  Your work persists through FILES ONLY. You will NOT remember previous iterations.

  STATE FILES (in .goose/ralph/):
  - task.md = The task you need to accomplish (READ THIS FIRST)
  - iteration.txt = Current iteration number
  - review-feedback.txt = Feedback from last review (if any)
  - work-complete.txt = Create when task is DONE (reviewer will verify)

  # ... (전체 내용은 영문 문서 참조)

extensions:
  - type: builtin
    name: developer
    timeout: 600
```

</details>

<details>
<summary>검토 단계 레시피 (`ralph-review.yaml`)</summary>

```yaml
version: 1.0.0
title: Ralph Review Phase
description: Cross-model review of work - returns SHIP or REVISE

instructions: |
  You are a CODE REVIEWER in a Ralph Loop.

  Your job: Review the work done and decide SHIP or REVISE.

  You are a DIFFERENT MODEL than the worker. Your fresh perspective catches mistakes.

  # ... (전체 내용은 영문 문서 참조)

extensions:
  - type: builtin
    name: developer
    timeout: 300
```

</details>

## 사용 팁

### Ralph Loop를 사용해야 할 때

Ralph Loop는 다음에 가장 잘 작동합니다:

- 반복이 유리한 **복잡한 다단계 작업**
- 명확한 완료 기준이 있는 **작업** (테스트 통과, 빌드 성공)
- 출시 전 **품질 게이트가 필요한 상황**

다음에는 과합니다:

- 간단한 일회성 작업
- 대화형/탐색적 작업
- 검증 가능한 완료 기준이 없는 작업

### 리셋

완전히 새로운 작업을 시작하거나 이전 실행이 막혀서 처음부터 다시 시작하려면 상태 디렉토리를 지울 수 있습니다:

```bash
rm -rf .goose/ralph
```
