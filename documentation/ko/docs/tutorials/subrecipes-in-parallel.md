---
title: 서브레시피 병렬 실행
sidebar_label: 병렬 서브레시피
description: 실시간 진행 상황 추적과 함께 여러 서브레시피 인스턴스를 동시에 실행
---

goose 레시피는 격리된 워커 프로세스를 사용하여 여러 [서브레시피](/docs/guides/recipes/subrecipes) 인스턴스를 동시에 실행할 수 있습니다. 이 기능은 효율적인 배치 작업, 다양한 작업의 병렬 처리, 복잡한 워크플로우의 빠른 완료를 가능하게 합니다.

:::warning 실험적 기능
서브레시피 병렬 실행은 활발히 개발 중인 실험적 기능입니다. 동작과 구성은 향후 릴리스에서 변경될 수 있습니다.
:::

일반적인 사용 사례는 다음과 같습니다:

- **모노레포 빌드 실패**: 모노레포 빌드에서 3개의 서비스가 실패하면 각 빌드 URL로 "장애 진단" 서브레시피를 사용하여 모든 실패를 병렬로 진단
- **문서 요약**: 문서 링크가 있는 CSV 파일을 처리하여 각 링크에 대해 "문서 요약" 서브레시피를 동시에 실행
- **저장소 간 코드 분석**: 여러 코드베이스에서 보안, 품질, 성능 분석을 동시에 실행

## 작동 방식

병렬 서브레시피 실행은 동시 작업 실행을 자동으로 관리하는 격리된 워커 시스템을 사용합니다. goose는 각 서브레시피 인스턴스에 대해 개별 작업을 생성하고 최대 10개의 동시 워커에 분배합니다.

| 시나리오 | 기본 동작 | 재정의 옵션 |
|----------|------------------|------------------|
| **다른 서브레시피** | 순차적 | 프롬프트에 "in parallel" 추가 |
| **같은 서브레시피**와 다른 매개변수 | 병렬 | • `sequential_when_repeated: true` 설정<br />• 프롬프트에 "sequentially" 추가 |

### 다른 서브레시피

다른 서브레시피를 실행할 때 goose는 다음을 기반으로 실행 모드를 결정합니다:
1. **프롬프트의 명시적 사용자 요청** ("in parallel", "sequentially")
2. **기본적으로 순차 실행**: 병렬 실행을 명시적으로 요청하지 않으면 다른 서브레시피는 순차적으로 실행

프롬프트에서 다른 서브레시피를 호출할 때 "in parallel"을 언급하면 됩니다:

```yaml
prompt: |
  run the following subrecipes in parallel:
    - use weather subrecipe to get the weather for Sydney
    - use things-to-do subrecipe to find activities in Sydney
```

### 같은 서브레시피

같은 서브레시피를 다른 매개변수로 실행할 때 goose는 다음을 기반으로 실행 모드를 결정합니다:
1. **[레시피 수준 구성](#실행-모드-선택)** (`sequential_when_repeated` 플래그) - true로 설정하면 순차 실행 강제
2. **프롬프트의 사용자 요청** ("sequentially"로 기본 병렬 동작 재정의)
3. **기본적으로 병렬 실행**: 같은 서브레시피의 여러 인스턴스는 동시에 실행

프롬프트가 같은 서브레시피의 여러 실행을 암시하면 goose는 자동으로 병렬 인스턴스를 생성합니다:

```yaml
prompt: |
  get the weather forecast for the three biggest cities in Australia
```

이 예에서 goose는 "호주에서 가장 큰 세 도시"가 다른 도시에 대해 날씨 서브레시피를 여러 번 실행해야 한다는 것을 인식하므로 병렬로 실행합니다.

순차적으로 실행하려면 goose에게 말하면 됩니다:

```yaml
prompt: |
  get the weather for three biggest cities in Australia one at a time
```

### 실시간 진행 상황 모니터링

CLI에서 여러 작업을 병렬로 실행할 때 실행 중 자동으로 나타나는 실시간 대시보드를 통해 진행 상황을 추적할 수 있습니다. 대시보드는 다음을 제공합니다:
- **실시간 진행 추적**: 완료, 실행 중, 실패, 보류 중 통계로 작업 완료를 실시간으로 모니터링
- **작업 세부 정보**: 고유한 작업 ID, 매개변수 세트, 실행 타이밍, 출력 미리보기, Pending → Running → Completed/Failed로 진행되는 오류 정보 확인

## 예제

### 다른 서브레시피를 병렬로 실행

이 예는 `weather`와 `things-to-do` 서브레시피를 병렬로 실행합니다:

```yaml
# plan_trip.yaml
version: 1.0.0
title: Plan Your Trip
description: Get weather forecast and find things to do for your destination
instructions: You are a travel planning assistant that helps users prepare for their trips.
prompt: |
  run the following subrecipes in parallel to plan my trip:
    - use weather subrecipe to get the weather forecast for Sydney
    - use things-to-do subrecipe to find activities and attractions in Sydney
sub_recipes:
- name: weather
  path: "./subrecipes/weather.yaml"
  values:
    city: Sydney
- name: things-to-do
  path: "./subrecipes/things-to-do.yaml"
  values:
    city: Sydney
    duration: "3 days"
extensions:
- type: builtin
  name: developer
  timeout: 300
  bundled: true
```

### 같은 서브레시피를 병렬로 실행 (다른 매개변수)

이 예는 다른 도시에 대해 `weather` 서브레시피의 세 인스턴스를 병렬로 실행합니다:

```yaml
# multi_city_weather.yaml
version: 1.0.0
title: Multi-City Weather Comparison
description: Compare weather across multiple cities for trip planning
instructions: You are a travel weather specialist helping users compare conditions across cities.
prompt: |
  get the weather forecast for the three biggest cities in Australia
  to help me decide where to visit
sub_recipes:
- name: weather
  path: "./subrecipes/weather.yaml"
extensions:
- type: builtin
  name: developer
  timeout: 300
  bundled: true
```

**서브레시피:**

<details>
  <summary>weather</summary>
    ```yaml
    # subrecipes/weather.yaml
    version: 1.0.0
    title: Find weather
    description: Get weather data for a city
    instructions: You are a weather expert. You will be given a city and you will need to return the weather data for that city.
    prompt: |
      Get the weather forecast for {{ city }} for today and the next few days.
    parameters:
      - key: city
        input_type: string
        requirement: required
        description: city name
    extensions:
      - type: stdio
        name: weather
        cmd: uvx
        args:
          - mcp_weather@latest
        timeout: 300
    ```
</details>

<details>
  <summary>things-to-do</summary>
    ```yaml
    # subrecipes/things-to-do.yaml
    version: 1.0.0
    title: Things to do in a city
    description: Find activities and attractions for travelers
    instructions: You are a local travel expert who knows the best activities, attractions, and experiences in cities around the world.
    prompt: |
      Suggest the best things to do in {{ city }} for a {{ duration }} trip.
      Include a mix of popular attractions, local experiences, and hidden gems.
      {% if weather_context %}
      Consider the weather conditions: {{ weather_context }}
      {% endif %}
    parameters:
      - key: city
        input_type: string
        requirement: required
        description: city name
      - key: duration
        input_type: string
        requirement: required
        description: trip duration (e.g., "2 days", "1 week")
      - key: weather_context
        input_type: string
        requirement: optional
        default: ""
        description: weather conditions to consider for activity recommendations
    ```
</details>

## 실행 모드 선택

병렬 실행이 속도 이점을 제공하지만 순차 실행이 필요하거나 선호되는 경우가 있습니다. 결정 방법은 다음과 같습니다:

**순차 사용 시:**
- 작업이 공유 리소스를 수정하는 경우
- 실행 순서가 중요한 경우
- 메모리 또는 CPU 제약이 있는 경우
- 병렬 모드에서 복잡한 실패 디버깅 시

**병렬 사용 시:**
- 작업이 독립적인 경우
- 더 빠른 완료가 필요한 경우
- 시스템 리소스가 최대 10개의 병렬 워커에 대한 동시 실행을 처리할 수 있는 경우
- 대규모 데이터셋 또는 여러 파일 처리 시

**레시피 수준 구성:**

절대 병렬로 실행해서는 안 되는 서브레시피의 경우 `sequential_when_repeated: true`를 설정하여 사용자 요청을 재정의합니다:

```yaml
sub_recipes:
  - name: database-migration
    path: "./subrecipes/migrate.yaml"
    sequential_when_repeated: true  # 항상 순차적
```

## 더 알아보기
goose 레시피를 마스터하는 데 도움이 되는 더 많은 문서, 도구, 리소스는 [레시피](/docs/guides/recipes) 가이드를 확인하세요.
