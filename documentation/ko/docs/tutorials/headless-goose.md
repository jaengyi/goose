---
title: 자동화를 위한 goose 헤드리스 모드 사용
description: goose 헤드리스 모드
---

# goose 헤드리스 모드로 개발 작업 자동화

*CI/CD 파이프라인, 서버 및 배치 처리 시나리오에서 AI 기반 엔지니어링 워크플로우 실행*

인간의 개입 없이 복잡한 엔지니어링 작업을 자동화하는 것은 큰 발전이었지만, AI와 함께 한 단계 더 나아가봅시다. goose의 "헤드리스" 모드를 사용하면 개발자가 대화형 세션이 불가능한 서버 환경, CI/CD 파이프라인 및 배치 처리 시나리오에서 AI 자동화의 전체 기능을 활용할 수 있습니다.

## 헤드리스 모드란?

헤드리스 모드는 goose의 비대화형 실행 환경으로, 인간의 개입이 불가능하거나 원하지 않는 자동화 시나리오를 위해 설계되었습니다.

대화형 데스크톱 앱이나 CLI 세션과 달리 헤드리스 모드는 지침을 처리하고 자동으로 종료되므로 기존 개발 워크플로우에 통합하기에 완벽합니다.

AI 어시스턴트와 대화하는 것과 명확한 지침과 함께 사이드 퀘스트로 보내고 작업을 자율적으로 완료할 것이라고 믿는 것의 차이라고 생각하세요.

## 대화형 vs. 헤드리스: 차이점 이해

| 기능 | 대화형 모드 | 헤드리스 모드 |
|---------|------------------|---------------|
| **사용자 입력** | 사용자 결정 및 설명을 요청 | 기본 동작 및 미리 구성된 설정 사용 |
| **컨텍스트 관리** | 제한에 도달하면 사용자에게 전략 선택 요청 | 자동으로 대화 요약 |
| **세션 지속성** | 진행 중인 대화 상태 유지 | 작업 실행 후 깔끔하게 종료 |
| **오류 처리** | 사용자가 개입하고 지침 제공 가능 | 구성에 따른 자동 오류 응답 |
| **도구 권한** | 위험한 작업에 대한 승인 요청 가능 | 구성된 기본값 사용 또는 안전하게 실패 |
| **실행 흐름** | 대화 스타일의 왕복 | 포괄적인 출력과 함께 단일 실행 |

## 명령 예시와 함께하는 실제 사용 사례

### 1. 서버 환경 및 클라우드 배포

GUI 액세스가 불가능한 헤드리스 서버, 컨테이너화된 환경 및 클라우드 배포에 적합합니다.

```bash
# 자동화된 서버 유지 관리
goose run --with-builtin developer -t "Check system logs for errors in the last 24 hours, identify performance bottlenecks, and generate a maintenance report"

# 컨테이너 최적화
goose run --no-session -t "Analyze the Dockerfile, optimize for smaller image size, and update the build process documentation"

# 클라우드 리소스 감사
goose run -t "Review our AWS infrastructure configuration, identify cost optimization opportunities, and create a migration plan for underutilized resources"
```

### 2. CI/CD 파이프라인 통합

AI 기반 분석 및 수정을 지속적 통합 워크플로우에 원활하게 통합합니다.

```bash
# .github/workflows/ci.yml에서
- name: AI-Powered Code Review
  run: |
    goose run --with-builtin developer \
      -t "Analyze the code changes in this PR, check for security vulnerabilities, performance issues, and suggest improvements. Generate a detailed review report."

# 테스트 실패 분석
goose run --debug -t "Examine the failing test suite, identify the root cause of failures, implement fixes, and ensure all tests pass"

# 자동화된 문서 업데이트
goose run -t "Review code changes and update the README.md and API documentation to reflect new features and modifications"
```

### 3. 배치 처리 및 대량 작업

여러 파일, 리포지토리 또는 시스템에 걸친 대규모 작업을 처리합니다.

```bash
# 대량 코드 현대화
goose run --with-builtin developer \
  -t "Upgrade all Python files in the src/ directory from Python 3.8 to 3.11 syntax, update dependencies, and ensure compatibility"

# 다중 리포지토리 유지 관리
for repo in repo1 repo2 repo3; do
  cd $repo
  goose run --no-session -t "Update all dependencies to latest stable versions, run tests, and create a PR if changes are needed"
  cd ..
done

# 데이터베이스 마이그레이션 자동화
goose run -t "Analyze the current database schema, generate migration scripts for the new requirements, and create rollback procedures"
```

### 4. 예약된 작업 실행

정기적인 자동화 유지 관리를 위해 cron 작업 또는 작업 스케줄러와 결합합니다.

```bash
# 일일 보안 스캔 (crontab에 추가)
0 2 * * * /usr/local/bin/goose run --no-session -t "Run comprehensive security audit, check for vulnerabilities, and email report to security team"

# 주간 의존성 업데이트
0 9 * * 1 /usr/local/bin/goose run -t "Check for outdated dependencies, create update PRs for non-breaking changes, and schedule review for major updates"
```

## 헤드리스 성공을 위한 모범 사례

그러면 이 모든 것을 어떻게 설정할까요?

성공적인 헤드리스 자동화의 기초에 대해 이야기해 봅시다:

### 1. **명확한 지침 작성**

지침은 성공적인 도구의 청사진입니다. 프롬프트 아이디어는 구체적이고 상세하며 모호하지 않아야 합니다. 확실하지 않으면 [바이브 프롬프팅으로 시작](https://www.youtube.com/watch?v=IjXmT0W4f2Q)해 보세요.

```bash
# 좋음: 구체적이고 실행 가능함
goose run -t "Analyze the test failures in the latest CI run, identify the root cause, and create a fix with appropriate unit tests"

# 더 좋음: 훨씬 더 상세함
goose run -t "Review the failed tests in tests/integration/, identify why the authentication middleware is failing, implement a fix that maintains backward compatibility, and add regression tests"
```

### 2. **환경 미리 구성**
반복적인 런타임 결정을 피하기 위해 환경 변수를 설정합니다.

```bash
export GOOSE_CONTEXT_STRATEGY=summarize
export GOOSE_MAX_TURNS=50
export GOOSE_MODE=auto
```

`CONTEXT_STRATEGY` 및 `MAX_TURNS` 설정은 대화 제한을 관리하는 데 도움이 되고, `auto`로 설정된 `GOOSE_MODE`는 비대화형 실행을 허용합니다.

### 3. **강력한 오류 처리 구현**

자동화 스크립트에서 항상 종료 코드를 확인하고 실패를 우아하게 처리합니다:

```bash
#!/bin/bash
if ! goose run --no-session -t "Run security audit and fix critical issues"; then
    echo "goose automation failed - manual intervention required"
    exit 1
fi
```

### 4. **올바른 세션 전략 선택**
세션 기록이 복잡해지는 것을 피하기 위해 일회성 작업에는 `--no-session`을 사용하지만, 나중에 디버깅이 필요할 수 있는 복잡한 다단계 워크플로우(또는 처음 시도할 때)에는 세션을 유지합니다.


## 헤드리스 모드에서 레시피 실행

[레시피](/docs/guides/recipes/)는 재사용 가능하고 매개변수화된 워크플로우를 정의하는 goose의 강력한 방법입니다. 헤드리스 모드에서 레시피는 정교한 자동화 시나리오를 가능하게 하므로 더욱 가치가 있습니다.

### 헤드리스 모드를 위한 레시피 요구 사항

레시피가 헤드리스 모드에서 작동하려면 **반드시** `prompt` 필드를 포함해야 합니다. 이 프롬프트는 자동화 실행을 시작하는 초기 지침 역할을 합니다:

```yaml
# automation-recipe.yaml
title: "Automated Code Quality Check"
name: "Automated Code Quality Check"
description: "Comprehensive code quality analysis and improvement"
author:
  name: "DevOps Team"
  email: "devops@company.com"

# 헤드리스 모드에 필수
prompt: "Perform a comprehensive code quality analysis including linting, security scanning, test coverage analysis, and generate an improvement plan"

instructions: |
  You are an expert code quality engineer. Your task is to:
  1. Run static analysis tools (eslint, pylint, etc.)
  2. Perform security vulnerability scanning
  3. Analyze test coverage and identify gaps
  4. Check for code duplication and complexity issues
  5. Generate a prioritized improvement plan
  6. Create actionable tickets for the development team

parameters:
  - key: target_directory
    input_type: string
    requirement: required
    description: "Directory to analyze"
    default: "./src"
  - key: output_format
    input_type: string
    requirement: required
    description: "Report format (markdown, json, html)"
    default: "markdown"

extensions:
  - type: builtin
    name: developer
    display_name: Developer
    timeout: 300
    bundled: true
```

### 헤드리스 모드에서 레시피 실행

```bash
# 기본 레시피 실행
goose run --recipe automation-recipe.yaml

# 사용자 정의 매개변수 사용
goose run --recipe automation-recipe.yaml \
  --params target_directory=./backend \
  --params output_format=json

# 여러 레시피를 사용한 복잡한 워크플로우
goose run --recipe main-workflow.yaml \
  --sub-recipe security-audit.yaml \
  --sub-recipe performance-analysis.yaml \
  --params environment=production
```

## 제한 사항 이해

헤드리스 모드는 매우 강력하지만 자동화 아이디어에 대한 적절한 기대치를 설정할 수 있도록 제약 사항을 이해하는 것이 중요합니다.

### 1. 사용자 상호 작용 기능 없음

**의미**: goose는 실행 중에 설명, 승인 또는 추가 입력을 요청할 수 없습니다. 무엇을 해야 할지 확실하지 않으면 프롬프트 결과에 일반적으로 "어떻게 진행해야 할까요?"와 같은 질문이 표시됩니다.

**영향**: 지침이 모호하거나 예상치 못한 상황이 발생하면 goose는 사용 가능한 컨텍스트를 기반으로 최선의 판단을 내릴 것이며, 이는 항상 의도와 일치하지 않을 수 있습니다.

**완화**: 특히 문제가 발생한 경우 무엇을 해야 하는지에 대해 매우 상세한 지침을 제공하고, **비프로덕션** 환경에서 자동화를 철저히 테스트합니다.

```bash
# 문제: 너무 모호함
goose run -t "Fix the issues"

# 더 좋음: 구체적이고 실행 가능함
goose run -t "Fix the TypeScript compilation errors in src/components/, ensure all imports are correct, and update any deprecated API calls to use the latest syntax"
```

### 2. 레시피 프롬프트 요구 사항

**의미**: 헤드리스 모드에서 사용되는 모든 레시피는 `prompt` 필드를 포함해야 하며, 그렇지 않으면 오류와 함께 실행이 실패합니다.

**영향**: 기존 대화형 레시피는 자동화 시나리오에서 사용하기 전에 수정이 필요할 수 있습니다.

**완화**: 대화형 사용을 위해 주로 설계되었더라도 레시피에 항상 의미 있는 프롬프트를 포함합니다.

### 3. 도구 권한 의존성

**의미**: goose는 잠재적으로 위험한 도구나 작업을 사용할 권한을 요청할 수 없습니다.

**영향**: 승인이 필요한 작업은 기본 권한을 사용하거나 실패하여 자동화 워크플로우를 차단할 수 있습니다.

**완화**: 환경 변수를 사용하여 도구 권한을 미리 구성합니다:

```bash
export GOOSE_MODE=auto  # 안전한 작업을 자동으로 승인
# 또는 구성에서 특정 도구 권한 구성
```

### 4. 컨텍스트 결정 자동화

**의미**: 대화 컨텍스트 제한에 도달하면 goose는 사용자 입력 없이 구성된 전략을 자동으로 적용합니다.

**영향**: 요약이 완벽하지 않으면 중요한 컨텍스트가 손실될 수 있고, 컨텍스트 정리가 너무 공격적이면 실행이 중단될 수 있습니다.

**완화**: 적절한 컨텍스트 전략을 구성하고 토큰 사용량을 모니터링합니다:

```bash
export GOOSE_CONTEXT_STRATEGY=summarize  # 일반적으로 자동화에 가장 좋은 선택
export GOOSE_MAX_TURNS=100  # 무한 실행 방지
```

### 5. 오류 복구 제한

**의미**: 인간의 통찰력이 도움이 될 복잡한 오류 시나리오는 대화형으로 해결할 수 없습니다.

**영향**: 인간이 대화를 통해 쉽게 해결할 수 있는 엣지 케이스에서 자동화가 실패할 수 있습니다.

**완화**: 호출 스크립트에 포괄적인 오류 처리를 구현하고 폴백 절차를 준비합니다:

```bash
#!/bin/bash
if ! goose run --recipe complex-deployment.yaml; then
    # 더 간단한 접근 방식으로 폴백하거나 운영자에게 알림
    echo "Complex deployment failed, initiating rollback procedure"
    goose run --recipe rollback.yaml
fi
```

## 구성 및 환경 설정

### 필수 환경 변수

```bash
# 컨텍스트 관리
export GOOSE_CONTEXT_STRATEGY=summarize
export GOOSE_MAX_TURNS=50

# 도구 동작
export GOOSE_MODE=auto

# 모델 구성
export GOOSE_PROVIDER=openai
export GOOSE_MODEL=gpt-4o

# 출력 제어
export GOOSE_CLI_MIN_PRIORITY=0.2  # 자세한 출력 줄이기
```

### 고급 구성

```bash
# 다른 모델이 필요한 복잡한 워크플로우용
export GOOSE_LEAD_MODEL=gpt-4o  # 계획용
export GOOSE_WORKER_MODEL=gpt-4o-mini  # 실행용

# 보안 및 권한
export GOOSE_ALLOWLIST=https://company.com/allowed-extensions.json
```

## 자동화된 개발의 미래

goose의 헤드리스 모드는 단순한 기능 이상입니다 -- AI가 구동하는 진정한 자동화 개발 워크플로우로의 전환입니다. 일상적인 작업에서 인간의 개입을 제거하여 팀이 고가치 작업에 집중하는 동안 AI가 우리를 느리게 하는 반복적이고 시간 소모적인 작업을 처리할 수 있습니다.

CI/CD 파이프라인을 간소화하거나, 서버 유지 관리를 자동화하거나, 여러 리포지토리에 걸친 대량 작업을 처리하려는 경우 goose의 헤드리스 모드는 정교하고 안정적인 자동화 워크플로우를 구축하기 위한 기반을 제공합니다.

**오늘 자동화 여정을 시작하세요:**

1. **goose를 설치**하고 환경 변수를 구성합니다
2. 명확한 프롬프트와 상세한 지침으로 **첫 번째 레시피를 만듭니다**
3. 프로덕션에 배포하기 전에 **안전한 환경에서 테스트합니다**
4. **기존 워크플로우와 통합**하고 생산성이 급상승하는 것을 지켜봅니다

헤드리스 모드 성공 사례를 공유하고, 질문하고, AI 자동화의 경계를 넓히고자 하는 다른 개발자들과 협업하려면 [Discord 커뮤니티](https://discord.gg/goose-oss)에 연결하세요.
