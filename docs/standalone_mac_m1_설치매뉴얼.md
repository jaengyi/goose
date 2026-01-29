# macOS (Apple Silicon M1/M2/M3/M4) 환경에서 goose 설치 매뉴얼

이 문서는 인터넷이 연결된 macOS Apple Silicon 환경에서 goose를 설치하고 로컬 LLM과 함께 사용하는 방법을 단계별로 설명합니다.

## 목차

1. [사전 준비 사항](#1-사전-준비-사항)
2. [Ollama 설치](#2-ollama-설치)
3. [LLM 모델 다운로드](#3-llm-모델-다운로드)
4. [goose 설치](#4-goose-설치)
5. [goose 설정](#5-goose-설정)
6. [Tool Shim 설정](#6-tool-shim-설정)
7. [동작 확인](#7-동작-확인)
8. [소스 코드 분석 활용 예시](#8-소스-코드-분석-활용-예시)
9. [문제 해결](#9-문제-해결)

---

## 1. 사전 준비 사항

### 하드웨어 요구 사항

| 항목 | 최소 사양 | 권장 사양 |
|------|----------|----------|
| 칩셋 | Apple M1 | M1 Pro / M2 / M3 / M4 이상 |
| OS | macOS 13 Ventura | macOS 14 Sonoma 이상 |
| RAM (통합 메모리) | 16GB | 32GB 이상 |
| 저장 공간 | 20GB 여유 | 50GB 이상 여유 |

> **Apple Silicon 참고**: M1/M2/M3/M4 칩은 GPU와 CPU가 통합 메모리를 공유합니다. Ollama는 Apple Silicon의 Metal GPU 가속을 자동으로 활용하므로 별도의 GPU 설정이 필요 없습니다.

---

## 2. Ollama 설치

Ollama는 로컬에서 LLM을 실행할 수 있는 가장 간단한 방법입니다.

### 방법 1: 공식 사이트에서 다운로드 (권장)

1. https://ollama.com/download/mac 에서 `Ollama-darwin.zip`을 다운로드합니다.
2. 압축을 풀고 `Ollama.app`을 `/Applications` 폴더로 이동합니다.
3. Launchpad 또는 Spotlight(Cmd+Space)에서 **Ollama**를 실행합니다.

### 방법 2: Homebrew로 설치

```bash
brew install ollama
```

### 설치 확인

Ollama를 실행하면 메뉴 바에 아이콘이 나타납니다. 터미널에서 확인:

```bash
ollama --version
```

---

## 3. LLM 모델 다운로드

터미널에서 코드 분석에 적합한 모델을 다운로드합니다:

```bash
# 코드 특화 모델 - 권장
ollama pull qwen2.5-coder:7b        # 코드 특화 모델 (약 4.7GB)

# 기타 선택지 (용도에 맞게 추가 다운로드)
ollama pull qwen3                    # 범용 모델 (약 4.7GB)
ollama pull codellama:13b            # 코드 특화 모델 (약 7.4GB)
ollama pull deepseek-coder:6.7b     # 코드 분석 특화 (약 3.8GB)

# Tool Shim용 모델 (도구 호출 보조, 6단계 참고)
ollama pull llama3.2

# 대형 모델 (32GB+ 통합 메모리 환경에서만)
ollama pull qwen2.5-coder:32b       # 고성능 코드 모델 (약 19GB)
```

다운로드 완료 후 확인:

```bash
ollama list
```

---

## 4. goose 설치

### 방법 1: 설치 스크립트 (권장)

```bash
curl -fsSL https://github.com/block/goose/releases/latest/download/download_cli.sh | sh
```

### 방법 2: GitHub Releases에서 직접 다운로드

1. https://github.com/block/goose/releases 에서 `goose-aarch64-apple-darwin.zip` 파일을 다운로드합니다.

   > **주의**: Intel Mac용인 `x86_64`가 아닌, **aarch64** (ARM64) 빌드를 선택해야 합니다.

2. 압축 해제 및 설치:

   ```bash
   unzip goose-aarch64-apple-darwin.zip
   chmod +x goose
   sudo mv goose /usr/local/bin/
   ```

### 방법 3: Homebrew로 설치

```bash
brew install block/tap/goose
```

### macOS Gatekeeper 허용

바이너리 직접 다운로드 시 실행이 차단될 수 있습니다:

```bash
sudo xattr -d com.apple.quarantine /usr/local/bin/goose
```

### 설치 확인

```bash
goose --version
```

---

## 5. goose 설정

### 5-1. 환경 변수 설정

macOS의 기본 셸은 zsh입니다. `~/.zshrc` 파일에 환경 변수를 추가합니다:

```bash
cat >> ~/.zshrc << 'EOF'

# === goose 설정 ===
export GOOSE_PROVIDER="ollama"
export GOOSE_MODEL="qwen2.5-coder:7b"
export OLLAMA_HOST="localhost:11434"
export OLLAMA_TIMEOUT=600
EOF

source ~/.zshrc
```

### 5-2. goose 설정 파일 생성

```bash
mkdir -p ~/.config/goose

cat > ~/.config/goose/config.yaml << 'EOF'
GOOSE_PROVIDER: "ollama"
GOOSE_MODEL: "qwen2.5-coder:7b"
OLLAMA_HOST: "localhost:11434"

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
EOF
```

### 5-3. 대화형 설정 (선택)

터미널에서 대화형으로 설정할 수도 있습니다:

```bash
goose configure
```

메뉴에서 다음을 선택합니다:

1. **Configure Providers** > **Ollama** 선택
2. **OLLAMA_HOST** 입력: `localhost:11434`
3. 사용할 모델 선택 (예: `qwen2.5-coder:7b`)

---

## 6. Tool Shim 설정

일부 로컬 모델은 도구 호출(tool calling) 기능이 약할 수 있습니다. 이 경우 Tool Shim을 활성화하여 별도의 경량 모델이 도구 호출을 해석하도록 합니다.

```bash
# llama3.2 모델이 없으면 다운로드
ollama pull llama3.2

# ~/.zshrc에 추가
cat >> ~/.zshrc << 'EOF'

# === Tool Shim 설정 ===
export GOOSE_TOOLSHIM=1
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
EOF

source ~/.zshrc
```

---

## 7. 동작 확인

### 7-1. Ollama 동작 확인

```bash
# 모델 목록 확인
ollama list

# 모델 테스트
ollama run qwen2.5-coder:7b "Hello, world!"
```

### 7-2. goose 동작 확인

```bash
# goose 세션 시작
goose session

# 세션 내에서 간단한 테스트
# > 안녕하세요. 현재 디렉토리의 파일 목록을 보여주세요.
```

정상적으로 응답이 오면 설치가 완료된 것입니다. `Ctrl+C`로 세션을 종료합니다.

---

## 8. 소스 코드 분석 활용 예시

### 8-1. 프로젝트 분석 시작

```bash
# 분석할 프로젝트 폴더로 이동
cd ~/projects/my-project

# goose 세션 시작
goose session
```

### 8-2. Java 소스 분석

```
이 프로젝트의 Java 소스 코드 구조를 분석해줘.
src/main/java 디렉토리의 패키지 구조와 주요 클래스들을 파악하고,
클래스 다이어그램을 텍스트로 작성해줘.
```

### 8-3. JavaScript/HTML/JSP 분석

```
webapp 폴더의 JSP 파일들을 분석해서:
1. 각 페이지의 기능을 요약
2. 포함된 JavaScript 함수 목록
3. 사용된 공통 컴포넌트 파악
결과를 docs/page-analysis.md 파일로 작성해줘.
```

### 8-4. WebSquare 화면 분석

```
이 프로젝트의 WebSquare 화면 정의 파일(.xml)들을 분석해줘:
1. 화면 ID와 제목 목록
2. 사용된 컴포넌트 종류 통계
3. 이벤트 핸들러 함수 매핑
4. 서비스 호출 목록 (submission)
```

### 8-5. 문서 자동 생성

```
다음 작업을 수행해줘:
1. src 폴더의 모든 Java 클래스를 스캔
2. 각 클래스의 public 메서드 시그니처 추출
3. 메서드별 기능 설명을 추론하여 작성
4. 결과를 Markdown 형식의 API 문서로 생성
5. docs/api-reference.md 파일로 저장
```

---

## 9. 문제 해결

### Ollama 연결 오류

**증상**: `Error: Cannot connect to Ollama`

**해결 방법**:

```bash
# 1. Ollama 프로세스 확인
pgrep -l ollama

# 2. Ollama가 실행 중이 아니면 시작
ollama serve

# 3. 포트 사용 확인
lsof -i :11434

# 4. Ollama 앱 재시작
# 메뉴 바의 Ollama 아이콘 > Quit Ollama 후 다시 실행
```

### 모델 로드 실패

**증상**: `Error: Model not found`

**해결 방법**:

```bash
# 1. 사용 가능한 모델 목록 확인
ollama list

# 2. 모델 다시 다운로드
ollama pull qwen2.5-coder:7b
```

### 메모리 부족

**증상**: `Error: Out of memory` 또는 시스템이 매우 느려짐

**해결 방법**:

```bash
# 1. 현재 메모리 사용량 확인
vm_stat | head -10

# 2. 더 작은 모델로 변경
export GOOSE_MODEL="qwen3"

# 3. 컨텍스트 길이 줄이기
export OLLAMA_CONTEXT_LENGTH=8192

# 4. 영구 반영하려면 ~/.zshrc 수정
```

> **Apple Silicon 팁**: M1(16GB)에서는 7B 모델, M1 Pro/Max(32GB+)에서는 13B~32B 모델 사용을 권장합니다.

### Tool Calling 오류

**증상**: goose가 도구를 호출하지 못하거나 오류 발생

**해결 방법**:

```bash
# Tool Shim 활성화 (영구 적용)
echo 'export GOOSE_TOOLSHIM=1' >> ~/.zshrc
echo 'export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2' >> ~/.zshrc
source ~/.zshrc
```

### Gatekeeper / 보안 차단

**증상**: `"goose"은(는) Apple에서 악성 소프트웨어가 있는지 확인할 수 없습니다`

**해결 방법**:

```bash
# 방법 1: xattr로 격리 속성 제거
sudo xattr -d com.apple.quarantine /usr/local/bin/goose

# 방법 2: 시스템 설정에서 허용
# 시스템 설정 > 개인 정보 보호 및 보안 > "확인 없이 열기" 클릭
```

### 한글 인코딩 문제

**증상**: 한글이 깨져서 출력됨

**해결 방법**:

```bash
# ~/.zshrc에 추가
cat >> ~/.zshrc << 'EOF'
export LANG=ko_KR.UTF-8
export LC_ALL=ko_KR.UTF-8
EOF

source ~/.zshrc
```

### goose 명령어를 찾을 수 없음

**증상**: `zsh: command not found: goose`

**해결 방법**:

```bash
# 1. goose 바이너리 위치 확인
which goose

# 2. PATH에 /usr/local/bin이 포함되어 있는지 확인
echo $PATH | tr ':' '\n' | grep local

# 3. Homebrew로 설치한 경우 PATH 확인
eval "$(/opt/homebrew/bin/brew shellenv)"
```

---

## 부록: Apple Silicon 모델 선택 가이드

| 작업 유형 | 권장 모델 | 최소 통합 메모리 | 비고 |
|----------|----------|-----------------|------|
| 간단한 코드 리뷰 | qwen3 (8B) | 16GB (M1) | 범용 모델 |
| 복잡한 코드 분석 | qwen2.5-coder:7b | 16GB (M1) | 코드 특화, 가장 권장 |
| 대규모 리팩토링 | qwen2.5-coder:32b | 32GB (M1 Pro/Max) | 높은 정확도 |
| 문서 생성 | codellama:13b | 16GB (M1) | 코드 이해력 우수 |
| Tool Shim 보조 | llama3.2 | 추가 2GB | 도구 호출 해석용 |

> **성능 참고**: Apple Silicon은 Metal GPU 가속을 통해 Ollama 추론을 수행합니다. 동일 RAM 기준으로 Intel Mac 대비 추론 속도가 크게 향상됩니다.

---

## 부록: 전체 설치 요약 (Quick Start)

```bash
# 1. Ollama 설치 (둘 중 하나 선택)
brew install ollama                  # Homebrew
# 또는 https://ollama.com/download/mac 에서 다운로드

# 2. Ollama 실행 (Homebrew 설치 시)
ollama serve &

# 3. 모델 다운로드
ollama pull qwen2.5-coder:7b
ollama pull llama3.2

# 4. goose 설치 (셋 중 하나 선택)
curl -fsSL https://github.com/block/goose/releases/latest/download/download_cli.sh | sh  # 스크립트
brew install block/tap/goose         # Homebrew
# 또는 https://github.com/block/goose/releases 에서 aarch64 빌드 다운로드

# 5. 환경 변수 설정
cat >> ~/.zshrc << 'EOF'
export GOOSE_PROVIDER="ollama"
export GOOSE_MODEL="qwen2.5-coder:7b"
export OLLAMA_HOST="localhost:11434"
export OLLAMA_TIMEOUT=600
export GOOSE_TOOLSHIM=1
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
EOF
source ~/.zshrc

# 6. 확인
goose --version
ollama list
goose session
```
