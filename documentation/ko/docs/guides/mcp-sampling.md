---
sidebar_position: 54
title: MCP 샘플링 확장 기능
sidebar_label: MCP 샘플링
description: MCP 서버를 goose의 AI를 사용하여 생각하고, 분석하고, 결정을 내릴 수 있는 지능적인 에이전트로 변환
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

MCP 샘플링은 확장 기능을 단순한 데이터 제공자에서 지능적인 에이전트로 변환할 수 있습니다. goose가 해석할 원시 정보만 반환하는 대신, 확장 기능은 goose의 AI 기능을 활용하여 전문가 수준의 안내를 제공하고, 상황별 분석을 수행하며, 완전히 새로운 상호작용 패턴을 만들 수 있습니다.

이 기능은 goose에서 자동으로 활성화되며 구성이 필요하지 않습니다! 샘플링을 지원하는 모든 MCP 서버 확장 기능은 goose가 사용하는 LLM에 자동으로 액세스할 수 있습니다. 이는 다음을 의미합니다:
- goose 사용자는 확장 기능의 특정 기능에 맞춰진 더 표적화된 응답을 받을 수 있습니다
- 개발자는 MCP 서버에 샘플링 지원을 추가하여 goose에서 향상된 기능을 제공할 수 있습니다

MCP 샘플링이 작동하는 것을 보려면 [Council of Mine](/docs/mcp/council-of-mine-mcp) 확장 기능을 사용해 보세요!

:::info
[MCP 샘플링](https://modelcontextprotocol.io/specification/draft/client/sampling)은 Model Context Protocol의 기능입니다.
:::

## MCP 샘플링 작동 방식

MCP 샘플링을 사용하면 확장 기능이 작업에 대해 goose의 AI에게 도움을 요청할 수 있습니다. 확장 기능이 데이터를 분석하거나, 지능적인 결정을 내리거나, 자연어를 이해해야 할 때 AI 지원을 요청하는 "샘플링" 요청을 보낼 수 있습니다. goose는 AI 기능을 사용하여 요청을 처리하고 확장 기능에 응답을 반환합니다.

이 기능을 통해 확장 기능은 더 전문적인 상황별 응답이나 새로운 상호작용 패턴을 제공할 수 있습니다. 다음 예시는 데이터베이스 확장 기능이 도메인 지식과 goose의 AI 분석을 결합하여 전문가 수준의 진단을 제공하는 방법을 보여줍니다:

<Tabs>
  <TabItem value="without" label="샘플링 없이">
    1. goose에게 묻습니다: "데이터베이스 성능에 무슨 문제가 있어?"

    2. goose가 데이터베이스 도구를 호출합니다

    3. 데이터베이스 도구가 goose에 원시 메트릭을 반환합니다:
       ```
       Query times: 2.3s, 1.8s, 5.2s, 0.3s, 8.1s
       Table sizes: users (1M rows), orders (5M rows)
       Indexes: 3 on users, 1 on orders
       ```

    4. goose가 일반적인 권장 사항으로 응답합니다:
       ```
       데이터베이스가 느린 것 같습니다. 일부 쿼리가 5초 이상 걸립니다. 더 많은 인덱스가 필요할 수 있습니다.
       ```

  </TabItem>
  <TabItem value="with" label="샘플링으로">
    1. goose에게 묻습니다: "데이터베이스 성능에 무슨 문제가 있어?"

    2. goose가 데이터베이스 도구를 호출합니다

    3. 데이터베이스 도구가 원시 메트릭을 가져옵니다:
       ```
       Query times: 2.3s, 1.8s, 5.2s, 0.3s, 8.1s
       Table sizes: users (1M rows), orders (5M rows)
       Indexes: 3 on users, 1 on orders
       ```

       그런 다음 도구가:
       - 도메인 전문 지식(쿼리 패턴, 테이블 관계, 데이터베이스 유형)을 사용하여 goose의 AI에게 묻습니다: "이 메트릭과 이 PostgreSQL 데이터베이스의 JOIN 패턴을 알고 있을 때, 문제가 뭐야?"
       - AI가 향상된 응답을 goose에 반환합니다

    4. goose가 표적화된 권장 사항으로 응답합니다:
       ```
       orders 테이블에 customer_id에 대한 인덱스가 없어서 JOIN 쿼리에서 5-8초 지연이 발생하고 있습니다. 느린 쿼리는 모두 고객 조회와 관련이 있습니다. 실행: `CREATE INDEX idx_orders_customer ON orders(customer_id);`
       ```

  </TabItem>
</Tabs>

### 사용 사례

MCP 샘플링은 다음과 같은 강력한 기능을 가능하게 합니다:
- 컨텍스트에서 코드를 설명하는 **스마트 문서 도구**
- 결과를 필터링하고 순위를 매기는 **지능적 검색**
- 특정 최적화 권장 사항을 제공하는 **데이터베이스 분석기**
- 확장 기능이 여러 AI 관점을 생성하고 종합하는 **다중 관점 분석**

## 확장 기능 개발자를 위해

자체 확장 기능에 MCP 샘플링을 추가하고 싶으신가요? MCP 서버가 goose의 AI 기능을 활용하는 방법에 대해 자세히 알아보려면 [사용자 정의 확장 기능 빌드](/docs/tutorials/custom-extensions) 튜토리얼을 참조하세요.

## 추가 리소스

import ContentCardCarousel from '@site/src/components/ContentCardCarousel';
import mcpSampling from '@site/blog/2025-12-04-mcp-sampling/mcp-sampling.png';

<ContentCardCarousel
  items={[
    {
      type: 'blog',
      title: 'MCP 샘플링: 도구가 생각해야 할 때',
      description: 'MCP 샘플링이 어떻게 도구가 AI를 호출하게 하는지 알아보세요.',
      thumbnailUrl: mcpSampling,
      linkUrl: '/goose/blog/2025/12/04/mcp-sampling',
      date: '2025-12-04',
      duration: '6분 읽기'
    }
  ]}
/>
