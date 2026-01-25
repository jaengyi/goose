---
title: goose와 함께하는 고급 Cognee 사용법
description: 향상된 메모리 및 자동화를 위해 goose와 Cognee 지식 그래프를 사용하는 고급 패턴
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# goose와 함께하는 고급 Cognee 사용법

이 튜토리얼에서는 자동화된 메모리 관리, 지식 그래프 최적화 및 다양한 통합 전략을 포함하여 goose와 Cognee 확장 프로그램의 고급 사용 패턴을 다룹니다.

## 개요

기본 [Cognee MCP 설정](../mcp/cognee-mcp.md)으로 시작할 수 있지만, 이 튜토리얼에서는 goose가 지식 그래프를 자율적으로 사용하고 워크플로우를 최적화하는 방법을 탐구합니다.

## 주요 개념

### 지식 그래프 메모리
Cognee는 다음과 같은 구조화된 지식 그래프를 생성합니다:
- 대화, 문서, 이미지 및 오디오 전사를 상호 연결
- 30개 이상의 데이터 소스 지원
- 기존 RAG 시스템을 동적 관계 매핑으로 대체
- 복잡한 다중 홉 추론 가능

### 검색 유형
효과적인 사용을 위해 Cognee의 검색 유형을 이해하는 것이 중요합니다:

| 검색 유형 | 사용 사례 | 설명 |
|-------------|----------|-------------|
| `SUMMARIES` | 요약 요청 | 상위 수준 개요 |
| `INSIGHTS` | 관계 쿼리 | 엔티티 간 연결 |
| `CHUNKS` | 특정 사실 | 원시 텍스트 세그먼트 |
| `COMPLETION` | 설명 | LLM 생성 응답 |
| `GRAPH_COMPLETION` | 복잡한 관계 | 다중 홉 추론 |
| `GRAPH_SUMMARY` | 간결한 답변 | 짧고 집중된 응답 |
| `GRAPH_COMPLETION_COT` | 다중 홉 Q&A | 사고 연쇄 추론 |
| `GRAPH_CONTEXT_EXT` | 컨텍스트 확장 | 확장된 컨텍스트 |
| `CODE` | 코드 예제 | 프로그래밍 관련 쿼리 |

## 자동화 전략

<Tabs>
<TabItem value="method1" label="방법 1 (느림)" default>

### 지침 파일

세션 간 일관된 동작을 위해 지침 파일을 사용합니다. 이 방법은 토큰을 적게 사용하지만 시작이 느립니다.

`~/.config/goose/cognee-instructions.md` 생성:

````markdown
당신은 메모리용 Cognee 지식 그래프에 액세스할 수 있는 LLM 에이전트입니다.

**중요한 규칙:**
- `prune` 명령을 절대 호출하지 마세요
- 사용자 쿼리에 응답하기 전에 항상 메모리를 검색하세요
- 사용자에 대해 새로 알게 된 정보를 자동으로 cognify하세요

**메모리 워크플로우:**
1. **각 응답 전**: 지식 그래프 검색
   - 사용자 요청을 적절한 검색 유형에 매핑:
     - 요약 → SUMMARIES
     - 관계 → INSIGHTS
     - 특정 사실 → CHUNKS
     - 설명 → COMPLETION
     - 복잡한 관계 → GRAPH_COMPLETION
     - 코드 예제 → CODE

2. **검색 명령**:
   ```text
   cognee-mcp__search(\{
     search_query: "사용자 프롬프트",
     search_type: "매핑된 유형"
   \})
   ```

3. **결과를 응답에 통합**

**메모리 업데이트:**
- 사용자에 대한 새로운 사실, 선호도 또는 관계를 알게 되면
- 호출: `cognee-mcp__cognify(\{ data: "정보" \})`
- 모니터링: `cognee-mcp__cognify_status()`

**코드 분석:**
- 코드 저장소 분석 요청 시
- 사용: `cognee-mcp__codify(\{ repo_path: "경로" \})`
- `rg --files`가 반환하는 파일만 처리
````

지침으로 goose 시작:
```bash
goose run -i ~/.config/goose/cognee-instructions.md -s
```

</TabItem>
<TabItem value="method2" label="방법 2">

### goosehints 파일

더 높은 토큰 사용량으로 더 빠른 시작을 원하면 `.goosehints` 파일에 추가:

```text
COGNEE_MEMORY_SYSTEM:
영구 메모리용 Cognee 지식 그래프에 액세스할 수 있습니다.

MEMORY_RETRIEVAL_PROTOCOL:
- 응답하기 전에 요청 유형을 결정하고 검색 유형에 매핑
- 검색 유형: SUMMARIES, INSIGHTS, CHUNKS, COMPLETION, GRAPH_COMPLETION, CODE
- 항상 search_query 및 search_type 매개변수로 cognee-mcp__search 호출
- 메모리 결과를 응답에 통합

MEMORY_STORAGE_PROTOCOL:
- 새로운 사용자 사실, 선호도, 관계 자동 cognify
- data 매개변수로 cognee-mcp__cognify 호출
- prune 명령 사용 금지

CODE_ANALYSIS_PROTOCOL:
- 저장소: repo_path 매개변수로 cognee-mcp__codify
- rg --files 출력의 파일만 처리
```

</TabItem>
</Tabs>

### 전략 3: Memory MCP 통합

하이브리드 접근 방식을 위해 [Memory MCP 확장 프로그램](../mcp/memory-mcp.md)과 결합:

1. Cognee 사용 패턴을 메모리로 저장
2. Memory MCP를 사용하여 Cognee 검색 트리거
3. goosehints보다 낮은 토큰 사용량
4. 순수 지침 파일보다 안정적

## 고급 워크플로우

### 개발자 워크플로우

소프트웨어 개발 프로젝트용:

```bash
# Cognee와 함께 goose 시작
goose session

# goose에서 코드베이스 분석
> goose, 이 저장소를 codify하고 아키텍처를 이해하는 데 도움을 줘
```

goose가 수행하는 작업:
1. 저장소에서 `cognee-mcp__codify` 실행
2. 코드 지식 그래프 구축
3. 그래프를 사용하여 아키텍처 질문에 답변

### 연구 워크플로우

연구 및 문서화용:

```bash
# 연구 문서 cognify
> goose, 이 연구 논문들을 cognify해 줘: paper1.pdf, paper2.pdf, paper3.pdf

# 나중에 관계 쿼리
> 이 논문들의 방법론 간 연결은 무엇인가요?
```

### 개인 비서 워크플로우

개인 생산성용:

```bash
# 선호도 저장
> 나는 오전 회의를 선호하고, 2시간 집중 블록으로 가장 잘 일하며, 통화 사이에 15분 휴식이 필요하다는 것을 기억해 줘

# 나중에 쿼리
> 내 선호도에 따라 내일 일정을 어떻게 구성해야 할까요?
```

## 성능 최적화

### 서버 구성

최적의 성능을 위해 Cognee를 별도의 서버로 실행:

```bash
# 최적화된 시작 스크립트 생성
cat > start-cognee-optimized.sh << 'EOF'
#!/bin/bash
set -e

# 성능 설정
export DEBUG=false
export LOG_LEVEL=WARNING
export RATE_LIMIT_INTERVAL=30

# 모델 구성
export LLM_API_KEY=${OPENAI_API_KEY}
export LLM_MODEL=openai/gpt-4o-mini  # 더 빠르고 저렴한 모델
export EMBEDDING_API_KEY=${OPENAI_API_KEY}
export EMBEDDING_MODEL=openai/text-embedding-3-small  # 더 빠른 임베딩

# 서버 설정
export HOST=0.0.0.0
export PORT=8000

cd /path/to/cognee-mcp
uv run python src/server.py --transport sse
EOF

chmod +x start-cognee-optimized.sh
```

### 메모리 관리

지식 그래프 모니터링 및 관리:

```bash
# 상태 확인
> goose, cognify 파이프라인 상태는 어떻게 돼?

# 선택적 정리 (필요한 경우)
> goose, 지식 그래프에서 오래된 정보를 식별하는 것을 도와줄 수 있어?
```

## 문제 해결

### 일반적인 문제

1. **느린 시작**: 방법 2 (별도 서버) 구성 사용
2. **메모리가 유지되지 않음**: 파일 권한 및 경로 확인
3. **검색이 빈 결과 반환**: 데이터가 제대로 cognify되었는지 확인
4. **높은 토큰 사용량**: goosehints 대신 지침 파일 사용

### 디버그 명령

```bash
# Cognee 로그 확인
tail -f ~/.local/share/cognee/logs/cognee.log

# 서버 연결 테스트
curl http://localhost:8000/health

# 지식 그래프 상태 확인
# goose 세션에서:
> goose, cognify_status와 codify_status 실행해 줘
```

## 모범 사례

### 데이터 구성

1. **노드셋 사용**으로 다양한 유형의 정보 구성:
   ```bash
   # 개발자 규칙
   > goose, 이 코딩 표준을 'developer_rules' 노드셋에 추가해 줘

   # 프로젝트별 정보
   > goose, 이 프로젝트 문서를 'project_alpha' 노드셋으로 cognify해 줘
   ```

2. **정기적인 유지 관리**:
   - 저장된 정보를 월간으로 검토 및 업데이트
   - 오래된 선호도 및 사실 제거
   - 사용 패턴에 따라 검색 쿼리 최적화

### 통합 패턴

1. **계층화 접근 방식**: Memory MCP와 Cognee를 다른 목적으로 사용
2. **컨텍스트 전환**: 다른 워크플로우에 다른 지침 파일
3. **선택적 자동화**: 모든 상호 작용에 지식 그래프 쿼리가 필요한 것은 아님

## 예제

### 코드 리뷰 어시스턴트

```bash
# 설정
> goose, 이 저장소를 codify하고 내가 함수형 프로그래밍 패턴, 포괄적인 테스트, 명확한 문서화를 선호한다는 것을 기억해 줘

# 사용
> 이 풀 리퀘스트를 검토하고 내 코딩 선호도에 맞는지 확인해 줘
```

### 회의 어시스턴트

```bash
# 회의 전
> goose, 이 문서들에서 안건과 참가자 배경을 cognify해 줘

# 회의 중/후
> 지식 그래프를 기반으로 주요 실행 항목과 이전 논의와 어떻게 관련되는지 알려줘
```

### 연구 어시스턴트

```bash
# 문헌 검토
> goose, 이 10개의 연구 논문을 cognify하고 방법론 간의 관계에 대한 지식 그래프를 생성해 줘

# 종합
> 연구에서 떠오르는 패턴과 어떤 갭이 존재하나요?
```

이 고급 사용 가이드는 정교한 지식 관리 및 자동화 워크플로우를 위해 goose와 Cognee의 잠재력을 최대화하는 데 도움이 될 것입니다.
