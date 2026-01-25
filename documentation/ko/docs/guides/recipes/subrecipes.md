---
sidebar_position: 4
title: 특수 작업을 위한 서브레시피
sidebar_label: 서브레시피
description: 레시피가 서브레시피를 사용하여 특정 작업을 수행하는 방법 알아보기
---

서브레시피는 다른 레시피가 특정 작업을 수행하는 데 사용하는 레시피입니다. 다음을 가능하게 합니다:
- **다단계 워크플로우** - 복잡한 작업을 전문화된 단계로 분리
- **재사용 가능한 구성요소** - 다양한 워크플로우에서 사용할 수 있는 공통 작업 생성

:::warning 실험적 기능
서브레시피는 활발히 개발 중인 실험적 기능입니다. 향후 릴리스에서 동작과 구성이 변경될 수 있습니다.
:::

## 서브레시피 작동 방식

"메인 레시피"는 다음 필드를 포함하는 `sub_recipes` 필드에 서브레시피를 등록합니다:

- `name`: 서브레시피의 고유 식별자, 도구 이름 생성에 사용
- `path`: 서브레시피 파일 경로 (상대 또는 절대)
- `values`: (선택사항) 항상 서브레시피에 전달되는 사전 구성된 매개변수 값

메인 레시피가 실행되면 goose는 각 서브레시피에 대해 다음을 수행하는 도구를 생성합니다:
- 서브레시피가 정의한 매개변수 수락
- 자체 컨텍스트를 가진 별도 세션에서 서브레시피 실행
- 메인 레시피에 출력 반환

서브레시피 세션은 격리되어 실행됩니다 - 대화 기록, 메모리 또는 상태를 메인 레시피 또는 다른 서브레시피와 공유하지 않습니다. 또한 서브레시피는 자체 서브레시피를 정의할 수 없습니다(중첩 불허).

### 매개변수 처리

서브레시피가 받는 매개변수는 `{{ parameter_name }}` 구문을 사용하여 프롬프트와 지침에서 사용할 수 있습니다. 서브레시피는 두 가지 방식으로 매개변수를 받습니다:

1. **사전 설정 값**: `values` 필드에 정의된 고정 매개변수 값이 자동으로 제공되며 런타임에 재정의할 수 없음
2. **컨텍스트 기반 매개변수**: AI 에이전트가 이전 서브레시피의 결과를 포함한 대화 컨텍스트에서 매개변수 값을 추출할 수 있음

사전 설정 값이 컨텍스트 기반 매개변수보다 우선합니다. 대화 컨텍스트와 `values` 필드 모두 동일한 매개변수를 제공하면 `values` 버전이 사용됩니다.

:::tip
다중 줄 매개변수 값을 서브레시피에 전달할 때 유효한 YAML 형식을 유지하려면 `indent()` 필터를 사용하세요. 예: `{{ content | indent(2) }}`. 자세한 내용은 [템플릿 지원](/docs/guides/recipes/recipe-reference#template-support)을 참조하세요.
:::

## 예시

### 순차 처리

이 코드 리뷰 파이프라인 예시는 두 개의 서브레시피를 사용하여 포괄적인 코드 리뷰를 수행하는 메인 레시피를 보여줍니다:

**사용법:**
```bash
goose run --recipe code-review-pipeline.yaml --params repository_path=/path/to/repo
```

**메인 레시피:**

```yaml
# code-review-pipeline.yaml
version: "1.0.0"
title: "Code Review Pipeline"
description: "Automated code review using subrecipes"
instructions: |
  Perform a code review using the available subrecipe tools.
  Run security analysis first, then code quality analysis.

parameters:
  - key: repository_path
    input_type: string
    requirement: required
    description: "Path to the repository to review"

sub_recipes:
  - name: "security_scan"
    path: "./subrecipes/security-analysis.yaml"
    values:
      scan_level: "comprehensive"

  - name: "quality_check"
    path: "./subrecipes/quality-analysis.yaml"

extensions:
  - type: builtin
    name: developer
    timeout: 300
    bundled: true

prompt: |
  Review the code at {{ repository_path }} using the subrecipe tools.
  Run security scan first, then quality analysis.
```

**서브레시피:**

<details>
  <summary>security_scan</summary>
  ```yaml
  # subrecipes/security-analysis.yaml
  version: "1.0.0"
  title: "Security Scanner"
  description: "Analyze code for security vulnerabilities"
  instructions: |
    You are a security expert. Analyze the provided code for security issues.
    Focus on common vulnerabilities like SQL injection, XSS, and authentication flaws.

  parameters:
    - key: repository_path
      input_type: string
      requirement: required
      description: "Path to the code to analyze"

    - key: scan_level
      input_type: string
      requirement: optional
      default: "standard"
      description: "Depth of security scan (basic, standard, comprehensive)"

  extensions:
    - type: builtin
      name: developer
      timeout: 300
      bundled: true

  prompt: |
    Perform a {{ scan_level }} security analysis on the code at {{ repository_path }}.
    Report any security vulnerabilities found with severity levels and recommendations.
  ```
</details>

<details>
  <summary>quality_check</summary>
  ```yaml
  # subrecipes/quality-analysis.yaml
  version: "1.0.0"
  title: "Code Quality Analyzer"
  description: "Analyze code quality and best practices"
  instructions: |
    You are a code quality expert. Review code for maintainability,
    readability, and adherence to best practices.

  parameters:
    - key: repository_path
      input_type: string
      requirement: required
      description: "Path to the code to analyze"

  extensions:
    - type: builtin
      name: developer
      timeout: 300
      bundled: true

  prompt: |
    Analyze the code quality at {{ repository_path }}.
    Check for code smells, complexity issues, and suggest improvements.
  ```
</details>

:::tip
서브레시피가 독립적인 경우 더 빠른 실행을 위해 [서브레시피 병렬 실행](/docs/tutorials/subrecipes-in-parallel)을 참조하여 여러 서브레시피를 동시에 실행하세요.
:::

### 조건부 처리

이 스마트 프로젝트 분석기 예시는 분석에 따라 다른 서브레시피를 선택하는 조건부 로직을 보여줍니다:

**사용법:**
```bash
goose run --recipe smart-analyzer.yaml --params repository_path=/path/to/project
```

**메인 레시피:**

```yaml
# smart-analyzer.yaml
version: "1.0.0"
title: "Smart Project Analyzer"
description: "Analyze project and choose appropriate processing based on type"
instructions: |
  First examine the repository to determine the project type (web app, CLI tool, library, etc.).
  Based on what you find:
  - If it's a web application, use the web_security_audit subrecipe
  - If it's a CLI tool or library, use the api_documentation subrecipe
  Only run one subrecipe based on your analysis.

parameters:
  - key: repository_path
    input_type: string
    requirement: required
    description: "Path to the repository to analyze"

sub_recipes:
  - name: "web_security_audit"
    path: "./subrecipes/web-security.yaml"
    values:
      check_cors: "true"
      check_csrf: "true"

  - name: "api_documentation"
    path: "./subrecipes/api-docs.yaml"
    values:
      format: "markdown"

extensions:
  - type: builtin
    name: developer
    timeout: 300
    bundled: true

prompt: |
  Analyze the project at {{ repository_path }} and determine its type.
  Then run the appropriate subrecipe tool based on your findings.
```

**서브레시피:**

<details>
  <summary>web_security_audit</summary>
  ```yaml
  # subrecipes/web-security.yaml
  version: "1.0.0"
  title: "Web Security Auditor"
  description: "Security audit for web applications"
  instructions: |
    You are a web security specialist. Audit web applications for
    security vulnerabilities specific to web technologies.

  parameters:
    - key: repository_path
      input_type: string
      requirement: required
      description: "Path to the web application code"

    - key: check_cors
      input_type: string
      requirement: optional
      default: "false"
      description: "Whether to check CORS configuration"

    - key: check_csrf
      input_type: string
      requirement: optional
      default: "false"
      description: "Whether to check CSRF protection"

  extensions:
    - type: builtin
      name: developer
      timeout: 300
      bundled: true

  prompt: |
    Perform a web security audit on {{ repository_path }}.
    {% if check_cors == "true" %}Check CORS configuration for security issues.{% endif %}
    {% if check_csrf == "true" %}Verify CSRF protection is properly implemented.{% endif %}
    Focus on web-specific vulnerabilities like XSS, authentication flaws, and session management.
  ```
</details>

<details>
  <summary>api_documentation</summary>
  ```yaml
  # subrecipes/api-docs.yaml
  version: "1.0.0"
  title: "API Documentation Generator"
  description: "Generate documentation for APIs and libraries"
  instructions: |
    You are a technical writer specializing in API documentation.
    Create comprehensive documentation for code libraries and APIs.

  parameters:
    - key: repository_path
      input_type: string
      requirement: required
      description: "Path to the code to document"

    - key: format
      input_type: string
      requirement: optional
      default: "markdown"
      description: "Output format for documentation (markdown, html, rst)"

  extensions:
    - type: builtin
      name: developer
      timeout: 300
      bundled: true

  prompt: |
    Generate {{ format }} documentation for the code at {{ repository_path }}.
    Include API endpoints, function signatures, usage examples, and installation instructions.
    Focus on making it easy for developers to understand and use this code.
  ```
</details>

### 컨텍스트 기반 매개변수 전달

이 여행 플래너 예시는 서브레시피가 이전 서브레시피의 결과를 포함한 대화 컨텍스트에서 매개변수를 받는 방법을 보여줍니다:

**사용법:**
```bash
goose run --recipe travel-planner.yaml
```

**메인 레시피:**

```yaml
# travel-planner.yaml
version: "1.0.0"
title: "Travel Activity Planner"
description: "Get weather data and suggest appropriate activities"
instructions: |
  Plan activities by first getting weather data, then suggesting activities based on conditions.

prompt: |
  Plan activities for Sydney by first getting weather data, then suggesting activities based on the weather conditions we receive.

sub_recipes:
  - name: weather_data
    path: "./subrecipes/weather-data.yaml"
    # 값 없음 - location 매개변수는 프롬프트 컨텍스트에서 가져옴

  - name: activity_suggestions
    path: "./subrecipes/activity-suggestions.yaml"
    # weather_conditions 매개변수는 대화 컨텍스트에서 가져옴

extensions:
  - type: builtin
    name: developer
    timeout: 300
    bundled: true
```

**서브레시피:**

<details>
  <summary>weather_data</summary>
  ```yaml
  # subrecipes/weather-data.yaml
  version: "1.0.0"
  title: "Weather Data Collector"
  description: "Fetch current weather conditions for a location"
  instructions: |
    You are a weather data specialist. Gather current weather information
    including temperature, conditions, and seasonal context.

  parameters:
    - key: location
      input_type: string
      requirement: required
      description: "City or location to get weather data for"

  extensions:
    - type: stdio
      name: weather
      cmd: uvx
      args:
        - mcp_weather@latest
      timeout: 300
      description: "Weather data for trip planning"
    - type: builtin
      name: developer
      timeout: 300
      bundled: true

  prompt: |
    Get the current weather conditions for {{ location }}.
    Include temperature, weather conditions (sunny, rainy, etc.),
    and any relevant seasonal information.
  ```
</details>

<details>
  <summary>activity_suggestions</summary>
  ```yaml
  # subrecipes/activity-suggestions.yaml
  version: "1.0.0"
  title: "Activity Recommender"
  description: "Suggest activities based on weather conditions"
  instructions: |
    You are a travel expert. Recommend appropriate activities and attractions
    based on current weather conditions.

  parameters:
    - key: weather_conditions
      input_type: string
      requirement: required
      description: "Current weather conditions to base recommendations on"

  extensions:
    - type: builtin
      name: developer
      timeout: 300
      bundled: true

  prompt: |
    Based on these weather conditions: {{ weather_conditions }},
    suggest appropriate activities, attractions, and travel tips.
    Include both indoor and outdoor options as relevant.
  ```
</details>

이 예시에서:
- `weather_data` 서브레시피는 프롬프트 컨텍스트에서 위치를 가져옵니다 (AI가 자연어 프롬프트에서 "Sydney"를 추출)
- `activity_suggestions` 서브레시피는 대화 컨텍스트에서 날씨 조건을 가져옵니다 (AI가 첫 번째 서브레시피의 날씨 결과를 사용)

## 모범 사례
- **단일 책임**: 각 서브레시피는 하나의 명확한 목적을 가져야 함
- **명확한 매개변수**: 설명적인 이름과 설명 사용
- **고정 값 사전 설정**: 변경되지 않는 매개변수에 `values` 사용
- **독립적으로 테스트**: 결합하기 전에 서브레시피가 단독으로 작동하는지 확인

## 자세히 알아보기
goose 레시피를 마스터하는 데 도움이 되는 더 많은 문서, 도구 및 리소스는 [레시피](/docs/guides/recipes) 가이드를 확인하세요.
