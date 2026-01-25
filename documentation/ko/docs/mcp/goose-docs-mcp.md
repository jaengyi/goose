---
title: goose 문서 확장 기능

description: goose 문서 MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';


이 튜토리얼은 [goose 문서 MCP 서버](https://github.com/idosal/git-mcp)를 goose 확장 기능으로 추가하여 goose가 자신에 대한 질문에 답할 수 있게 하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=mcp-remote&arg=https%3A%2F%2Fblock.gitmcp.io%2Fgoose%2F&id=goose-docs&name=goose%20Docs&description=gitmcp%20for%20goose%20documentation)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx mcp-remote https://block.gitmcp.io/goose/
  ```
  </TabItem>
</Tabs>
:::

## 구성

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
    <GooseDesktopInstaller
      extensionId="goose-docs"
      extensionName="goose Docs"
      description="goose 문서용 GitMCP"
      command="npx"
      args={["mcp-remote", "https://block.gitmcp.io/goose/"]}
      cliCommand="npx mcp-remote https://block.gitmcp.io/goose/"
      timeout={300}
      note="이 명령을 실행하려면 시스템에 Node.js가 설치되어 있어야 합니다(npx 사용)."
    />
 </TabItem>
  <TabItem value="cli" label="goose CLI">
      <CLIExtensionInstructions
        name="goose Docs"
        description="goose 문서용 GitMCP"
        command="npx mcp-remote https://block.gitmcp.io/goose/"
        timeout={300}
      />
  </TabItem>
</Tabs>


## 사용 예시
GitMCP를 사용하면 모든 Git 저장소를 goose 확장 기능으로 변환할 수 있습니다. goose 문서 MCP 서버는 goose가 자신에 대한 질문에 답할 수 있게 해주는 좋은 예입니다.

goose에게 레시피를 만들어달라고 요청해 봅시다! 관련 정보를 찾기 위해 goose 문서 MCP 서버를 사용할 것입니다.


### goose 프롬프트

```
goose야, 개발 팀이 사용할 수 있는 자동화된 코드 리뷰 레시피를 만들고 싶어. goose 문서를 보고 레시피 형식을 이해한 다음 다음과 같은 레시피를 만들어줘:


1. 다음에 대한 풀 리퀘스트 검토:

- 코드 품질 문제 및 모범 사례
- 보안 취약점
- 누락된 단위 테스트
- 성능 문제

2. 팀이 사용자 정의할 수 있도록 매개변수화:

- 프로그래밍 언어 (Python, JavaScript 등)
- 최소 테스트 커버리지 임계값
- 적용할 특정 코딩 표준

3. CI/CD 시스템에서 파싱할 수 있는 구조화된 결과 출력

먼저 goose 문서 확장 기능을 사용하여 레시피 형식을 조사한 다음 적절한 YAML 구조, 매개변수 및 필요한 확장 기능이 포함된 완전한 레시피 파일을 만들어줘.
개발 팀이 워크플로우에서 즉시 사용할 수 있는 것으로 만들어줘.
```


### goose 출력

:::note 데스크톱

완벽합니다! goose 문서 조사를 기반으로 포괄적인 자동화된 코드 리뷰 레시피를 만들었습니다. 개발 팀을 위해 만든 것은 다음과 같습니다:

#### 📋 레시피 개요

**자동화된 코드 리뷰** 레시피(`automated-code-review.yaml`)는 다음을 제공합니다:

#### ✅ **핵심 검토 영역**
1. **코드 품질 및 모범 사례** - 아키텍처, 명명, 복잡성, 문서화
2. **보안 취약점 평가** - OWASP Top 10, 입력 유효성 검사, 인증 문제
3. **테스트 커버리지 분석** - 누락된 테스트, 커버리지 임계값, 테스트 품질
4. **성능 문제** - 병목 현상, 알고리즘 복잡성, 리소스 관리
5. **기술 부채 평가** - 리팩토링 필요, 유지보수성 문제

#### 🔧 **매개변수화 기능**
- **`programming_language`** - Python, JavaScript, Java, Go, Rust, TypeScript 등
- **`min_test_coverage`** - 사용자 정의 가능한 임계값 (기본값: 80%)
- **`coding_standards`** - PEP8, ESLint, Google Style Guide, 회사별
- **`security_focus`** - 기본, 표준 또는 포괄적인 보안 분석
- **`performance_analysis`** - 성능 검토 활성화/비활성화
- **`exclude_patterns`** - 특정 파일/디렉토리 건너뛰기

#### 📊 **구조화된 CI/CD 출력**
레시피는 다음을 포함하는 포괄적인 JSON 구조를 출력합니다:
- **전체 품질 점수** 및 권장 사항 (승인/변경 요청/거부)
- **심각도 수준 및 특정 파일 위치가 있는 분류된 문제**
- **CWE ID 및 수정 단계가 있는 보안 취약점**
- **테스트 커버리지 메트릭** 및 누락된 테스트 권장 사항
- **최적화 제안이 있는 성능 문제**
- **노력 추정이 있는 기술 부채 평가**
- **파이프라인 통합을 위한 CI/CD 메타데이터**

#### 🚀 **사용 예시**

#### 기본 사용법:
```bash
goose run automated-code-review.yaml \
  --repository_path ./my-project \
  --programming_language Python
```

#### 고급 사용법:
```bash
goose run automated-code-review.yaml \
  --repository_path ./my-project \
  --programming_language JavaScript \
  --min_test_coverage 85 \
  --coding_standards ESLint \
  --security_focus comprehensive \
  --exclude_patterns "*.test.js,dist/*,node_modules/*"
```

#### 🔧 **CI/CD 통합**

구조화된 JSON 출력은 CI/CD 파이프라인에 완벽합니다:

```yaml
# GitHub Actions 통합 예시
- name: Run Code Review
  run: |
    goose run automated-code-review.yaml \
      --repository_path . \
      --programming_language ${{ matrix.language }} \
      --output review-results.json

- name: Process Results
  run: |
    # 파이프라인 결정을 위해 JSON 결과 파싱
    RECOMMENDATION=$(jq -r '.review_summary.recommendation' review-results.json)
    if [ "$RECOMMENDATION" = "reject" ]; then
      exit 1
    fi
```

#### 📁 **파일 구조**

레시피는 현재 디렉토리에 `automated-code-review.yaml`로 저장되며 다음을 포함합니다:
- goose 레시피 형식을 따르는 적절한 YAML 구조
- 기본값이 있는 포괄적인 매개변수 정의
- 파일 시스템 액세스를 위한 내장 developer 확장 기능
- 구조화된 출력을 위한 상세한 JSON 스키마
- 매개변수 대체를 위한 템플릿 구문

개발 팀은 이 레시피를 프로젝트 저장소에 배치하고 특정 요구 사항에 맞게 매개변수를 사용자 정의하여 즉시 사용을 시작할 수 있습니다!
:::

### 생성된 레시피 파일
<details>
<summary>automated-code-review.yaml</summary>

```yaml
version: "1.0.0"
title: "자동화된 코드 리뷰"
description: "사용자 정의 가능한 표준과 구조화된 CI/CD 출력을 갖춘 풀 리퀘스트를 위한 포괄적인 자동화된 코드 리뷰"

instructions: |
  당신은 소프트웨어 엔지니어링 모범 사례, 보안 취약점 및 테스트 방법론에 대한 깊은 지식을 가진 전문 코드 리뷰어입니다.
  당신의 역할은 다음에 초점을 맞춘 철저한 코드 리뷰를 수행하는 것입니다:

  1. 코드 품질 및 모범 사례
  2. 보안 취약점
  3. 테스트 커버리지 분석
  4. 성능 문제
  5. 유지보수성 문제

  항상 개선을 위한 구체적인 예시와 제안과 함께 실행 가능한 피드백을 제공하세요.
  지정된 프로그래밍 언어 규칙과 코딩 표준을 고려하세요.

# ... (나머지 레시피 내용은 원본과 동일)
```

</details>
