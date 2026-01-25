---
title: Auto Visualiser 확장 기능
description: goose에 자동 데이터 시각화 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseBuiltinInstaller from '@site/src/components/GooseBuiltinInstaller';
import ImageCarousel from '@site/src/components/ImageCarousel';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/txh6DlzPwNo" />

Auto Visualiser 확장 기능은 goose가 대화에서 직접 대화형 데이터 시각화를 자동으로 생성할 수 있게 합니다. 이 내장 확장 기능은 MCP-UI를 활용하여 수동 요청 없이 차트, 그래프, 지도 및 기타 데이터 시각적 표현을 만듭니다.

이 가이드는 Auto Visualiser MCP 서버를 활성화하고 사용하는 방법을 다룹니다.

## 구성


<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseBuiltinInstaller
    extensionName="Auto Visualiser"
    description="대화형 데이터 시각화 자동 생성"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">

  1. `configure` 명령을 실행합니다:
  ```sh
  goose configure
  ```

  2. `Toggle Extensions`를 선택합니다
  ```sh
  ┌   goose-configure
  │
  ◇  What would you like you configure?
  │  Toggle Extensions
  │
  ◆  Enable extensions: (use "space" to toggle and "enter" to submit)
  // highlight-start
  │  ● autovisualiser
  // highlight-end
  └  Extension settings updated successfully
  ```
  </TabItem>
</Tabs>


## 시각화 유형

Auto Visualiser는 goose의 MCP-UI 시스템과 통합되어 데이터가 시각화에서 이점을 얻을 수 있는 시점을 자동으로 감지하고 대화형 시각적 구성 요소를 렌더링하는 강력한 확장 기능입니다. 대화에서 데이터 패턴을 분석하고 가장 적절한 시각화 유형을 제안합니다.

| 차트 유형 | 설명 | 프롬프트가 암시할 때 감지 |
|------------|-------------|------------------------------|
| **Sankey 다이어그램** | 관계와 수량을 보여주는 흐름 데이터 시각화 | 흐름 또는 프로세스 데이터 <br/>(워크플로우, 퍼널, 프로세스 데이터셋) |
| **레이더 차트** | 다차원 데이터 비교(스파이더 차트) | 다차원 비교 <br/>(성능 메트릭, 기능 비교) |
| **도넛/파이 차트** | 여러 차트를 지원하는 범주형 데이터 시각화 | 범주형 비율 <br/>(백분율 분석, 카테고리 분포) |
| **트리맵 시각화** | 비례 면적 표현이 있는 계층적 데이터 | 계층적 데이터 <br/>(중첩된 카테고리, 조직 구조) |
| **코드 다이어그램** | 엔티티 간 관계 및 흐름 시각화 | 관계 매트릭스 <br/>(네트워크 연결, 상호 참조) |
| **대화형 지도** | Leaflet을 사용한 위치 마커가 있는 지리적 데이터 시각화 | 지리적 정보 <br/>(위치 데이터, 좌표, 주소) |
| **Mermaid 다이어그램** | Mermaid 구문을 사용한 플로우차트, 시퀀스 다이어그램, 간트 차트 및 기타 다이어그램 유형 | 다이어그램 생성 <br/>(플로우차트, 시퀀스 다이어그램, 아키텍처 다이어그램) |
| **라인/바/산점도 차트** | 데이터 분석을 위한 전통적인 차트 유형 | 시계열 데이터 <br/>(과거 데이터, 시간에 따른 추세) |

### 예시 시각화

<ImageCarousel id="flappy" width="40%" images={[

  require('/blog/2025-08-27-autovisualiser-with-mcp-ui/sankey.png').default,
  require('/blog/2025-08-27-autovisualiser-with-mcp-ui/treemap.png').default,
 ]}
 names={[
    "Sankey",
    "Treemap"
  ]} />

### 기능

- **자동 감지**: goose가 데이터에 가장 적합한 시각화 유형을 지능적으로 선택
- **대화형 구성 요소**: 모든 시각화는 호버 효과, 줌 및 드릴 기능을 갖춘 대화형
- **MCP-UI 통합**: 원활한 렌더링을 위해 새로운 MCP-UI 표준 활용
- **다중 차트 지원**: 단일 응답에서 여러 시각화를 렌더링할 수 있음
- **사용자 정의 스타일링**: 사용자 정의 색상, 레이블 및 서식 옵션 지원
- **내보내기 기능**: 시각화를 HTML 파일로 저장하여 공유 가능


## 사용 예시

이 예시에서는 goose가 판매 데이터를 분석하고 Auto Visualiser 확장 기능을 사용하여 자동으로 시각화합니다.


### goose 프롬프트
```
분기별 판매 데이터가 있습니다. 다음을 이해하는 데 도움을 줄 수 있나요:
1. 중첩된 제품 카테고리 전체에서 수익의 계층적 분석
2. 4분기 전체에서 성능 메트릭이 어떻게 비교되는지
3. 판매 퍼널 프로세스를 통한 고객 흐름

데이터입니다:
- 전자제품: Q1: $150k, Q2: $180k, Q3: $220k, Q4: $195k
- 의류: Q1: $120k, Q2: $140k, Q3: $160k, Q4: $175k
- 홈 & 가든: Q1: $80k, Q2: $95k, Q3: $110k, Q4: $125k
```

### goose 출력

:::note 데스크톱

분기별 판매 데이터를 세 가지 다른 시각화로 분석하여 각 질문에 답하겠습니다. 이 시각화들을 만들어 드리겠습니다.

먼저 제품 카테고리 전체에서 수익의 계층적 분석을 시각화해 보겠습니다:

![](/img/autovisualiser-treemap.png)

![](/img/autovisualiser-radar.png)

![](/img/autovisualiser-sankey.png)


세 가지 포괄적인 시각화를 만들어 다양한 관점에서 분기별 판매 데이터를 이해하는 데 도움을 드렸습니다:

#### 1. 계층적 수익 분석 (트리맵)
**트리맵 시각화**는 각 카테고리와 분기의 상대적 크기를 나타내는 비례 면적으로 수익 계층을 보여줍니다. 주요 인사이트:
- **전자제품**이 가장 큰 수익 창출 ($745k 총계)
- **의류**가 두 번째로 큼 ($595k 총계)
- **홈 & 가든**이 가장 작지만 성장 중 ($410k 총계)
- 각 카테고리 내에서 각 분기가 어떻게 기여하는지 볼 수 있음

#### 2. 성능 메트릭 비교 (레이더 차트)
**레이더 차트**는 세 가지 제품 카테고리가 4분기 동안 어떻게 수행되었는지 표시하여 비교하기 쉽게 합니다:
- **전자제품**은 Q3에서 강한 성능 ($220k)을 보이며 Q4에 약간 감소
- **의류**는 연중 일관된 성장을 보여줌
- **홈 & 가든**은 모든 분기에 걸쳐 꾸준한 상승 추세를 보임
- 모든 카테고리가 Q3-Q4에서 가장 강한 성능을 보임

#### 3. 판매 퍼널 흐름 (Sankey 다이어그램)
**Sankey 다이어그램**은 수익에 비례하는 흐름 두께로 판매 프로세스를 통한 고객 흐름을 나타냅니다:
- 각 카테고리에 대해 시장 기회가 어떻게 잠재 고객으로 흘러가는지 보여줌
- 잠재 고객에서 실제 판매로의 전환을 보여줌
- **전자제품**이 가장 두꺼운 흐름 (가장 높은 수익 전환)
- **홈 & 가든**이 가장 얇지만 성장 기회를 나타냄

#### 주요 비즈니스 인사이트:
- **총 수익**: 모든 카테고리에서 $1.75M
- **성장 패턴**: 모든 카테고리가 긍정적인 성장 추세를 보임
- **카테고리 성능**: 전자제품 (42.6%), 의류 (34%), 홈 & 가든 (23.4%)
- **시즌 트렌드**: Q3가 전체적으로 가장 강한 분기로 보임

추가 시각화를 만들거나 판매 데이터의 특정 측면을 더 깊이 살펴보시겠습니까?
:::



## 문제 해결

- **시각화가 나타나지 않음**: Auto Visualiser 확장 기능이 활성화되어 있고 데이터가 예상 형식을 따르는지 확인하세요.

- **데이터 형식 오류**: 데이터 구조가 시각화 유형에 필요한 스키마와 일치하는지 확인하세요. 확장 기능은 안내를 위한 자세한 오류 메시지를 제공합니다.

- **대규모 데이터셋의 성능**: 매우 큰 데이터셋의 경우 시각화 전에 데이터를 집계하거나 샘플링하는 것을 고려하세요.
