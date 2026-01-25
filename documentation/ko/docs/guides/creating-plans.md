---
sidebar_position: 87
title: 작업 전 계획 세우기
sidebar_label: 계획 만들기
---

import Card from '@site/src/components/Card';
import styles from '@site/src/components/Card/styles.module.css';

명확한 계획 없이 프로젝트를 시작하는 것은 설계도 없이 집을 짓는 것과 같습니다. 다음과 같은 문제가 발생할 수 있습니다:

* 무엇을 해야 할지 혼란
* 시간과 노력 낭비
* 범위가 너무 커지는 프로젝트

좋은 계획은 모든 사람이 올바른 방향으로 가도록 하고 진행 상황을 측정하는 데 도움이 됩니다. 그래서 goose CLI에는 프로젝트를 명확하고 관리 가능한 단계로 분해하는 데 도움이 되는 `/plan` 프롬프트 완성 명령이 포함되어 있습니다.

:::tip goose 데스크톱에서의 계획
goose 데스크톱에는 `plan` 키워드가 없습니다. goose 데스크톱에서 계획을 세우려면 다음과 같은 프롬프트를 사용해야 합니다:

```
"goose야, CLI 프로젝트를 각 CLI 명령에 대한 입력 필드를 제공하는 로컬 호스팅 웹 페이지로 변환하는 계획을 만들어줄 수 있어? 실제 작업은 시작하지 마"
```
"계획을 세워줘"라고 요청하지 않으면 goose가 바로 프로젝트 작업을 시작할 수 있습니다.
:::

goose CLI의 계획 모드는 대화형으로, 계획을 세우기 전에 프로젝트를 이해하기 위해 명확한 질문을 합니다. 이러한 질문에 사려 깊고 유익한 답변을 제공하면 goose는 정말 유용하고 실행 가능한 계획을 생성할 수 있습니다.

## 플래너 공급자 및 모델 설정
일부 워크플로우에서는 계획에 하나의 LLM을 사용하고 실행에는 다른 LLM을 사용하는 것이 도움이 될 수 있습니다. 예를 들어, GPT-4.1은 전략적 계획과 복잡한 작업을 명확하고 논리적인 단계로 분해하는 데 뛰어난 경향이 있습니다. 반면 Claude Sonnet 3.5는 깔끔하고 효율적인 코드를 작성하고 지시를 정확하게 따르는 데 특히 강합니다. GPT-4.1을 계획에 사용하고 Claude를 실행에 사용하면 두 모델의 장점을 살려 더 나은 결과를 얻을 수 있습니다.

goose CLI 계획 모드는 두 가지 구성 값을 사용합니다:

- `GOOSE_PLANNER_PROVIDER`: 계획에 사용할 공급자
- `GOOSE_PLANNER_MODEL`: 계획에 사용할 모델

:::tip 계획 모드의 다중 모델 대안
goose는 또한 [리드/워커 모드](/docs/guides/environment-variables#leadworker-model-configuration)를 통한 자동 모델 전환을 지원하며, 이는 두 모델 간의 턴 기반 전환을 제공하여 모델 기능과 비용 및 속도의 균형을 맞추는 데 도움이 됩니다.
:::

### goose 플래너 환경 변수 설정
bash 셸 구성 파일(.bashrc)에 다음 줄을 추가하여 플래너 환경 변수를 추가할 수 있습니다:
```bash
export GOOSE_PLANNER_PROVIDER=<my-chosen-provider>
export GOOSE_PLANNER_MODEL=<my-chosen-model>
```
구성 파일에 변경 사항을 저장한 후 goose가 변수를 사용할 수 있도록 goose 세션을 다시 시작해야 합니다.

이것들이 설정되지 않으면 goose는 기본 공급자 및 모델 설정을 사용합니다. 특정 모델이 작업을 명확한 단계로 분해하는 데 더 나은 경우 다른 계획 모델을 설정할 수 있습니다. 그러나 기본 모델 구성으로도 일반적으로 충분합니다.

플래너 공급자가 설정되었는지 확인하려면 다음 터미널 명령을 입력하세요:

```bash
~ goose info -v
```

이 예시에서 `info` 명령은 현재 구성과 구성 파일 경로를 반환합니다.

```bash
goose Version:
  Version:          1.0.18

goose Locations:
  Config file:      /Users/alincoln/.config/goose/config.yaml
  Sessions dir:     /Users/alincoln/.local/share/goose/sessions
  Logs dir:         /Users/alincoln/.local/state/goose/logs

goose Configuration:
  GOOSE_PROVIDER: anthropic
  GOOSE_MODEL: claude-3.5-sonnet
  GOOSE_PLANNER_PROVIDER: openai
  GOOSE_MODE: smart_approve
  GOOSE_PLANNER_MODEL: gpt-4.1
```

`GOOSE_PLANNER_PROVIDER` 또는 `GOOSE_PLANNER_MODEL` 중 하나가 설정되지 않으면 `GOOSE_PROVIDER`와 `GOOSE_MODEL`이 계획을 세우는 데 사용됩니다.

## 프로젝트 설명하기
goose는 복잡한 프로젝트 설명을 처리할 수 있지만 명확하고 간결한 설명에서 가장 잘 작동합니다. 프로젝트의 목적과 원하는 결과를 명시하는 데 집중하세요. 이것들이 명확하지 않으면 goose는 목표를 완전히 이해할 때까지 명확한 질문을 합니다.

## 간단한 건설 계획 예시
goose는 주택 건설 예시와 같은 비교적 간단한 프로젝트에 대해 좋은 계획을 생성할 수 있습니다:

```bash
( O)> /plan

Entering plan mode. You can provide instructions to create a plan and then act on it. To exit early, type /endplan

( O)> Build a four bedroom house
◓  Measuring semantic distance...

I need to gather more information before creating a detailed house-building plan. Please provide clarification on the following:

1. What is the purpose of this plan? Are you looking for architectural designs, construction steps, or a project management timeline?
2. What is your budget range for this four-bedroom house?
3. What is the approximate square footage or size requirements?
4. Do you have a specific location or lot already selected? What are the dimensions and any zoning restrictions?
5. Are there any specific architectural styles or features you want included?
6. How many bathrooms and other rooms (e.g., kitchen, living room, dining room, garage) do you want?
7. Do you have any specific sustainability or energy efficiency requirements?
8. What is your timeline for completion?

( O)> 1 construction steps and a project management timeline. 2 $600,000. 3 4,000 sq feet. 4 the lot is not yet selected. 5 one story rambler. 6 four bedrooms, two ful baths, a kitchen, diningroom, family room, and garage. 7 the house must be efficient and powered by solar energy. 8 complete in six months.
◓  Taming tensors...                                                                                                          I'll create a detailed plan for building a 4,000 sq ft, one-story rambler with four bedrooms, two full bathrooms, kitchen, dining room, family room, garage, and home office. The house will be energy efficient with solar power, with a $600,000 budget and 6-month timeline.

◇  Do you want to clear message history & act on this plan?
│  Yes
│

Exiting plan mode and acting on the above plan

### Introduction to Building a 4,000 Sq Ft Rambler
To create a detailed plan for your 4,000 sq ft, one-story rambler, we need to consider several factors including design, energy efficiency, budget, and timeline. Given your requirements, here's an overview of how to approach this project:

### Step 1: Design and Planning
- **Architectural Design**: Hire an architect to design the house, ensuring it meets your space requirements and is energy efficient. Consider factors like natural lighting, insulation, and window placement.
- **Floor Plan**: Create a floor plan that includes four bedrooms, two full bathrooms, a kitchen, dining room, family room, garage, and home office. Ensure the layout is functional and spacious.
- **Land Preparation**: Ensure the building site is prepared, including clearing the land, grading, and preparing the foundation.

### Step 2: Energy Efficiency and Solar Power
- **Solar Power System**: Design and install a solar power system that can meet the energy needs of a 4,000 sq ft house. This may include a large solar panel array and a battery storage system for nighttime and cloudy days.
- **Insulation and Windows**: Use high-quality insulation in the walls, floor, and ceiling to minimize heat loss. Install energy-efficient windows that allow natural light in while keeping the heat or cold out.
- **Energy-Efficient Appliances**: Specify energy-efficient appliances and lighting to minimize energy consumption.

### Step 3: Budgeting
- **Construction Costs**: Allocate approximately $200-250 per sq ft for construction, which translates to $800,000-$1,000,000 for a 4,000 sq ft house. However, your budget is $600,000, so you'll need to optimize costs without compromising on quality and energy efficiency.
- **Solar Power System Costs**: Budget an additional $20,000 to $50,000 for the solar power system, depending on its size and complexity.
- **Contingency Fund**: Set aside 10-20% of the budget for unexpected expenses.

### Step 4: Timeline
- **Month 1-2: Design and Planning**: Complete the architectural design, obtain necessary permits, and prepare the building site.
- **Month 3-4: Foundation and Framing**: Lay the foundation, frame the house, and install the roof.
- **Month 5: Installation of Electrical, Plumbing, and HVAC Systems**: Install the solar power system, electrical, plumbing, and HVAC systems.
- **Month 6: Finishing Work**: Complete the interior finishing work, including installation of energy-efficient appliances, flooring, and fixtures.

### Conclusion
Building a 4,000 sq ft, energy-efficient rambler with solar power within a $600,000 budget and 6-month timeline is challenging but feasible with careful planning and design. It's essential to work closely with your architect, contractor, and solar power system installer to ensure the project stays on track and within budget.

Given that no extensions are currently available for more specific tools or data sources, consider the following steps to proceed with your project:

* Search for available extensions using the <function=platform__search_available_extensions>{}

```
주택 건설 계획은 goose의 현재 모델이 건설보다는 기술 및 소프트웨어 개발에 특화되어 있기 때문에 높은 수준을 유지합니다. 그래서 첫 번째 예시인 웹 애플리케이션 구축과 같은 프로젝트가 더 상세한 계획과 구체적인 지침을 받습니다.

### 계획 하위 단계에 대한 별도 계획 만들기
주택 건설 예시로 돌아가 봅시다. 계획에는 건축가 고용이 포함되어 있지만, 이 높은 수준의 단계에는 어떤 유형의 건축가를 고용할지, 선택 과정을 어떻게 진행할지와 같은 더 자세한 내용이 필요합니다.

```
- **Architectural Design**: Hire an architect to design the house, ensuring it meets your space requirements and is energy efficient. Consider factors like natural lighting, insulation, and window placement.
```
건설 계획을 검토하는 동안 계획 모드를 종료해도 언제든지 다시 시작하여 goose와 계속 작업할 수 있습니다.

```
( O)> /plan hire an architect

Entering plan mode. You can provide instructions to create a plan and then act on it. To exit early, type /endplan

◓  Synchronizing flock algorithms...                                                                                                       I need some clarifying questions to better understand your request about hiring an architect:

1. What is your location or preferred region for the architect to be based in?
2. Do you have a specific budget range for architectural services?
3. Are you looking for an architect with specific expertise (e.g., energy-efficient homes, modern design, traditional styles)?
4. Do you need full architectural services (design through construction oversight) or just certain phases?
5. Do you have a timeline for when you'd like to begin working with an architect?
6. Have you already purchased land for your home, or will the architect need to help evaluate potential sites?
7. Are there any specific certifications or qualifications you're looking for in an architect?
8. Would you prefer to work with a solo practitioner or a larger architectural firm?
9. Do you have any examples of architectural styles or specific homes you admire?
10. How involved do you want to be in the design process?
```

명확한 질문을 통해 정보를 수집한 후 goose는 건축가 고용에 대한 상세한 계획을 만듭니다. 이 하위 계획은 더 큰 주택 건설 프로젝트와 통합되며, 단계들은 전체 건설 맥락을 반영하고 지원합니다.

## 개발 프로젝트 예시
이 예시에서 개발자는 Contentful CMS와 상호작용하여 사용자가 문자열을 검색하고 새 문자열로 바꿀 수 있게 하는 Python CLI를 작성했습니다. 웹사이트로서 검색/바꾸기 기능이 더 사용하기 쉽고 더 많은 기능 세트를 허용합니다. 개발자는 goose CLI를 사용하여 변환 프로젝트를 계획하고 있습니다.

goose가 프로젝트를 여러 가지 방법과 다양한 구성 요소를 사용하여 완료할 수 있다고 판단하면 각 결정 지점에 대해 명확한 질문을 합니다. 예를 들어, 다음과 같이 계획을 시작하면:

```bash
( O)> /plan

Entering plan mode. You can provide instructions to create a plan and then act on it. To exit early, type /endplan

( O)> Convert the CLI built by search_replace_routes.py into a web page
```
goose는 프로젝트 설명을 파싱하고 구성한 LLM 모델과 상담한 다음 더 많은 정보가 필요하면 명확한 질문을 시작합니다.

## 명확한 질문
Python CLI를 웹사이트로 변환하는 것은 충분히 간단해 보이지만 goose는 스타일링, 인증, 기능, 기술 스택 등에 대해 질문할 것입니다. 다음과 같은 질문을 볼 수 있습니다:

```bash
1. Should the application support any keyboard shortcuts for common actions?
2. Would you like the application to remember user preferences (like case sensitivity setting) between sessions?
3. Should there be any form of notification when operations complete successfully?
4. How should the application handle very large text fields that might be difficult to display in the three-column layout?
5. Are there any specific CI/CD requirements for deployment to AWS?

```
질문에 하나씩 답하거나 일괄적으로 답할 수 있습니다:

```bash
( O)> 1 no keyboard shortcuts. 2 do not remember preferences. 3 on success, open a dialog that says "success". 4 Truncate to 30 characters before the string to replace and then 30 characters after the string to replace.5 no CI/CD requirements
```


:::tip
계획 모드에서 goose가 소스 코드와 같은 프로젝트 아티팩트를 요청하면 내용을 채팅에 직접 붙여넣어야 합니다. 파일 내용을 복사하고 'Here's the requested code:'와 같은 간단한 설명을 접두사로 붙이면 충분합니다. 계획 모드에서는 파일 경로만 제공하면 작동하지 않습니다.
:::

여러 질문에 답할 때 각 질문에 맞게 응답에 번호를 매기세요. 예를 들어, 단순히 'no'나 'don't remember'로 답하는 대신 '2. Do not store my preferences.'와 같이 맥락을 제공하세요. 이렇게 하면 goose가 어떤 질문에 답변했는지 추적하고 반복되는 질문을 방지할 수 있습니다.

CLI를 웹사이트로 변환하는 것과 같은 복잡한 프로젝트에서 goose는 여러 라운드의 명확한 질문을 할 수 있습니다. 각 라운드는 일반적으로 이전 답변의 새로운 정보에서 비롯되거나 프로젝트의 특정 측면에 대해 추가 세부 정보가 필요할 때 발생합니다.

goose의 _모든_ 질문에 답변했고 더 이상 질문이 없으면 goose가 계획을 생성합니다. 다른 경우에는 goose가 절대 질문을 멈추지 않을 것 같을 수 있습니다. 계획을 원하고 더 이상 질문에 답하고 싶지 않다면 간단히 "일반적인" 계획을 요청할 수 있습니다:

```bash
I still need some critical information to create a comprehensive plan:

1. What specific functionality does your Python script perform with Contentful? Could you share the actual code or describe how it interacts with the Contentful API to search and replace content?
2. What are the 5 predefined keys/inputs you mentioned earlier?
3. Should the web application include all the command-line options from the script (environment, status, case-sensitive, partial-match, dry-run)?
4. How should the application handle errors from the Contentful API?
5. Should the application maintain any history of previous search/replace operations?

Without this information, I can only provide a generic plan that might not accurately capture your requirements.
( O)> please provde a generic plan
```
goose는 표준화된 계획 형식을 생성하지만 답변을 기반으로 내용을 사용자 정의합니다. goose는 생성하는 계획의 단계를 구현하는 데 필요한 코드를 생성할 수 있습니다. 계획 모드를 종료(`/endplan`)하고 goose에게 계획을 구현하도록 요청하기 전에 계획과 생성된 코드를 검토해야 합니다.

다음은 이 프로젝트의 샘플 계획이며, 생성된 웹사이트 코드는 간결성을 위해 생략되었습니다:

```bash
# Plan for Converting CLI Script to React Web Application

## Step 1: Set up the React project and dependencies
1. Create a new React application using Create React App
2. Install necessary dependencies
3. Set up project structure
   src/
   ├── components/
   │   ├── Auth/
   │   ├── Layout/
   │   ├── Search/
   │   ├── Results/
   │   └── History/
   ├── services/
   │   ├── contentful.ts
   │   ├── auth.ts
   │   └── storage.ts
   ├── utils/
   │   ├── validation.ts
   │   └── helpers.ts
   ├── contexts/
   │   └── AppContext.tsx
   └── pages/
       ├── Home.tsx
       ├── Search.tsx
       └── History.tsx

## Step 2: Implement authentication with Okta
1. Create an Okta application in the Okta Developer Console
2. Configure the Okta authentication settings in the application
3. Implement protected routes using Okta's React components
## Step 3: Create the layout and UI components with Material UI
1. Implement the main layout with Material UI
2. Create the search form component
3. Create the results display component with three columns
4. Implement the "Load More" pagination component
## Step 4: Implement Contentful service
1. Create a service for interacting with Contentful
## Step 5: Implement local storage service for history
1. Create a service for managing search history in localStorage
## Step 6: Implement validation utilities
1. Create utility functions for input validation
## Step 7: Implement main pages
1. Create the Search page
2. Create the History page
## Step 8: Set up routing and main application
1. Create the main App component
## Step 9: Implement error handling and loading states
1. Create error boundary components
2. Add loading indicators for API operations
3. Implement error messages display

## Step 10: Set up deployment configuration
1. Create AWS deployment configuration
## Step 11: Testing and quality assurance
1. Write unit tests for key components
2. Implement integration tests for the main workflows
3. Perform manual testing of the application

This plan provides a comprehensive framework for converting your CLI script to a React web application with Material UI and Okta authentication. You'll need to adapt specific parts based on your exact requirements and the functionality of your original script.

```



## 기본 사용법
CLI를 계획 모드로 전환하기 전에 활성 goose 세션이 있어야 합니다. 계획 세우기에 전용 세션을 사용하려면 다음 예시와 같이 새 세션에 이름을 지정해야 합니다:

```bash
~ goose session -n web-project-plan
starting session | provider: databricks model: databricks-meta-llama
    session id: 20251110_5
    working directory: /Users/alincoln

goose is running! Enter your instructions, or try asking what goose can do.
```
계획 모드로 들어가려면 `/plan`을 입력하세요. 선택적으로 프롬프트 완성 명령에 계획 설명을 추가할 수 있습니다.
```bash
( O)> /plan  Build a four bedroom house
```

CLI의 계획 모드는 goose가 작업을 관리 가능한 단계로 분해하는 데 도움이 되는 특별한 상호작용 모드입니다. 계획 모드를 닫고 활성 세션으로 돌아가려면 `/endplan`을 입력하세요.

```bash
( O)> /endplan
```

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>📚 추천 튜토리얼</h2>
  <div className={styles.cardGrid}>
    <Card
      title="복잡한 작업 계획하기"
      description="Plan 기능을 사용하여 복잡한 작업을 관리 가능하고 실행 가능한 단계로 분해하는 방법을 알아보세요."
      link="/docs/tutorials/plan-feature-devcontainer-setup"
    />
  </div>
</div>
