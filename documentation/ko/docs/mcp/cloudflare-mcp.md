---
title: Cloudflare 확장 프로그램
description: Cloudflare MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

이 튜토리얼에서는 자연어를 사용하여 Cloudflare 인프라 관리, 애플리케이션 디버깅, 트래픽 분석 등을 수행하기 위해 [Cloudflare의 MCP 서버](https://github.com/cloudflare/mcp-server-cloudflare)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

Cloudflare는 플랫폼의 다양한 측면을 위한 여러 특화된 MCP 서버를 제공하여 Workers, DNS, 보안 기능, 분석 및 개발 도구와 상호 작용할 수 있습니다.

:::tip 요약

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=mcp-remote&arg=https%3A%2F%2Fobservability.mcp.cloudflare.com%2Fmcp&id=cloudflare-observability&name=Cloudflare%20Observability&description=Debug%20and%20get%20insight%20into%20your%20application%27s%20logs%20and%20analytics&env=CLOUDFLARE_API_TOKEN%3DCloudflare%20API%20Token)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx mcp-remote https://observability.mcp.cloudflare.com/mcp
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  CLOUDFLARE_API_TOKEN: 적절한 권한이 있는 Cloudflare API 토큰
  ```
:::

## 사용 가능한 Cloudflare MCP 서버

Cloudflare는 다양한 사용 사례를 위한 여러 특화된 MCP 서버를 제공합니다:

| 서버 | 설명 | 사용 사례 |
|--------|-------------|-----------|
| **Documentation** | Cloudflare에 대한 최신 참조 정보 제공 | API 참조, 기능 문서, 문제 해결 가이드 |
| **Workers Bindings** | 스토리지, AI, 컴퓨팅 프리미티브로 Workers 애플리케이션 구축 | KV 스토리지, R2 버킷, AI 모델, Durable Objects |
| **Workers Builds** | Cloudflare Workers 빌드에 대한 인사이트 및 관리 | 배포 상태, 빌드 로그, 버전 관리 |
| **Observability** | 애플리케이션 로그 및 분석에 대한 디버깅 및 인사이트 | 오류 추적, 성능 모니터링, 요청 분석 |
| **Radar** | 글로벌 인터넷 트래픽 인사이트, 트렌드, URL 스캔 및 유틸리티 | 트래픽 분석, 위협 인텔리전스, URL 스캐닝 |
| **Container** | 샌드박스 개발 환경 구축 | 격리된 테스트, 개발 컨테이너 |
| **Browser Rendering** | 웹 페이지 가져오기, 마크다운 변환, 스크린샷 촬영 | 웹 스크래핑, 콘텐츠 분석, 시각적 테스트 |
| **Logpush** | Logpush 작업 상태에 대한 빠른 요약 | 로그 관리, 데이터 파이프라인 모니터링 |
| **AI Gateway** | 로그 검색, 프롬프트 및 응답에 대한 세부 정보 | AI 사용량 분석, 프롬프트 최적화 |
| **AutoRAG** | AutoRAG의 문서 목록 및 검색 | 문서 검색, 지식 베이스 관리 |
| **Audit Logs** | 감사 로그 쿼리 및 검토용 보고서 생성 | 보안 모니터링, 규정 준수 보고 |
| **DNS Analytics** | DNS 성능 최적화 및 문제 디버깅 | DNS 문제 해결, 성능 최적화 |
| **Digital Experience Monitoring** | 조직의 중요 애플리케이션에 대한 인사이트 | 애플리케이션 성능, 사용자 경험 모니터링 |
| **Cloudflare One CASB** | SaaS 애플리케이션의 보안 구성 오류 식별 | 보안 상태, 규정 준수 확인 |
| **GraphQL** | Cloudflare의 GraphQL API를 사용한 분석 데이터 | 커스텀 분석, 데이터 시각화 |

## 전제 조건

- [Cloudflare 계정](https://dash.cloudflare.com/sign-up)
- 적절한 권한이 있는 [Cloudflare API 토큰](https://dash.cloudflare.com/profile/api-tokens)
- Node.js 설치 (`npx` 명령용)

## 구성

### 1단계: API 토큰 생성

1. [Cloudflare API 토큰](https://dash.cloudflare.com/profile/api-tokens)으로 이동
2. **"Create Token"** 클릭
3. 특정 권한을 위해 **"Custom token"** 또는 전체 액세스를 위해 **"Global API Key"** 선택
4. 사용하려는 MCP 서버에 따라 권한 구성:
   - **Zone:Read** - DNS, 분석 및 일반 존 정보용
   - **Zone:Edit** - 구성 변경용
   - **Account:Read** - 계정 수준 리소스용
   - **Workers:Read/Edit** - Workers 관련 서버용
   - **Logs:Read** - 관측성 및 감사 로그용

### 2단계: goose에 MCP 서버 추가

필요에 따라 하나 이상의 서버를 선택하세요. 가장 인기 있는 구성은 다음과 같습니다:

#### Observability 서버 (디버깅에 권장)

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  1. [설치 프로그램 실행](goose://extension?cmd=npx&arg=mcp-remote&arg=https%3A%2F%2Fobservability.mcp.cloudflare.com%2Fsse&id=cloudflare-observability&name=Cloudflare%20Observability&description=Debug%20and%20get%20insight%20into%20your%20application%27s%20logs%20and%20analytics&env=CLOUDFLARE_API_TOKEN%3DCloudflare%20API%20Token)
  2. `Yes`를 눌러 설치 확인
  3. Cloudflare API 토큰 입력
  4. `Save Configuration` 클릭
  5. 상단으로 스크롤하여 왼쪽 상단의 `Exit` 클릭
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Command-line Extension` 추가 선택
  3. 확장 프로그램 이름 입력: `cloudflare-observability`
  4. 명령 입력: `npx mcp-remote https://observability.mcp.cloudflare.com/mcp`
  5. 타임아웃 설정: `300`초
  6. 환경 변수 추가:
     - 이름: `CLOUDFLARE_API_TOKEN`
     - 값: Cloudflare API 토큰

  </TabItem>
</Tabs>

#### Workers Bindings 서버 (Workers 개발용)

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=mcp-remote&arg=https%3A%2F%2Fbindings.mcp.cloudflare.com%2Fmcp&id=cloudflare-bindings&name=Cloudflare%20Workers%20Bindings&description=Build%20Workers%20applications%20with%20storage%2C%20AI%2C%20and%20compute%20primitives&env=CLOUDFLARE_API_TOKEN%3DCloudflare%20API%20Token)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  명령: `npx mcp-remote https://bindings.mcp.cloudflare.com/mcp`
  </TabItem>
</Tabs>

#### Radar 서버 (트래픽 인사이트용)

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=mcp-remote&arg=https%3A%2F%2Fradar.mcp.cloudflare.com%2Fmcp&id=cloudflare-radar&name=Cloudflare%20Radar&description=Get%20global%20Internet%20traffic%20insights%2C%20trends%2C%20URL%20scans%2C%20and%20other%20utilities&env=CLOUDFLARE_API_TOKEN%3DCloudflare%20API%20Token)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  명령: `npx mcp-remote https://radar.mcp.cloudflare.com/mcp`
  </TabItem>
</Tabs>

## 사용 예제

### 애플리케이션 문제 디버깅

Observability 서버를 사용하여 Workers 애플리케이션의 성능 문제를 디버깅해 보겠습니다:

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::

#### goose 프롬프트
```
내 Workers 애플리케이션 "my-api-worker"에서 높은 오류율이 발생하고 있습니다. 도와주실 수 있나요:
1. 최근 오류 로그 확인
2. 가장 일반적인 오류 유형 식별
3. 성능 메트릭 분석
4. 데이터를 기반으로 최적화 제안
```

#### 예상 goose 출력
```
"my-api-worker" 애플리케이션의 문제를 디버깅하는 것을 도와드리겠습니다. 문제를 식별하기 위해 관측성 데이터를 수집하겠습니다.

먼저, worker의 최근 오류 로그를 확인하겠습니다:

[goose가 observability MCP 서버를 사용하여 오류 로그 가져오기]

worker에서 여러 문제를 발견했습니다:

**오류 분석:**
- 45%의 오류가 500 Internal Server Error
- 30%가 타임아웃 오류 (30초 CPU 제한 초과)
- 25%가 메모리 제한 초과 오류

**성능 메트릭:**
- 평균 응답 시간: 2.3초 (지난주 대비 150% 증가)
- CPU 사용량: 평균 85% (임계 임계값)
- 메모리 사용량: 평균 92% (제한 근접)

**식별된 근본 원인:**
1. 데이터베이스 연결 풀링 문제로 인한 타임아웃
2. 대용량 페이로드의 JSON 파싱에서 메모리 누수
3. 요청 유효성 검사에서 비효율적인 정규식 연산

**권장 최적화:**
1. 적절한 정리와 함께 연결 풀링 구현
2. 대용량 JSON 페이로드를 메모리에 로드하는 대신 스트리밍
3. 정규식을 더 효율적인 문자열 연산으로 대체
4. 자주 액세스하는 데이터에 캐싱 추가

이러한 수정 사항 중 구현을 도와드릴까요?
```

### Workers Bindings 관리

Workers Bindings 서버를 사용하여 스토리지 및 AI 기능 설정:

#### goose 프롬프트
```
다음을 포함하는 새 Workers 프로젝트를 설정해야 합니다:
1. 캐싱을 위한 KV 스토리지
2. 파일 업로드를 위한 R2 버킷
3. 텍스트 분석을 위한 AI 바인딩
4. 실시간 기능을 위한 Durable Object

이러한 바인딩을 구성하는 데 도움을 주실 수 있나요?
```

### Radar로 트래픽 분석

보안 및 트래픽 분석을 위한 Radar 서버 사용:

#### goose 프롬프트
```
내 도메인 example.com의 보안 상태를 분석하는 데 도움을 주실 수 있나요? 다음을 원합니다:
1. 보안 위협이나 악성 트래픽 확인
2. 글로벌 트래픽 패턴 분석
3. 취약점 스캔
4. 보안 개선을 위한 권장 사항 얻기
```

## 일반적인 사용 사례

### 1. 애플리케이션 디버깅
- **Observability 서버**: 오류, 성능 및 사용자 경험 모니터링
- **Logpush 서버**: 로그 패턴 및 데이터 파이프라인 상태 분석
- **DNS Analytics**: DNS 해결 문제 디버깅

### 2. 개발 및 배포
- **Workers Bindings**: 스토리지, AI 및 컴퓨팅 리소스 구성
- **Workers Builds**: 배포 상태 및 빌드 상태 모니터링
- **Container 서버**: 격리된 개발 환경 설정

### 3. 보안 및 규정 준수
- **Audit Logs**: 구성 변경 및 액세스 패턴 추적
- **Cloudflare One CASB**: SaaS 애플리케이션 보안 모니터링
- **Radar 서버**: 위협 인텔리전스 및 URL 스캐닝

### 4. 분석 및 인사이트
- **GraphQL 서버**: 커스텀 분석 및 보고
- **Digital Experience Monitoring**: 애플리케이션 성능 인사이트
- **AI Gateway**: AI 사용량 분석 및 최적화

### 5. 콘텐츠 및 웹 관리
- **Browser Rendering**: 웹 스크래핑 및 콘텐츠 분석
- **AutoRAG**: 문서 관리 및 검색
- **Documentation 서버**: API 참조 및 문제 해결

## 모범 사례

### 보안
- 최소한의 필수 권한으로 범위가 지정된 API 토큰 사용
- API 토큰을 정기적으로 교체
- 감사 로그를 통해 API 사용량 모니터링
- 비정상적인 활동에 대한 알림 설정

### 성능
- 다양한 작업에 적절한 타임아웃 값 사용
- 가능한 경우 자주 액세스하는 데이터 캐시
- 속도 제한 및 사용량 할당량 모니터링
- 적절한 오류 처리 및 재시도 구현

### 개발 워크플로우
- API 참조를 위해 Documentation 서버로 시작
- 격리된 테스트를 위해 Container 서버 사용
- 개발 중 Observability 서버로 모니터링
- 라이브 전에 Radar 서버로 분석

## 문제 해결

### 일반적인 문제

**인증 오류:**
- API 토큰에 올바른 권한이 있는지 확인
- 토큰이 만료되지 않았는지 확인
- 환경 변수에 토큰이 올바르게 설정되었는지 확인

**속도 제한:**
- Cloudflare 대시보드에서 API 사용량 모니터링
- 재시도에 지수 백오프 구현
- 더 높은 제한을 위해 플랜 업그레이드 고려

**연결 문제:**
- Cloudflare API에 대한 네트워크 연결 확인
- 방화벽 설정 확인
- 적절한 DNS 해결 확인

### 도움 받기

문제가 발생하면:

1. [Cloudflare MCP Server 저장소](https://github.com/cloudflare/mcp-server-cloudflare)에서 문서 확인
2. [Cloudflare API 문서](https://developers.cloudflare.com/api/) 검토
3. [Discord 커뮤니티](https://discord.gg/goose-oss)에서 지원 요청
4. 플랫폼별 도움을 위해 [Cloudflare 커뮤니티](https://community.cloudflare.com/) 확인

## 다음 단계

goose에서 Cloudflare MCP 서버를 활성화하면 다음을 수행할 수 있습니다:

- 자연어 쿼리로 애플리케이션 **모니터링 및 디버깅**
- 대화형 명령을 통한 **인프라 관리**
- 손쉬운 **보안** 및 성능 데이터 분석
- Cloudflare의 전체 플랫폼에서 **워크플로우 자동화**

현재 애플리케이션에 대한 인사이트를 얻기 위해 Observability 서버로 시작한 다음 특정 요구 사항에 따라 다른 서버로 확장해 보세요.
