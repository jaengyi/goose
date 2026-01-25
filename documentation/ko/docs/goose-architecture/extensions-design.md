---
sidebar_position: 2
---

# 확장 기능 설계

이 문서는 goose의 [확장 기능 프레임워크](/docs/getting-started/using-extensions) 설계와 구현을 설명합니다. 이를 통해 AI 에이전트가 통합된 도구 기반 인터페이스를 통해 다양한 확장 기능과 상호작용할 수 있습니다.

## 핵심 개념

### Extension
Extension은 AI 에이전트가 작동할 수 있는 모든 구성요소를 나타냅니다. 확장 기능은 도구를 통해 기능을 노출하고 자체 상태를 유지합니다. 핵심 인터페이스는 `Extension` 트레이트로 정의됩니다:

```rust
#[async_trait]
pub trait Extension: Send + Sync {
    fn name(&self) -> &str;
    fn description(&self) -> &str;
    fn instructions(&self) -> &str;
    fn tools(&self) -> &[Tool];
    async fn status(&self) -> AnyhowResult<HashMap<String, Value>>;
    async fn call_tool(&self, tool_name: &str, parameters: HashMap<String, Value>) -> ToolResult<Value>;
}
```

### 도구
도구는 확장 기능이 에이전트에게 기능을 노출하는 주요 방법입니다. 각 도구에는 다음이 있습니다:
- 이름
- 설명
- 파라미터 집합
- 도구의 기능을 실행하는 구현

도구는 Value를 받아 `AgentResult<Value>`를 반환해야 합니다(비동기여야 함). 이것이 에이전트의 도구 호출 프레임워크와 호환되게 합니다.

```rust
async fn echo(&self, params: Value) -> AgentResult<Value>
```

## 아키텍처

### 구성요소 개요

1. **Extension 트레이트**: 모든 확장 기능이 구현해야 하는 핵심 인터페이스
2. **오류 처리**: 도구 실행을 위한 특수 오류 유형
3. **프로시저 매크로**: 도구 정의 및 등록 단순화 [*아직 구현되지 않음*]

### 오류 처리

시스템은 두 가지 주요 오류 유형을 사용합니다:
- `ErrorData`: 도구 실행과 관련된 특정 오류
- `anyhow::Error`: 확장 기능 상태 및 기타 작업을 위한 범용 오류

이 분리를 통해 도구 실행에 대한 정밀한 오류 처리가 가능하면서 일반 확장 기능 작업에 대한 유연성을 유지합니다.

## 모범 사례

### 도구 설계

1. **명확한 이름**: 도구에 명확하고 작업 지향적인 이름 사용(예: "user"가 아닌 "create_user")
2. **설명적인 파라미터**: 각 파라미터에 명확한 설명이 있어야 함
3. **오류 처리**: 가능한 경우 특정 오류 반환, 오류는 "프롬프트"가 됨
4. **상태 관리**: 상태 수정에 대해 명시적으로 표현

### 확장 기능 구현

1. **상태 캡슐화**: 확장 기능 상태를 비공개로 유지하고 제어
2. **오류 전파**: 도구 실행에 `ErrorData`와 함께 `?` 연산자 사용
3. **상태 명확성**: 명확하고 구조화된 상태 정보 제공
4. **문서화**: 모든 도구와 그 효과를 문서화

### 예시 구현

다음은 간단한 확장 기능의 완전한 예시입니다:

```rust
use goose_macros::tool;

struct FileSystem {
    registry: ToolRegistry,
    root_path: PathBuf,
}

impl FileSystem {
    #[tool(
        name = "read_file",
        description = "Read contents of a file"
    )]
    async fn read_file(&self, path: String) -> ToolResult<Value> {
        let full_path = self.root_path.join(path);
        let content = tokio::fs::read_to_string(full_path)
            .await
            .map_err(|e| ErrorData {
                code: ErrorCode::INTERNAL_ERROR,
                message: Cow::from(e.to_string(),
                data: None,
            }))?;

        Ok(json!({ "content": content }))
    }
}

#[async_trait]
impl Extension for FileSystem {
    // ... 트레이트 메서드 구현 ...
}
```

## 테스트

확장 기능은 여러 수준에서 테스트되어야 합니다:
1. 개별 도구에 대한 단위 테스트
2. 확장 기능 동작에 대한 통합 테스트
3. 도구 불변성에 대한 속성 테스트

예시 테스트:
```rust
#[tokio::test]
async fn test_echo_tool() {
    let extension = TestExtension::new();
    let result = extension.call_tool(
        "echo",
        hashmap!{ "message" => json!("hello") }
    ).await;

    assert_eq!(result.unwrap(), json!({ "response": "hello" }));
}
```
