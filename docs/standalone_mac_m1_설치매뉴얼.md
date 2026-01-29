# 폐쇄망 macOS (Apple Silicon M1/M2/M3/M4) 환경에서 goose 설치 매뉴얼

이 문서는 인터넷이 연결되지 않는 폐쇄망(Air-gapped) macOS Apple Silicon 환경에서 goose를 설치하고 사용하는 방법을 단계별로 설명합니다.

## 목차

1. [사전 준비 사항](#1-사전-준비-사항)
2. [인터넷 환경에서 파일 다운로드](#2-인터넷-환경에서-파일-다운로드)
3. [폐쇄망 환경으로 파일 이동](#3-폐쇄망-환경으로-파일-이동)
4. [Ollama 설치 및 모델 설정](#4-ollama-설치-및-모델-설정)
5. [goose 설치 및 설정](#5-goose-설치-및-설정)
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

### 준비물

- USB 드라이브 또는 외장 하드 (최소 32GB)
- 인터넷이 연결된 별도 Mac 또는 PC (다운로드용)

---

## 2. 인터넷 환경에서 파일 다운로드

인터넷이 연결된 Mac에서 아래 파일들을 미리 다운로드합니다.

### 2-1. goose 바이너리 다운로드

GitHub Releases 페이지에서 macOS Apple Silicon용 바이너리를 다운로드합니다:

```
https://github.com/block/goose/releases
```

- `goose-aarch64-apple-darwin.zip` (Apple Silicon / ARM64) 파일을 다운로드합니다.

> **주의**: Intel Mac용인 `goose-x86_64-apple-darwin.zip`이 아닌, **aarch64** (ARM64) 빌드를 선택해야 합니다.

### 2-2. Ollama 설치 파일 다운로드

```
https://ollama.com/download/mac
```

- `Ollama-darwin.zip` 파일을 다운로드합니다. Apple Silicon을 자동으로 지원합니다.

### 2-3. LLM 모델 다운로드

인터넷 연결 Mac에 Ollama를 먼저 설치한 뒤, 터미널에서 필요한 모델을 다운로드합니다:

```bash
# 코드 분석에 적합한 모델 (용도에 맞게 선택)
ollama pull qwen3                    # 기본 모델 (약 4.7GB)
ollama pull qwen2.5-coder:7b        # 코드 특화 모델 (약 4.7GB) - 권장
ollama pull codellama:13b            # 코드 특화 모델 (약 7.4GB)
ollama pull deepseek-coder:6.7b     # 코드 분석 특화 (약 3.8GB)

# Tool Shim용 모델 (도구 호출 지원이 약한 모델 사용 시 필요)
ollama pull llama3.2

# 대형 모델 (32GB+ 통합 메모리 환경에서만)
ollama pull qwen2.5-coder:32b       # 고성능 코드 모델 (약 19GB)
```

### 2-4. 모델 파일 위치 확인

다운로드된 모델은 다음 경로에 저장됩니다:

```
~/.ollama/models/
```

이 폴더 전체를 USB 드라이브에 복사합니다:

```bash
# USB 드라이브에 모델 복사 (/Volumes/USB는 실제 USB 마운트 경로로 변경)
cp -r ~/.ollama/models /Volumes/USB/.ollama_models
```

### 2-5. 다운로드 파일 체크리스트

USB 드라이브에 다음 파일들이 모두 있는지 확인합니다:

```
USB 드라이브/
├── Ollama-darwin.zip            # Ollama 설치 파일
├── goose-aarch64-apple-darwin.zip  # goose 바이너리 (ARM64)
└── .ollama_models/              # 다운로드한 모델 파일 전체
    ├── blobs/
    └── manifests/
```

---

## 3. 폐쇄망 환경으로 파일 이동

USB 드라이브를 폐쇄망 Mac에 연결하고, 파일을 복사합니다.

### 3-1. USB 드라이브 마운트 확인

```bash
# USB 드라이브 마운트 경로 확인
ls /Volumes/
```

### 3-2. 작업 폴더 생성

```bash
mkdir -p ~/goose-install
```

### 3-3. 파일 복사

```bash
# USB 드라이브 경로 (실제 이름으로 변경)
USB="/Volumes/USB"

# goose 바이너리 복사
cp "$USB/goose-aarch64-apple-darwin.zip" ~/goose-install/

# Ollama 설치 파일 복사
cp "$USB/Ollama-darwin.zip" ~/goose-install/

# 모델 파일 복사 (용량이 크므로 시간이 걸릴 수 있음)
mkdir -p ~/.ollama
cp -r "$USB/.ollama_models" ~/.ollama/models
```

---

## 4. Ollama 설치 및 모델 설정

### 4-1. Ollama 설치

```bash
# 압축 해제
cd ~/goose-install
unzip Ollama-darwin.zip

# Applications 폴더로 이동
mv Ollama.app /Applications/
```

또는 Finder에서 `Ollama-darwin.zip`을 더블 클릭하여 압축을 풀고, `Ollama.app`을 `/Applications` 폴더로 드래그합니다.

### 4-2. Ollama 최초 실행

1. Launchpad 또는 Spotlight(Cmd+Space)에서 **Ollama**를 검색하여 실행합니다.
2. 최초 실행 시 "개발자를 확인할 수 없습니다" 경고가 나올 수 있습니다:
   - **시스템 설정** > **개인 정보 보호 및 보안** > 하단의 "확인 없이 열기" 클릭
   - 또는 터미널에서 실행:
     ```bash
     xattr -d com.apple.quarantine /Applications/Ollama.app
     ```
3. 메뉴 바에 Ollama 아이콘(🦙)이 나타나면 정상 실행된 것입니다.

### 4-3. 모델 파일 확인

터미널에서 모델이 정상적으로 인식되는지 확인합니다:

```bash
ollama list
```

정상 출력 예시:

```
NAME                    ID              SIZE      MODIFIED
qwen2.5-coder:7b       a1b2c3d4e5f6    4.7 GB    2 hours ago
llama3.2                f6e5d4c3b2a1    2.0 GB    2 hours ago
```

### 4-4. Ollama 서비스 확인

```bash
# API 응답 확인
curl http://localhost:11434/api/tags
```

> Ollama가 실행되지 않는 경우, 터미널에서 `ollama serve`를 실행하거나 Launchpad에서 Ollama 앱을 다시 실행합니다.

---

## 5. goose 설치 및 설정

### 5-1. goose 바이너리 설치

```bash
# 압축 해제
cd ~/goose-install
unzip goose-aarch64-apple-darwin.zip

# 실행 권한 부여
chmod +x goose

# 시스템 경로로 이동
sudo mv goose /usr/local/bin/
```

### 5-2. macOS Gatekeeper 허용

goose 바이너리가 서명되지 않은 경우 실행이 차단될 수 있습니다:

```bash
# Gatekeeper 격리 속성 제거
sudo xattr -d com.apple.quarantine /usr/local/bin/goose
```

### 5-3. 설치 확인

```bash
goose --version
```

### 5-4. 환경 변수 설정

macOS의 기본 셸은 zsh입니다. `~/.zshrc` 파일에 환경 변수를 추가합니다:

```bash
# ~/.zshrc에 추가 (nano, vim 등 사용 가능한 에디터로 편집)
cat >> ~/.zshrc << 'EOF'

# === goose 설정 ===
export GOOSE_PROVIDER="ollama"
export GOOSE_MODEL="qwen2.5-coder:7b"
export OLLAMA_HOST="localhost:11434"
export OLLAMA_TIMEOUT=600
export GOOSE_DISABLE_KEYRING=1
EOF

# 설정 즉시 적용
source ~/.zshrc
```

### 5-5. goose 설정 파일 생성

```bash
# 설정 디렉토리 생성
mkdir -p ~/.config/goose
```

`~/.config/goose/config.yaml` 파일을 생성합니다:

```bash
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

### 5-6. 대화형 설정 (선택)

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
# ~/.zshrc에 추가
cat >> ~/.zshrc << 'EOF'

# === Tool Shim 설정 ===
export GOOSE_TOOLSHIM=1
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
EOF

# 설정 즉시 적용
source ~/.zshrc
```

> **참고**: Tool Shim을 사용하려면 2단계에서 `llama3.2` 모델도 함께 다운로드해야 합니다.

---

## 7. 동작 확인

### 7-1. Ollama 동작 확인

```bash
# Ollama 서비스 상태 확인
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

# 2. 모델 파일 경로 확인
ls -la ~/.ollama/models/

# 3. 모델 디렉토리 구조 확인
ls -la ~/.ollama/models/manifests/
ls -la ~/.ollama/models/blobs/

# 4. 모델 파일 권한 확인 (USB에서 복사 시 권한 문제 가능)
chmod -R 755 ~/.ollama/models/
```

### 메모리 부족

**증상**: `Error: Out of memory` 또는 시스템이 매우 느려짐

**해결 방법**:

```bash
# 1. 현재 메모리 사용량 확인 (활성 메모리 확인)
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
ls -la /usr/local/bin/goose

# 2. 바이너리가 없으면 다시 복사
sudo cp ~/goose-install/goose /usr/local/bin/
sudo chmod +x /usr/local/bin/goose

# 3. PATH에 /usr/local/bin이 포함되어 있는지 확인
echo $PATH | tr ':' '\n' | grep local
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

아래는 전체 설치 과정을 요약한 것입니다:

```bash
# === 인터넷 Mac에서 수행 ===
# 1. Ollama-darwin.zip 다운로드 (https://ollama.com/download/mac)
# 2. goose 바이너리 다운로드 (https://github.com/block/goose/releases)
#    -> goose-aarch64-apple-darwin.zip 선택
# 3. Ollama 설치 후 모델 다운로드
ollama pull qwen2.5-coder:7b
ollama pull llama3.2
# 4. 모델 폴더를 USB로 복사
cp -r ~/.ollama/models /Volumes/USB/.ollama_models

# === 폐쇄망 Mac에서 수행 ===
USB="/Volumes/USB"   # 실제 USB 경로로 변경

# 1. Ollama 설치
unzip "$USB/Ollama-darwin.zip" -d /Applications/
xattr -d com.apple.quarantine /Applications/Ollama.app
open /Applications/Ollama.app

# 2. 모델 파일 복사
mkdir -p ~/.ollama
cp -r "$USB/.ollama_models" ~/.ollama/models

# 3. goose 설치
unzip "$USB/goose-aarch64-apple-darwin.zip" -d ~/goose-install/
sudo mv ~/goose-install/goose /usr/local/bin/
sudo chmod +x /usr/local/bin/goose
sudo xattr -d com.apple.quarantine /usr/local/bin/goose

# 4. 환경 변수 설정
cat >> ~/.zshrc << 'EOF'
export GOOSE_PROVIDER="ollama"
export GOOSE_MODEL="qwen2.5-coder:7b"
export OLLAMA_HOST="localhost:11434"
export OLLAMA_TIMEOUT=600
export GOOSE_DISABLE_KEYRING=1
export GOOSE_TOOLSHIM=1
export GOOSE_TOOLSHIM_OLLAMA_MODEL=llama3.2
EOF
source ~/.zshrc

# 5. 확인
goose --version
ollama list
goose session
```
