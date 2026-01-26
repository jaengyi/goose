# 폐쇄망 환경에서 goose 사용 가이드

이 문서는 인터넷이 연결되지 않는 폐쇄망(Air-gapped) 환경에서 goose를 사용하여 Java, JavaScript, HTML, JSP, WebSquare 등의 소스 코드를 분석하고 문서화하는 방법을 설명합니다.

## 목차

1. [사전 준비](#사전-준비)
2. [로컬 LLM 설치](#로컬-llm-설치)
3. [goose 설치 및 설정](#goose-설치-및-설정)
4. [MCP 확장 기능 구성](#mcp-확장-기능-구성)
5. [소스 코드 분석 및 문서화 작업](#소스-코드-분석-및-문서화-작업)
6. [WebSquare 분석 팁](#websquare-분석-팁)
7. [문제 해결](#문제-해결)

---

## 사전 준비

### 필수 요구 사항

- **운영체제**: Linux, macOS, 또는 Windows
- **RAM**: 최소 16GB (권장 32GB 이상)
- **저장 공간**: 최소 20GB (LLM 모델 저장용)
- **GPU** (선택): NVIDIA GPU가 있으면 추론 속도 향상

### 폐쇄망 환경 준비 절차

폐쇄망으로 이동하기 전에 인터넷이 연결된 환경에서 다음 파일들을 미리 다운로드해야 합니다:

1. **goose 바이너리**
2. **Ollama 설치 파일**
3. **LLM 모델 파일**
4. **Node.js** (MCP 서버 일부에 필요)

---

## 로컬 LLM 설치

### Ollama 설치 (권장)

Ollama는 로컬에서 LLM을 실행할 수 있는 가장 간단한 방법입니다.

#### 1. Ollama 다운로드 (인터넷 연결 환경에서)

```bash
# Linux
curl -fsSL https://ollama.com/install.sh | sh

# macOS
# https://ollama.com/download 에서 설치 파일 다운로드

# Windows
# https://ollama.com/download 에서 설치 파일 다운로드
```

#### 2. 모델 다운로드 (인터넷 연결 환경에서)

코드 분석에 적합한 모델을 다운로드합니다:

```bash
# 권장 모델 (코드 분석에 최적화)
ollama pull qwen3                    # 기본 모델 (약 4.7GB)
ollama pull qwen2.5-coder:7b        # 코드 특화 모델 (약 4.7GB)
ollama pull codellama:13b            # 코드 특화 모델 (약 7.4GB)
ollama pull deepseek-coder:6.7b     # 코드 분석 특화 (약 3.8GB)

# 대형 모델 (32GB+ RAM 필요)
ollama pull qwen2.5-coder:32b       # 고성능 코드 모델 (약 19GB)
```

#### 3. 모델 파일 백업

다운로드한 모델은 다음 위치에 저장됩니다:

```bash
# Linux/macOS
~/.ollama/models/

# Windows
C:\Users\<username>\.ollama\models\
```

이 폴더 전체를 USB 또는 외장 하드에 복사하여 폐쇄망 환경으로 이동합니다.

#### 4. 폐쇄망에서 Ollama 실행

```bash
# Ollama 서버 시작
ollama serve

# 다른 터미널에서 모델 확인
ollama list
```

### 대안: Ramalama (컨테이너 기반)

OCI 컨테이너 런타임을 사용하는 대안입니다:

```bash
ramalama serve --runtime-args="--jinja" qwen3
# 기본 포트: 8080
```

---

## goose 설치 및 설정

### 1. goose 설치

#### 바이너리 설치 (권장)

인터넷 환경에서 goose 바이너리를 다운로드하고 폐쇄망으로 복사합니다:

```bash
# GitHub Releases에서 최신 바이너리 다운로드
# https://github.com/block/goose/releases

# 실행 권한 부여
chmod +x goose
sudo mv goose /usr/local/bin/
```

#### 소스에서 빌드

소스 코드를 폐쇄망에서 빌드하려면 Rust 툴체인도 함께 이동해야 합니다:

```bash
# 의존성 캐시와 함께 빌드
source bin/activate-hermit
cargo build --release
```

### 2. goose 설정

#### 환경 변수 설정

```bash
# ~/.bashrc 또는 ~/.zshrc에 추가
export GOOSE_PROVIDER="ollama"
export GOOSE_MODEL="qwen2.5-coder:7b"
export OLLAMA_HOST="localhost:11434"
export OLLAMA_TIMEOUT=600

# 키링 비활성화 (서버 환경에서 필요)
export GOOSE_DISABLE_KEYRING=1
```

#### 설정 파일 생성

`~/.config/goose/config.yaml` 파일을 생성합니다:

```yaml
# goose 설정 파일
GOOSE_PROVIDER: "ollama"
GOOSE_MODEL: "qwen2.5-coder:7b"
OLLAMA_HOST: "localhost:11434"

# 확장 기능 설정
extensions:
  developer:
    bundled: true
    enabled: true
    name: developer
    timeout: 300
    type: builtin

  memory:
    bundled: true
    enabled: true
    name: memory
    timeout: 300
    type: builtin
```

#### 대화형 설정

```bash
goose configure
# 메뉴에서 선택:
# 1. Configure Providers > Ollama
# 2. OLLAMA_HOST 입력 (예: localhost:11434)
# 3. 사용할 모델 선택
```

### 3. Tool Shim 설정 (도구 호출 지원이 약한 모델용)

일부 로컬 모델은 도구 호출(tool calling) 기능이 약할 수 있습니다. 이 경우 Tool Shim을 활성화합니다:

```bash
# 추가 인터프리터 모델 다운로드 (인터넷 환경에서)
ollama pull llama3.2

# 환경 변수 설정
export GOOSE_TOOLSHIM=1
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
```

---

## MCP 확장 기능 구성

### 기본 내장 확장 기능

goose에는 코드 분석에 유용한 내장 MCP 서버가 포함되어 있습니다:

| 확장 기능 | 설명 | 용도 |
|----------|------|------|
| developer | 파일 읽기/쓰기, 코드 분석 | 소스 코드 분석 및 수정 |
| memory | 세션 메모리 관리 | 분석 컨텍스트 유지 |

### 확장 기능 활성화 확인

```bash
goose configure
# Extensions 메뉴에서 확인
```

### 커스텀 MCP 서버 추가 (선택)

프로젝트 특화 도구가 필요한 경우 커스텀 MCP 서버를 구성할 수 있습니다:

```yaml
# ~/.config/goose/config.yaml에 추가
extensions:
  my-custom-tool:
    name: my-custom-tool
    type: stdio
    cmd: /path/to/custom-mcp-server
    timeout: 300
```

---

## 소스 코드 분석 및 문서화 작업

### 세션 시작

```bash
# 분석할 프로젝트 디렉토리로 이동
cd /path/to/your/project

# goose 세션 시작
goose session
```

### Java 소스 분석 예시

```
# goose 세션 내에서:

이 프로젝트의 Java 소스 코드 구조를 분석해줘.
src/main/java 디렉토리의 패키지 구조와 주요 클래스들을 파악하고,
클래스 다이어그램을 텍스트로 작성해줘.
```

### JavaScript/HTML/JSP 분석 예시

```
# goose 세션 내에서:

webapp 폴더의 JSP 파일들을 분석해서:
1. 각 페이지의 기능을 요약
2. 포함된 JavaScript 함수 목록
3. 사용된 공통 컴포넌트 파악
결과를 docs/page-analysis.md 파일로 작성해줘.
```

### 문서 자동 생성

```
# goose 세션 내에서:

다음 작업을 수행해줘:
1. src 폴더의 모든 Java 클래스를 스캔
2. 각 클래스의 public 메서드 시그니처 추출
3. 메서드별 기능 설명을 추론하여 작성
4. 결과를 Markdown 형식의 API 문서로 생성
5. docs/api-reference.md 파일로 저장
```

### 코드 리팩토링 지원

```
# goose 세션 내에서:

UserController.java 파일을 분석하고:
1. 코드 품질 이슈 식별
2. 리팩토링 제안 사항 정리
3. 개선된 코드 예시 작성
```

---

## WebSquare 분석 팁

WebSquare는 한국에서 사용되는 UI 프레임워크입니다. 다음과 같이 분석할 수 있습니다:

### WebSquare XML 분석

```
# goose 세션 내에서:

이 프로젝트의 WebSquare 화면 정의 파일(.xml)들을 분석해줘:
1. 화면 ID와 제목 목록
2. 사용된 컴포넌트 종류 통계
3. 이벤트 핸들러 함수 매핑
4. 서비스 호출 목록 (submission)
```

### WebSquare JavaScript 분석

```
# goose 세션 내에서:

WebSquare 화면의 JavaScript 코드를 분석해서:
1. 초기화 함수 (scwin.onpageload) 로직 파악
2. 버튼 클릭 이벤트 핸들러 정리
3. 서버 통신 로직 (submission 콜백) 분석
4. 공통 함수 호출 관계 다이어그램 작성
```

### WebSquare 화면 문서화

```
# goose 세션 내에서:

화면정의서를 작성해줘:
- 화면 ID: SCR001
- 파일 위치: /ui/screen/SCR001.xml
포함 내용:
1. 화면 개요
2. 레이아웃 구조
3. 주요 컴포넌트 설명
4. 이벤트 처리 흐름
5. 연동 서비스 목록
```

---

## 문제 해결

### Ollama 연결 오류

```
Error: Cannot connect to Ollama
```

해결:
```bash
# Ollama 서비스 상태 확인
systemctl status ollama  # Linux (systemd 사용 시)
ollama list              # 실행 중인지 확인

# 수동 시작
ollama serve
```

### 모델 로드 실패

```
Error: Model not found
```

해결:
```bash
# 사용 가능한 모델 확인
ollama list

# 모델 경로 확인 (모델 파일이 올바른 위치에 있는지)
ls ~/.ollama/models/
```

### 메모리 부족

```
Error: Out of memory
```

해결:
- 더 작은 모델 사용 (예: 7B 대신 3B 모델)
- 컨텍스트 길이 줄이기:
  ```bash
  export OLLAMA_CONTEXT_LENGTH=8192
  ```
- 다른 애플리케이션 종료

### Tool Calling 오류

일부 로컬 모델은 도구 호출을 지원하지 않을 수 있습니다:

```bash
# Tool Shim 활성화
export GOOSE_TOOLSHIM=1
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
```

### 한글 인코딩 문제

```bash
# 환경 설정
export LANG=ko_KR.UTF-8
export LC_ALL=ko_KR.UTF-8
```

---

## 권장 워크플로우

### 1. 프로젝트 초기 분석

```bash
cd /path/to/project
goose session

# 첫 번째 요청
"이 프로젝트의 전체 구조를 분석하고
docs/project-overview.md 파일로 정리해줘."
```

### 2. 모듈별 상세 분석

```
"src/main/java/com/example/service 패키지의
모든 서비스 클래스를 분석하고
docs/services.md 파일로 문서화해줘."
```

### 3. 코드 수정 및 개선

```
"OrderService.java의 processOrder 메서드에
입력값 검증 로직을 추가해줘."
```

### 4. 테스트 코드 생성

```
"OrderService.java에 대한
단위 테스트 코드를 작성해줘."
```

---

## 성능 최적화 팁

### 모델 선택 가이드

| 작업 유형 | 권장 모델 | 최소 RAM |
|----------|----------|----------|
| 간단한 코드 리뷰 | qwen3 (8B) | 16GB |
| 복잡한 코드 분석 | qwen2.5-coder:7b | 16GB |
| 대규모 리팩토링 | qwen2.5-coder:32b | 32GB |
| 문서 생성 | codellama:13b | 16GB |

### 컨텍스트 길이 조정

대용량 파일 분석 시:

```bash
# 컨텍스트 길이 증가 (RAM 사용량 증가)
export OLLAMA_CONTEXT_LENGTH=32768
```

### GPU 활용

NVIDIA GPU가 있는 경우 자동으로 활용됩니다:

```bash
# GPU 사용량 확인
nvidia-smi
```

---

## 참고 자료

- [goose 공식 문서](https://block.github.io/goose/)
- [Ollama 모델 라이브러리](https://ollama.com/library)
- [MCP 프로토콜 문서](https://modelcontextprotocol.io/)
