---
title: 스킬 사용
sidebar_position: 2
sidebar_label: 스킬 사용
---

스킬은 goose에게 특정 작업을 수행하는 방법을 가르치는 재사용 가능한 지침과 리소스 세트입니다. 스킬은 간단한 체크리스트부터 도메인 전문 지식이 포함된 상세한 워크플로우까지 다양하며 스크립트나 템플릿과 같은 지원 파일을 포함할 수 있습니다. 예시 사용 사례에는 배포 절차, 코드 리뷰 체크리스트 및 API 통합 가이드가 포함됩니다.

:::info
이 기능을 사용하려면 내장된 [Skills 확장 기능](/docs/mcp/skills-mcp)이 활성화되어 있어야 합니다 (기본적으로 활성화됨).
:::

세션이 시작되면 goose는 발견한 모든 스킬을 지침에 추가합니다. 세션 중에 goose는 다음과 같은 경우 자동으로 스킬을 로드합니다:
- 요청이 스킬의 목적과 명확하게 일치할 때
- 스킬 사용을 명시적으로 요청할 때, 예를 들어:
  - "code-review 스킬을 사용하여 이 PR을 리뷰해 주세요"
  - "new-service 스킬을 따라 인증 서비스를 설정해 주세요"
  - "deployment 스킬을 적용해 주세요"

사용 가능한 스킬이 무엇인지 goose에게 물어볼 수도 있습니다.

:::info Claude 호환성
goose 스킬은 Claude Desktop 및 [Agent Skills를 지원하는 다른 에이전트](https://agentskills.io/home#adoption)와 호환됩니다.
:::

## 스킬 위치

스킬은 전역적으로 및/또는 프로젝트별로 저장할 수 있습니다. goose는 이러한 모든 디렉토리를 순서대로 확인하고 찾은 것을 결합합니다. 동일한 스킬 이름이 여러 디렉토리에 있으면 나중 디렉토리가 우선합니다:

1. `~/.claude/skills/` — 전역, Claude Desktop과 공유
2. `~/.config/agents/skills/` — 전역, AI 코딩 에이전트 간 이식 가능
3. `~/.config/goose/skills/` — 전역, goose 전용
4. `./.claude/skills/` — 프로젝트 수준, Claude Desktop과 공유
5. `./.goose/skills/` — 프로젝트 수준, goose 전용
6. `./.agents/skills/` — 프로젝트 수준, AI 코딩 에이전트 간 이식 가능

프로젝트 간에 사용하는 워크플로우에는 전역 스킬을 사용합니다. 코드베이스에 고유한 절차에는 프로젝트 수준 스킬을 사용합니다.

## 스킬 만들기

여러 단계, 전문 지식 또는 지원 파일이 포함된 반복 가능한 워크플로우가 있을 때 스킬을 만듭니다.

### 스킬 파일 구조

각 스킬은 `SKILL.md` 파일이 있는 자체 디렉토리에 있습니다:

```
~/.config/agents/skills/
└── code-review/
    └── SKILL.md
```

`SKILL.md` 파일에는 `name`과 `description`이 있는 YAML 프론트매터가 필요하며 그 뒤에 스킬 내용이 옵니다:

```markdown
---
name: code-review
description: Comprehensive code review checklist for pull requests
---

# Code Review Checklist

When reviewing code, check each of these areas:

## Functionality
- [ ] Code does what the PR description claims
- [ ] Edge cases are handled
- [ ] Error handling is appropriate

## Code Quality
- [ ] Follows project style guide
- [ ] No hardcoded values that should be configurable
- [ ] Functions are focused and well-named

## Testing
- [ ] New functionality has tests
- [ ] Tests are meaningful, not just for coverage
- [ ] Existing tests still pass

## Security
- [ ] No credentials or secrets in code
- [ ] User input is validated
- [ ] SQL queries are parameterized
```

### 지원 파일

스킬에는 스크립트, 템플릿 또는 구성 파일과 같은 지원 파일이 포함될 수 있습니다. 스킬 디렉토리에 배치합니다:

```
~/.config/agents/skills/
└── api-setup/
    ├── SKILL.md
    ├── setup.sh
    └── templates/
        └── config.template.json
```

goose가 스킬을 로드하면 지원 파일을 보고 [Developer 확장 기능](/docs/mcp/developer-mcp)의 파일 도구를 사용하여 액세스할 수 있습니다.

<details>
<summary>지원 파일이 있는 스킬 예시</summary>

**SKILL.md:**
```markdown
---
name: api-setup
description: Set up API integration with configuration and helper scripts
---

# API Setup

This skill helps you set up a new API integration with our standard configuration.

## Steps

1. Run `setup.sh <api-name>` to create the integration directory
2. Copy `templates/config.template.json` to your integration directory
3. Update the config with your API credentials
4. Test the connection

## Configuration

The config template includes:
- `api_key`: Your API key (get from the provider's dashboard)
- `endpoint`: API endpoint URL
- `timeout`: Request timeout in seconds (default: 30)

## Verification

After setup, verify:
- [ ] Config file is valid JSON
- [ ] API key is set and not a placeholder
- [ ] Test connection succeeds
```

**setup.sh:**
```bash
#!/bin/bash
API_NAME=$1
mkdir -p "integrations/$API_NAME"
cp templates/config.template.json "integrations/$API_NAME/config.json"
echo "Created integration directory for $API_NAME"
echo "Edit integrations/$API_NAME/config.json with your credentials"
```

**templates/config.template.json:**
```json
{
  "api_key": "YOUR_API_KEY_HERE",
  "endpoint": "https://api.example.com/v1",
  "timeout": 30,
  "retry_attempts": 3
}
```

</details>

## 일반적인 사용 사례 예시

<details>
<summary>배포 워크플로우</summary>

```markdown
---
name: production-deploy
description: Safe deployment procedure for production environment
---

# Production Deployment

## Pre-deployment
1. Ensure all tests pass
2. Get approval from at least 2 reviewers
3. Notify #deployments channel

## Deploy
1. Create release branch from main
2. Run `npm run build:prod`
3. Deploy to staging, verify, then production
4. Monitor error rates for 30 minutes

## Rollback
If error rate exceeds 1%:
1. Revert to previous deployment
2. Notify #incidents channel
3. Create incident report
```

</details>

<details>
<summary>테스트 전략</summary>

```markdown
---
name: testing-strategy
description: Guidelines for writing effective tests in this project
---

# Testing Guidelines

## Unit Tests
- Test one thing per test
- Use descriptive test names: `test_user_creation_fails_with_invalid_email`
- Mock external dependencies

## Integration Tests
- Test API endpoints with realistic data
- Verify database state changes
- Clean up test data after each test

## Running Tests
- `npm test` — Run all tests
- `npm test:unit` — Unit tests only
- `npm test:integration` — Integration tests (requires database)
```

</details>

<details>
<summary>API 통합 가이드</summary>

````markdown
---
name: square-integration
description: How to integrate with our Square account
---

# Square Integration

## Authentication
- Test key: Use `SQUARE_TEST_KEY` from `.env.test`
- Production key: In 1Password under "Square Production"

## Common Operations

### Create a customer
```javascript
const customer = await squareup.customers.create({
  email: user.email,
  metadata: { userId: user.id }
});
```

### Handle webhooks
Always verify webhook signatures. See `src/webhooks/square.js` for our handler pattern.

## Error Handling
- `card_declined`: Show user-friendly message, suggest different payment method
- `rate_limit`: Implement exponential backoff
- `invalid_request`: Log full error, likely a bug in our code
````

</details>

:::tip 재사용을 지원하는 다른 goose 기능
- [.goosehints](/docs/guides/context-engineering/using-goosehints): 일반적인 선호도, 프로젝트 컨텍스트 및 "항상 TypeScript를 사용하라"와 같은 반복되는 지침에 가장 적합
- [레시피](/docs/guides/recipes/session-recipes): 지침, 프롬프트 및 설정을 함께 패키징하는 공유 가능한 구성
:::

## 모범 사례

- **스킬을 집중적으로 유지** — 워크플로우 또는 도메인당 하나의 스킬. 스킬이 길어지면 분할을 고려하세요.
- **명확하게 작성** — 스킬은 goose를 위한 지침입니다. 명확하고 직접적인 언어와 번호가 매겨진 단계를 사용하세요.
- **검증 단계 포함** — goose가 워크플로우가 성공적으로 완료되었는지 확인할 수 있도록 도와주세요.

## 추가 리소스

import ContentCardCarousel from '@site/src/components/ContentCardCarousel';
import skillsvsmcp from '@site/blog/2025-12-22-agent-skills-vs-mcp/skills-vs-mcp.png';

<ContentCardCarousel
  items={[
    {
      type: 'blog',
      title: 'Skills가 MCP를 대체했나요?',
      description: 'Agent Skills 대 MCP 개요',
      thumbnailUrl: skillsvsmcp,
      linkUrl: '/goose/blog/2025/12/22/agent-skills-vs-mcp',
      date: '2025-12-22',
      duration: '4분 읽기'
    }
  ]}
/>
