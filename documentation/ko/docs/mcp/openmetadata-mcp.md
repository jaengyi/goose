---
title: OpenMetadata 확장 기능
description: OpenMetadata MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

[OpenMetadata MCP 서버](https://open-metadata.org/mcp) 확장 기능을 사용하면 goose가 OpenMetadata와 직접 상호작용할 수 있어 OpenMetadata 자산, 용어집 및 계보에 대한 작업을 수행할 수 있습니다. 이를 통해 자연어 상호작용을 통해 OpenMetadata에 저장된 메타데이터로 쉽게 작업할 수 있습니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=mcp-remote&arg=http%3A%2F%2Flocalhost%3A8585%2Fmcp&arg=--auth-server-url%3Dhttp%3A%2F%2Flocalhost%3A8585%2Fmcp&arg=--client-id%3Dopenmetadata&arg=--verbose&arg=--clean&arg=--header&arg=Authorization%3A%24%7BAUTH_HEADER%7D&id=openmetadata&name=OpenMetadata&description=Intelligent%20data%20management%20and%20automated%20data%20operations&env=AUTH_HEADER%3DBearer%20YOUR_OPENMETADATA_PAT)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx -y mcp-remote http://localhost:8585/mcp --auth-server-url=http://localhost:8585/mcp --client-id=openmetadata --verbose --clean --header Authorization:${AUTH_HEADER}
  ```
  </TabItem>
</Tabs>
:::

## 연결 사용자 정의

OpenMetadata MCP 서버는 OpenMetadata의 내장 MCP 서버를 사용하여 OpenMetadata 인스턴스에 연결합니다. 여기서는 로컬 OpenMetadata 인스턴스에 액세스하기 위해 `http://localhost:8585`를 예시로 사용하지만 자체 환경에 맞게 구성할 수 있습니다. 여기서 AUTH_HEADER는 [OpenMetadata 개인 액세스 토큰(PAT)](https://docs.open-metadata.org/latest/how-to-guides/mcp#adding-a-personal-access-token-to-your-mcp-client)입니다.

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용). 또한 실행 중인 OpenMetadata 인스턴스 또는 [OpenMetadata 샌드박스](https://sandbox.open-metadata.org/)에 대한 액세스가 필요합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    <GooseDesktopInstaller
      extensionId="openmetadata"
      extensionName="OpenMetadata"
      description="지능적인 데이터 관리 및 자동화된 데이터 작업"
      command="npx"
      args={["-y", "mcp-remote", "http://localhost:8585/mcp", "--auth-server-url=http://localhost:8585/mcp", "--client-id=openmetadata", "--verbose", "--clean", "--header", "Authorization:${AUTH_HEADER}"]}
      envVars={[
        { name: "AUTH_HEADER", label: "Bearer YOUR_OPENMETADATA_PAT" }
      ]}
      apiKeyLink="https://docs.open-metadata.org/latest/how-to-guides/mcp#adding-a-personal-access-token-to-your-mcp-client"
      apiKeyLinkText="OpenMetadata 개인 액세스 토큰"
    />

    :::info 연결 문자열 구성
    필요한 경우 [OpenMetadata 환경](#연결-사용자-정의)에 맞게 [확장 기능을 업데이트](/docs/getting-started/using-extensions#updating-extension-properties)하세요.
    :::

  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="OpenMetadata"
      description="지능적인 데이터 관리 및 자동화된 데이터 작업"
      command="npx -y mcp-remote http://localhost:8585/mcp --auth-server-url=http://localhost:8585/mcp --client-id=openmetadata --verbose --clean --header Authorization:${AUTH_HEADER}"
      envVars={[
        { key: "AUTH_HEADER", value: "Bearer YOUR_OPENMETADATA_PAT" }
      ]}
      commandNote={
        <>
          <code>http://localhost:8585/mcp</code>를 <a href="#연결-사용자-정의">OpenMetadata 환경</a>에 맞는 실제 OpenMetadata 인스턴스 URL로 교체하세요.
        </>
      }
      infoNote={
        <>
          <a href="https://docs.open-metadata.org/latest/how-to-guides/mcp#adding-a-personal-access-token-to-your-mcp-client" target="_blank" rel="noopener noreferrer">OpenMetadata 개인 액세스 토큰</a>을 얻어 <code>Bearer</code> 토큰으로 붙여넣으세요.
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 가능한 작업

OpenMetadata 확장 기능은 자연어 상호작용을 통해 포괄적인 메타데이터 관리 기능을 제공합니다. 다음을 포함한 다양한 작업을 수행할 수 있습니다:

### OpenMetadata 작업
- **search_metadata** OpenMetadata 카탈로그에서 데이터 자산과 비즈니스 용어를 찾습니다.
- **get_entity_details** 정규화된 이름을 사용하여 특정 엔티티에 대한 자세한 정보를 검색합니다.
- **create_glossary** 비즈니스 용어와 정의를 구성하기 위한 새 용어집을 만듭니다.
- **create_glossary_term** 계층적 관계를 지원하여 기존 용어집 내에 새 용어를 만듭니다.
- **get_entity_lineage** 데이터 종속성과 영향 분석을 이해하기 위해 모든 엔티티의 업스트림 및 다운스트림 계보 정보를 검색합니다.

자연어로 달성하고 싶은 것을 설명하기만 하면 goose가 적절한 OpenMetadata 작업을 안전하고 효율적으로 실행하는 데 도움을 줄 것입니다.

## 사용 예시

이 예시에서 goose는 OpenMetadata에 저장된 `customers`라는 Redshift 테이블의 메타데이터, 특히 계보를 살펴봅니다. 계보에 `customerinfo`라는 Databricks 테이블이 `customers`를 만드는 데 사용되는 것으로 표시되지 않으면 goose가 추가합니다.

#### goose 프롬프트
```
redshift의 customers 테이블을 만드는 데 어떤 테이블이 사용되나요? databricks 테이블 customerinfo가 아직 없다면 customers 테이블 계보에 추가해줘.
```

#### goose 출력
```
분석 결과 `redshift prod.dev.dbt_jaffle.customers`에서 Redshift customers 테이블을 찾았습니다. 현재 이를 만드는 데 사용되는 테이블들입니다:

##### Redshift Customers 테이블의 현재 업스트림 테이블
###### 직접 종속성:
1. `redshift prod.dev.dbt_jaffle.stg_customers` (Customers용 스테이징 테이블)
2. `redshift prod.dev.dbt_jaffle.stg_payments`
3. `redshift prod.dev.dbt_jaffle.stg_orders`

###### 간접 종속성 (원시 테이블):
4. `redshift prod.dev.dbt_jaffle.raw_customers` → `stg_customers`로 공급
5. `redshift prod.dev.dbt_jaffle.raw_payments` → `stg_payments`로 공급
6. `redshift prod.dev.dbt_jaffle.raw_orders` → `stg_orders`로 공급

###### 누락된 Databricks CustomerInfo 테이블
다음 위치에서 Databricks `customerinfo` 테이블을 찾았습니다:

`DatabricksBanking.demo.mssqlbanking_customer.customerinfo`
이 테이블은 현재 Redshift customers 테이블 계보에 연결되어 있지 않습니다.

Databricks customerinfo 테이블과 Redshift customers 테이블 간의 계보를 성공적으로 만들었습니다. 수행한 작업은 다음과 같습니다:
###### 계보가 성공적으로 생성됨
소스 테이블 (Databricks):

이름: `customerinfo`
전체 이름: `DatabricksBanking.demo.mssqlbanking_customer.customerinfo`
플랫폼: Databricks
열: customerid, firstname, lastname, dateofbirth, email, phonenumber, _fivetran_deleted, _fivetran_synced

대상 테이블 (Redshift):

이름: `customers`
전체 이름: `redshift prod.dev.dbt_jaffle.customers`
플랫폼: Redshift
열: customer_id, first_name, last_name, first_order, most_recent_order, number_of_orders, customer_lifetime_value

계보 관계는 이제 Databricks customerinfo 테이블이 Redshift customers 테이블을 만들기 위해 데이터를 공급하는 업스트림 소스임을 보여줍니다. 이 관계는 OpenMetadata의 계보 뷰에서 볼 수 있어 Databricks와 Redshift 시스템 간의 데이터 흐름과 종속성을 추적하는 데 도움이 됩니다.
이제 OpenMetadata UI에서 두 테이블 중 하나로 이동하여 "Lineage" 탭을 확인하면 이러한 고객 데이터 자산 간의 연결을 볼 수 있습니다.
```
