# 폐쇄망 Windows 11 환경에서 goose 설치 매뉴얼

이 문서는 인터넷이 연결되지 않는 폐쇄망(Air-gapped) Windows 11 환경에서 goose를 설치하고 사용하는 방법을 단계별로 설명합니다.

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
| OS | Windows 11 | Windows 11 22H2 이상 |
| RAM | 16GB | 32GB 이상 |
| 저장 공간 | 20GB 여유 | 50GB 이상 여유 |
| GPU | 없어도 가능 | NVIDIA GPU (CUDA 지원) |

### 준비물

- USB 드라이브 또는 외장 하드 (최소 32GB)
- 인터넷이 연결된 별도 PC (다운로드용)

---

## 2. 인터넷 환경에서 파일 다운로드

인터넷이 연결된 PC에서 아래 파일들을 미리 다운로드합니다.

### 2-1. goose 바이너리 다운로드

GitHub Releases 페이지에서 Windows용 바이너리를 다운로드합니다:

```
https://github.com/block/goose/releases
```

- `goose-x86_64-pc-windows-msvc.zip` (또는 최신 Windows 빌드) 파일을 다운로드합니다.

### 2-2. Ollama 설치 파일 다운로드

```
https://ollama.com/download/windows
```

- `OllamaSetup.exe` 파일을 다운로드합니다.

### 2-3. LLM 모델 다운로드

인터넷 연결 PC에 Ollama를 먼저 설치한 뒤, 필요한 모델을 다운로드합니다.

PowerShell 또는 명령 프롬프트에서 실행:

```powershell
# 코드 분석에 적합한 모델 (용도에 맞게 선택)
ollama pull qwen3                    # 기본 모델 (약 4.7GB)
ollama pull qwen2.5-coder:7b        # 코드 특화 모델 (약 4.7GB) - 권장
ollama pull codellama:13b            # 코드 특화 모델 (약 7.4GB)
ollama pull deepseek-coder:6.7b     # 코드 분석 특화 (약 3.8GB)

# Tool Shim용 모델 (도구 호출 지원이 약한 모델 사용 시 필요)
ollama pull llama3.2

# 대형 모델 (32GB+ RAM 환경에서만)
ollama pull qwen2.5-coder:32b       # 고성능 코드 모델 (약 19GB)
```

### 2-4. 모델 파일 위치 확인

다운로드된 모델은 다음 경로에 저장됩니다:

```
C:\Users\<사용자명>\.ollama\models\
```

이 폴더 전체를 USB 드라이브에 복사합니다.

### 2-5. 다운로드 파일 체크리스트

USB 드라이브에 다음 파일들이 모두 있는지 확인합니다:

```
USB 드라이브/
├── OllamaSetup.exe              # Ollama 설치 파일
├── goose-windows.zip            # goose 바이너리 (파일명은 릴리스마다 다를 수 있음)
└── .ollama/
    └── models/                  # 다운로드한 모델 파일 전체
        ├── blobs/
        └── manifests/
```

---

## 3. 폐쇄망 환경으로 파일 이동

USB 드라이브를 폐쇄망 Windows 11 PC에 연결하고, 파일을 복사합니다.

### 3-1. 작업 폴더 생성

PowerShell을 **관리자 권한**으로 실행합니다:

```powershell
# goose 설치용 폴더 생성
mkdir C:\goose
```

### 3-2. 파일 복사

```powershell
# goose 바이너리 복사
Copy-Item "E:\goose-windows.zip" -Destination "C:\goose\"

# Ollama 설치 파일 복사
Copy-Item "E:\OllamaSetup.exe" -Destination "C:\goose\"

# 모델 파일 복사 (시간이 걸릴 수 있음)
Copy-Item -Recurse "E:\.ollama\models" -Destination "$env:USERPROFILE\.ollama\models"
```

> **참고**: `E:\`는 USB 드라이브 문자입니다. 실제 드라이브 문자에 맞게 변경하세요.

---

## 4. Ollama 설치 및 모델 설정

### 4-1. Ollama 설치

1. `C:\goose\OllamaSetup.exe`를 더블 클릭하여 설치합니다.
2. 설치 마법사의 안내에 따라 진행합니다.
3. 설치 완료 후 시스템 트레이에 Ollama 아이콘이 나타나는지 확인합니다.

### 4-2. 모델 파일 확인

PowerShell에서 모델이 정상적으로 인식되는지 확인합니다:

```powershell
ollama list
```

정상 출력 예시:

```
NAME                    ID              SIZE      MODIFIED
qwen2.5-coder:7b       a]b2c3d4e5f6    4.7 GB    2 hours ago
llama3.2                f6e5d4c3b2a1    2.0 GB    2 hours ago
```

### 4-3. Ollama 서비스 확인

Ollama는 설치 후 자동으로 백그라운드에서 실행됩니다. 서비스 상태를 확인합니다:

```powershell
# API 응답 확인
curl http://localhost:11434/api/tags
```

> Ollama가 실행되지 않는 경우, 시작 메뉴에서 "Ollama"를 검색하여 실행하거나 PowerShell에서 `ollama serve`를 실행합니다.

---

## 5. goose 설치 및 설정

### 5-1. goose 바이너리 설치

```powershell
# 압축 해제
Expand-Archive -Path "C:\goose\goose-windows.zip" -DestinationPath "C:\goose\bin"

# PATH에 추가 (현재 세션)
$env:PATH += ";C:\goose\bin"

# PATH에 영구 추가
[Environment]::SetEnvironmentVariable(
    "PATH",
    [Environment]::GetEnvironmentVariable("PATH", "User") + ";C:\goose\bin",
    "User"
)
```

### 5-2. 설치 확인

```powershell
goose --version
```

### 5-3. 환경 변수 설정

PowerShell에서 시스템 환경 변수를 설정합니다:

```powershell
# 영구 환경 변수 설정 (사용자 수준)
[Environment]::SetEnvironmentVariable("GOOSE_PROVIDER", "ollama", "User")
[Environment]::SetEnvironmentVariable("GOOSE_MODEL", "qwen2.5-coder:7b", "User")
[Environment]::SetEnvironmentVariable("OLLAMA_HOST", "localhost:11434", "User")
[Environment]::SetEnvironmentVariable("OLLAMA_TIMEOUT", "600", "User")
[Environment]::SetEnvironmentVariable("GOOSE_DISABLE_KEYRING", "1", "User")
```

> **중요**: 환경 변수 설정 후 PowerShell을 다시 시작해야 적용됩니다.

### 5-4. goose 설정 파일 생성

설정 파일 디렉토리를 생성하고 설정 파일을 작성합니다:

```powershell
# 설정 디렉토리 생성
mkdir "$env:APPDATA\goose" -Force
```

`%APPDATA%\goose\config.yaml` 파일을 메모장 또는 텍스트 에디터로 생성합니다:

```yaml
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
```

### 5-5. 대화형 설정 (선택)

명령 프롬프트 또는 PowerShell에서 대화형으로 설정할 수도 있습니다:

```powershell
goose configure
```

메뉴에서 다음을 선택합니다:

1. **Configure Providers** > **Ollama** 선택
2. **OLLAMA_HOST** 입력: `localhost:11434`
3. 사용할 모델 선택 (예: `qwen2.5-coder:7b`)

---

## 6. Tool Shim 설정

일부 로컬 모델은 도구 호출(tool calling) 기능이 약할 수 있습니다. 이 경우 Tool Shim을 활성화하여 별도의 경량 모델이 도구 호출을 해석하도록 합니다.

```powershell
# Tool Shim 환경 변수 설정
[Environment]::SetEnvironmentVariable("GOOSE_TOOLSHIM", "1", "User")
[Environment]::SetEnvironmentVariable("GOOSE_TOOLSHIM_OLLAMA_MODEL", "llama3.2", "User")
```

> **참고**: Tool Shim을 사용하려면 2단계에서 `llama3.2` 모델도 함께 다운로드해야 합니다.

---

## 7. 동작 확인

### 7-1. Ollama 동작 확인

```powershell
# Ollama 서비스 상태 확인
ollama list

# 모델 테스트
ollama run qwen2.5-coder:7b "Hello, world!"
```

### 7-2. goose 동작 확인

```powershell
# goose 세션 시작
goose session

# 세션 내에서 간단한 테스트
# > 안녕하세요. 현재 디렉토리의 파일 목록을 보여주세요.
```

정상적으로 응답이 오면 설치가 완료된 것입니다. `Ctrl+C`로 세션을 종료합니다.

---

## 8. 소스 코드 분석 활용 예시

### 8-1. 프로젝트 분석 시작

```powershell
# 분석할 프로젝트 폴더로 이동
cd C:\projects\my-project

# goose 세션 시작
goose session
```

### 8-2. Java 소스 분석

```
이 프로젝트의 Java 소스 코드 구조를 분석해줘.
src\main\java 디렉토리의 패키지 구조와 주요 클래스들을 파악하고,
클래스 다이어그램을 텍스트로 작성해줘.
```

### 8-3. JavaScript/HTML/JSP 분석

```
webapp 폴더의 JSP 파일들을 분석해서:
1. 각 페이지의 기능을 요약
2. 포함된 JavaScript 함수 목록
3. 사용된 공통 컴포넌트 파악
결과를 docs\page-analysis.md 파일로 작성해줘.
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
5. docs\api-reference.md 파일로 저장
```

---

## 9. 문제 해결

### Ollama 연결 오류

**증상**: `Error: Cannot connect to Ollama`

**해결 방법**:

```powershell
# 1. Ollama 프로세스 확인
Get-Process ollama -ErrorAction SilentlyContinue

# 2. Ollama가 실행 중이 아니면 시작
ollama serve

# 3. 포트 사용 확인
netstat -ano | findstr "11434"

# 4. 방화벽에서 localhost 통신 차단 여부 확인
# Windows Defender 방화벽 > 고급 설정에서 확인
```

### 모델 로드 실패

**증상**: `Error: Model not found`

**해결 방법**:

```powershell
# 1. 사용 가능한 모델 목록 확인
ollama list

# 2. 모델 파일 경로 확인
dir "$env:USERPROFILE\.ollama\models"

# 3. 모델 파일이 올바르게 복사되었는지 확인
dir "$env:USERPROFILE\.ollama\models\manifests"
dir "$env:USERPROFILE\.ollama\models\blobs"
```

### 메모리 부족

**증상**: `Error: Out of memory` 또는 시스템이 매우 느려짐

**해결 방법**:

```powershell
# 1. 더 작은 모델로 변경
[Environment]::SetEnvironmentVariable("GOOSE_MODEL", "qwen3", "User")

# 2. 컨텍스트 길이 줄이기
[Environment]::SetEnvironmentVariable("OLLAMA_CONTEXT_LENGTH", "8192", "User")

# 3. 현재 메모리 사용량 확인
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 10 Name, @{N='Memory(MB)';E={[math]::Round($_.WorkingSet64/1MB,1)}}
```

### Tool Calling 오류

**증상**: goose가 도구를 호출하지 못하거나 오류 발생

**해결 방법**:

```powershell
# Tool Shim 활성화
[Environment]::SetEnvironmentVariable("GOOSE_TOOLSHIM", "1", "User")
[Environment]::SetEnvironmentVariable("GOOSE_TOOLSHIM_OLLAMA_MODEL", "llama3.2", "User")

# PowerShell 재시작 후 다시 시도
```

### 한글 인코딩 문제

**증상**: 한글이 깨져서 출력됨

**해결 방법**:

```powershell
# PowerShell 인코딩 설정
[Console]::OutputEncoding = [System.Text.Encoding]::UTF8
[Console]::InputEncoding = [System.Text.Encoding]::UTF8
$OutputEncoding = [System.Text.Encoding]::UTF8

# 위 설정을 영구 적용하려면 PowerShell 프로필에 추가
notepad $PROFILE
# 열린 파일에 위 세 줄을 붙여넣고 저장

# 명령 프롬프트(cmd) 사용 시
chcp 65001
```

### goose 명령어를 찾을 수 없음

**증상**: `goose : 이 시스템에서 스크립트를 실행할 수 없습니다` 또는 `goose is not recognized`

**해결 방법**:

```powershell
# 1. PATH 확인
$env:PATH -split ";" | Where-Object { $_ -like "*goose*" }

# 2. PATH에 goose 경로가 없으면 추가
[Environment]::SetEnvironmentVariable(
    "PATH",
    [Environment]::GetEnvironmentVariable("PATH", "User") + ";C:\goose\bin",
    "User"
)

# 3. PowerShell 재시작 후 확인
goose --version
```

---

## 부록: 모델 선택 가이드

| 작업 유형 | 권장 모델 | 최소 RAM | 비고 |
|----------|----------|----------|------|
| 간단한 코드 리뷰 | qwen3 (8B) | 16GB | 범용 모델 |
| 복잡한 코드 분석 | qwen2.5-coder:7b | 16GB | 코드 특화, 가장 권장 |
| 대규모 리팩토링 | qwen2.5-coder:32b | 32GB | 높은 정확도 |
| 문서 생성 | codellama:13b | 16GB | 코드 이해력 우수 |
| Tool Shim 보조 | llama3.2 | 추가 2GB | 도구 호출 해석용 |

---

## 부록: 전체 설치 요약 (Quick Start)

아래는 전체 설치 과정을 요약한 것입니다:

```powershell
# === 인터넷 PC에서 수행 ===
# 1. OllamaSetup.exe 다운로드 (https://ollama.com/download/windows)
# 2. goose 바이너리 다운로드 (https://github.com/block/goose/releases)
# 3. Ollama 설치 후 모델 다운로드
ollama pull qwen2.5-coder:7b
ollama pull llama3.2
# 4. 모델 폴더 복사: C:\Users\<사용자명>\.ollama\models\ -> USB

# === 폐쇄망 PC에서 수행 ===
# 1. Ollama 설치
# 2. 모델 파일 복사
Copy-Item -Recurse "E:\.ollama\models" -Destination "$env:USERPROFILE\.ollama\models"
# 3. goose 설치
mkdir C:\goose\bin
Expand-Archive -Path "C:\goose\goose-windows.zip" -DestinationPath "C:\goose\bin"
[Environment]::SetEnvironmentVariable("PATH", [Environment]::GetEnvironmentVariable("PATH","User") + ";C:\goose\bin", "User")
# 4. 환경 변수 설정
[Environment]::SetEnvironmentVariable("GOOSE_PROVIDER", "ollama", "User")
[Environment]::SetEnvironmentVariable("GOOSE_MODEL", "qwen2.5-coder:7b", "User")
[Environment]::SetEnvironmentVariable("OLLAMA_HOST", "localhost:11434", "User")
[Environment]::SetEnvironmentVariable("GOOSE_DISABLE_KEYRING", "1", "User")
[Environment]::SetEnvironmentVariable("GOOSE_TOOLSHIM", "1", "User")
[Environment]::SetEnvironmentVariable("GOOSE_TOOLSHIM_OLLAMA_MODEL", "llama3.2", "User")
# 5. PowerShell 재시작 후 확인
goose --version
ollama list
goose session
```
