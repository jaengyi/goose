# Ollama 모델 라이브러리 완벽 가이드

이 문서는 Ollama를 처음 접하는 초보자를 위한 종합 가이드입니다. Ollama의 기본 개념부터 모델 선택, 설치, 사용법까지 상세하게 설명합니다.

## 목차

1. [Ollama란 무엇인가?](#ollama란-무엇인가)
2. [설치 방법](#설치-방법)
3. [기본 명령어](#기본-명령어)
4. [모델 라이브러리 개요](#모델-라이브러리-개요)
5. [모델 상세 설명](#모델-상세-설명)
6. [모델 선택 가이드](#모델-선택-가이드)
7. [하드웨어 요구사항](#하드웨어-요구사항)
8. [실전 사용 예제](#실전-사용-예제)
9. [고급 설정](#고급-설정)
10. [자주 묻는 질문 (FAQ)](#자주-묻는-질문-faq)

---

## Ollama란 무엇인가?

### 기본 개념

**Ollama**는 대규모 언어 모델(LLM)을 로컬 컴퓨터에서 쉽게 실행할 수 있게 해주는 오픈소스 도구입니다.

#### 왜 Ollama를 사용하나요?

| 장점 | 설명 |
|------|------|
| **프라이버시** | 데이터가 외부 서버로 전송되지 않음 |
| **오프라인 사용** | 인터넷 없이도 AI 사용 가능 |
| **비용 절감** | API 비용 없이 무제한 사용 |
| **커스터마이징** | 나만의 모델 설정 가능 |
| **간편함** | 복잡한 설정 없이 한 줄 명령으로 실행 |

#### 핵심 용어 정리

```
┌─────────────────────────────────────────────────────────────┐
│  용어              │  설명                                   │
├─────────────────────────────────────────────────────────────┤
│  LLM               │  Large Language Model (대규모 언어 모델) │
│  파라미터 (B)      │  모델의 크기 단위 (7B = 70억 개 매개변수)│
│  양자화 (Quantization) │  모델 크기를 줄이는 압축 기술        │
│  GGUF              │  Ollama가 사용하는 모델 파일 형식       │
│  컨텍스트 길이     │  모델이 한 번에 처리할 수 있는 텍스트 양│
│  토큰              │  텍스트의 최소 처리 단위 (약 4글자=1토큰)│
└─────────────────────────────────────────────────────────────┘
```

#### Ollama 작동 방식

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   사용자      │────▶│   Ollama     │────▶│   LLM 모델   │
│  (명령/질문)  │     │   서버       │     │  (추론 수행) │
└──────────────┘     └──────────────┘     └──────────────┘
        ▲                                        │
        │                                        │
        └────────────────────────────────────────┘
                      응답 반환
```

---

## 설치 방법

### macOS

```bash
# 방법 1: 공식 설치 파일
# https://ollama.com/download 에서 dmg 파일 다운로드 후 설치

# 방법 2: Homebrew 사용
brew install ollama
```

### Windows

```bash
# 공식 설치 파일 다운로드
# https://ollama.com/download/OllamaSetup.exe
# 다운로드 후 실행하여 설치
```

### Linux

```bash
# 자동 설치 스크립트 (권장)
curl -fsSL https://ollama.com/install.sh | sh

# 설치 확인
ollama --version
```

### Docker

```bash
# Docker 이미지 실행
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama

# GPU 지원 (NVIDIA)
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### 설치 확인

```bash
# Ollama 버전 확인
ollama --version

# Ollama 서버 시작
ollama serve

# 다른 터미널에서 테스트
ollama run llama3.2 "안녕하세요"
```

---

## 기본 명령어

### 필수 명령어 요약

| 명령어 | 설명 | 예시 |
|--------|------|------|
| `ollama run` | 모델 실행 (없으면 자동 다운로드) | `ollama run llama3.2` |
| `ollama pull` | 모델 다운로드 | `ollama pull qwen2.5-coder:7b` |
| `ollama list` | 설치된 모델 목록 | `ollama list` |
| `ollama ps` | 실행 중인 모델 확인 | `ollama ps` |
| `ollama rm` | 모델 삭제 | `ollama rm llama3.2` |
| `ollama show` | 모델 정보 확인 | `ollama show llama3.2` |
| `ollama cp` | 모델 복사 | `ollama cp llama3.2 my-llama` |
| `ollama serve` | 서버 시작 | `ollama serve` |
| `ollama stop` | 모델 중지 | `ollama stop llama3.2` |

### 명령어 상세 설명

#### 1. 모델 실행 (run)

```bash
# 기본 실행 (대화 모드)
ollama run llama3.2

# 특정 버전/크기 실행
ollama run llama3.2:3b      # 3B 파라미터 버전
ollama run qwen2.5:7b       # 7B 파라미터 버전
ollama run qwen2.5:72b      # 72B 파라미터 버전

# 한 줄 질문 후 종료
ollama run llama3.2 "파이썬으로 Hello World 출력하는 코드 알려줘"

# 파일 내용 전달
ollama run llama3.2 "이 코드를 설명해줘: $(cat main.py)"
```

#### 2. 모델 다운로드 (pull)

```bash
# 기본 버전 다운로드
ollama pull llama3.2

# 특정 크기 다운로드
ollama pull qwen2.5-coder:7b
ollama pull deepseek-coder:6.7b

# 다운로드 진행률 확인 가능
# pulling manifest...
# pulling abc123... 100% ████████████████ 4.7 GB
```

#### 3. 모델 목록 확인 (list)

```bash
ollama list

# 출력 예시:
# NAME                    ID              SIZE      MODIFIED
# llama3.2:latest         a80c4f17acd5    2.0 GB    2 hours ago
# qwen2.5-coder:7b        2b0496514337    4.7 GB    1 day ago
# codellama:13b           9f438cb9cd58    7.4 GB    3 days ago
```

#### 4. 모델 정보 확인 (show)

```bash
ollama show llama3.2

# 출력 내용:
# - 모델 아키텍처
# - 파라미터 수
# - 컨텍스트 길이
# - 라이선스
# - 시스템 프롬프트
```

---

## 모델 라이브러리 개요

Ollama는 다양한 목적에 맞는 모델들을 제공합니다. 모델은 크게 다음과 같이 분류됩니다:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Ollama 모델 분류                              │
├─────────────────────────────────────────────────────────────────┤
│  🗣️ 범용 대화 모델      │  일반적인 질문, 글쓰기, 번역 등       │
│  💻 코드 특화 모델      │  프로그래밍, 코드 분석, 디버깅         │
│  👁️ 비전 모델          │  이미지 분석, 시각적 이해              │
│  🔢 임베딩 모델        │  텍스트 검색, 유사도 분석              │
│  🧠 추론 모델          │  복잡한 논리적 사고, 수학 문제         │
│  🪶 경량 모델          │  저사양 기기, 빠른 응답                │
└─────────────────────────────────────────────────────────────────┘
```

---

## 모델 상세 설명

### 1. 범용 대화 모델

일상적인 대화, 질문 응답, 글쓰기 등 다양한 작업에 사용됩니다.

#### Llama 3.2 (Meta)

```bash
ollama pull llama3.2        # 3B 버전 (기본)
ollama pull llama3.2:1b     # 1B 버전 (가벼움)
```

| 항목 | 내용 |
|------|------|
| 개발사 | Meta (Facebook) |
| 크기 | 1B, 3B |
| 특징 | 가볍고 빠름, 다국어 지원 |
| 용도 | 간단한 대화, 요약, 번역 |
| 다운로드 수 | 5,400만+ |

#### Llama 3.1 (Meta)

```bash
ollama pull llama3.1:8b     # 8B 버전
ollama pull llama3.1:70b    # 70B 버전 (고성능)
ollama pull llama3.1:405b   # 405B 버전 (최고 성능)
```

| 항목 | 내용 |
|------|------|
| 개발사 | Meta |
| 크기 | 8B, 70B, 405B |
| 특징 | 도구 호출 지원, 높은 품질 |
| 용도 | 복잡한 작업, 코드 생성, 분석 |
| 다운로드 수 | 1억+ |

#### Qwen 2.5 (Alibaba)

```bash
ollama pull qwen2.5:7b      # 7B 버전
ollama pull qwen2.5:14b     # 14B 버전
ollama pull qwen2.5:32b     # 32B 버전
ollama pull qwen2.5:72b     # 72B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Alibaba Cloud |
| 크기 | 0.5B ~ 72B |
| 특징 | 128K 컨텍스트, 다국어 우수 (한국어 포함) |
| 용도 | 긴 문서 분석, 다국어 작업 |
| 다운로드 수 | 1,970만+ |

#### Qwen 3 (Alibaba) - 최신

```bash
ollama pull qwen3           # 기본 버전
ollama pull qwen3:8b        # 8B 버전
ollama pull qwen3:32b       # 32B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Alibaba Cloud |
| 크기 | 0.6B ~ 235B |
| 특징 | 최신 세대, MoE 지원, 추론 능력 향상 |
| 용도 | 범용 작업, 복잡한 추론 |
| 다운로드 수 | 1,800만+ |

#### Gemma 3 (Google)

```bash
ollama pull gemma3:4b       # 4B 버전
ollama pull gemma3:12b      # 12B 버전
ollama pull gemma3:27b      # 27B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Google DeepMind |
| 크기 | 270M ~ 27B |
| 특징 | 단일 GPU에서 최고 성능, 비전 기능 포함 |
| 용도 | 범용, 이미지 분석 |
| 다운로드 수 | 3,070만+ |

---

### 2. 코드 특화 모델

프로그래밍 작업에 최적화된 모델들입니다.

#### Qwen 2.5 Coder (추천)

```bash
ollama pull qwen2.5-coder:7b    # 7B 버전 (권장)
ollama pull qwen2.5-coder:14b   # 14B 버전
ollama pull qwen2.5-coder:32b   # 32B 버전 (최고 성능)
```

| 항목 | 내용 |
|------|------|
| 개발사 | Alibaba Cloud |
| 크기 | 0.5B ~ 32B |
| 특징 | 코드 생성/분석 특화, 128K 컨텍스트 |
| 지원 언어 | Java, JavaScript, Python, TypeScript, Go, Rust 등 92개+ |
| 다운로드 수 | 1,040만+ |

**사용 예시:**
```bash
ollama run qwen2.5-coder:7b "Java로 파일 읽기 예제 코드를 작성해줘"
```

#### DeepSeek Coder

```bash
ollama pull deepseek-coder:6.7b    # 6.7B 버전
ollama pull deepseek-coder:33b     # 33B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | DeepSeek |
| 크기 | 1.3B ~ 33B |
| 특징 | 2조 토큰으로 훈련, Fill-in-Middle 지원 |
| 용도 | 코드 완성, 코드 분석 |
| 다운로드 수 | 280만+ |

#### CodeLlama (Meta)

```bash
ollama pull codellama:7b     # 7B 버전
ollama pull codellama:13b    # 13B 버전
ollama pull codellama:34b    # 34B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Meta |
| 크기 | 7B, 13B, 34B |
| 특징 | Llama 2 기반, 코드 특화 훈련 |
| 용도 | 코드 생성, 리팩토링 |
| 다운로드 수 | 700만+ |

#### StarCoder 2

```bash
ollama pull starcoder2:3b    # 3B 버전
ollama pull starcoder2:7b    # 7B 버전
ollama pull starcoder2:15b   # 15B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | BigCode |
| 크기 | 3B, 7B, 15B |
| 특징 | 투명한 학습 과정, 600개+ 언어 지원 |
| 용도 | 코드 생성, 자동 완성 |
| 다운로드 수 | 190만+ |

---

### 3. 추론 모델

복잡한 논리적 사고가 필요한 작업에 적합합니다.

#### DeepSeek-R1

```bash
ollama pull deepseek-r1:8b      # 8B 버전
ollama pull deepseek-r1:14b     # 14B 버전
ollama pull deepseek-r1:32b     # 32B 버전
ollama pull deepseek-r1:70b     # 70B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | DeepSeek |
| 크기 | 1.5B ~ 671B |
| 특징 | O3, Gemini 2.5 Pro 수준의 추론 능력 |
| 용도 | 수학, 논리 문제, 복잡한 분석 |
| 다운로드 수 | 7,700만+ |

#### Phi-4 (Microsoft)

```bash
ollama pull phi4:14b    # 14B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Microsoft |
| 크기 | 14B |
| 특징 | 작은 크기 대비 높은 성능 |
| 용도 | 추론, STEM 문제 |
| 다운로드 수 | 700만+ |

---

### 4. 비전 모델 (이미지 분석)

이미지를 이해하고 분석할 수 있는 모델들입니다.

#### LLaVA

```bash
ollama pull llava:7b     # 7B 버전
ollama pull llava:13b    # 13B 버전
ollama pull llava:34b    # 34B 버전
```

| 항목 | 내용 |
|------|------|
| 크기 | 7B, 13B, 34B |
| 특징 | 이미지 + 텍스트 동시 이해 |
| 용도 | 이미지 설명, 시각적 질문 응답 |
| 다운로드 수 | 1,270만+ |

#### Llama 3.2 Vision

```bash
ollama pull llama3.2-vision:11b    # 11B 버전
ollama pull llama3.2-vision:90b    # 90B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Meta |
| 크기 | 11B, 90B |
| 특징 | 고품질 이미지 추론 |
| 용도 | 이미지 분석, 차트 해석 |
| 다운로드 수 | 370만+ |

---

### 5. 임베딩 모델

텍스트를 벡터로 변환하여 검색이나 유사도 분석에 사용합니다.

#### Nomic Embed Text

```bash
ollama pull nomic-embed-text
```

| 항목 | 내용 |
|------|------|
| 크기 | 137M |
| 특징 | 8192 토큰 컨텍스트, 고성능 |
| 용도 | RAG, 문서 검색 |
| 다운로드 수 | 5,210만+ |

#### BGE-M3

```bash
ollama pull bge-m3
```

| 항목 | 내용 |
|------|------|
| 크기 | 567M |
| 특징 | 다국어 지원, 다기능성 |
| 용도 | 다국어 검색, 임베딩 |
| 다운로드 수 | 310만+ |

---

### 6. 경량 모델

저사양 기기에서도 실행 가능한 작은 모델들입니다.

#### TinyLlama

```bash
ollama pull tinyllama
```

| 항목 | 내용 |
|------|------|
| 크기 | 1.1B |
| 특징 | 3조 토큰으로 훈련, 매우 가벼움 |
| 용도 | 경량 기기, 빠른 응답 필요시 |
| 다운로드 수 | 340만+ |

#### SmolLM2

```bash
ollama pull smollm2:135m    # 135M 버전
ollama pull smollm2:360m    # 360M 버전
ollama pull smollm2:1.7b    # 1.7B 버전
```

| 항목 | 내용 |
|------|------|
| 개발사 | Hugging Face |
| 크기 | 135M ~ 1.7B |
| 특징 | 초소형, 모바일/엣지 디바이스용 |
| 용도 | 리소스 제한 환경 |
| 다운로드 수 | 240만+ |

---

## 모델 선택 가이드

### 작업별 추천 모델

```
┌────────────────────────────────────────────────────────────────────┐
│  작업 유형                │  추천 모델              │  최소 RAM    │
├────────────────────────────────────────────────────────────────────┤
│  일반 대화/질문           │  llama3.2:3b           │  8GB        │
│  한국어 작업              │  qwen2.5:7b            │  16GB       │
│  코드 작성/분석           │  qwen2.5-coder:7b      │  16GB       │
│  코드 리뷰/리팩토링       │  qwen2.5-coder:14b     │  24GB       │
│  대규모 코드베이스 분석   │  qwen2.5-coder:32b     │  32GB       │
│  복잡한 논리/수학         │  deepseek-r1:14b       │  24GB       │
│  이미지 분석              │  llava:7b              │  16GB       │
│  문서 검색/RAG            │  nomic-embed-text      │  8GB        │
│  저사양 기기              │  tinyllama             │  4GB        │
└────────────────────────────────────────────────────────────────────┘
```

### 언어별 코드 분석 추천

| 언어 | 1순위 | 2순위 |
|------|-------|-------|
| Java | qwen2.5-coder:7b | deepseek-coder:6.7b |
| JavaScript/TypeScript | qwen2.5-coder:7b | codellama:13b |
| Python | qwen2.5-coder:7b | deepseek-coder:6.7b |
| HTML/CSS/JSP | qwen2.5-coder:7b | llama3.1:8b |
| SQL | qwen2.5-coder:7b | deepseek-coder:6.7b |

### RAM 기준 모델 선택

```
┌─────────────────────────────────────────────────────┐
│  보유 RAM     │  실행 가능 모델                     │
├─────────────────────────────────────────────────────┤
│  8GB          │  3B 이하 모델                       │
│               │  - tinyllama, llama3.2:1b          │
│               │  - smollm2:1.7b                    │
├─────────────────────────────────────────────────────┤
│  16GB         │  7B~8B 모델                         │
│               │  - llama3.2:3b, qwen2.5:7b         │
│               │  - qwen2.5-coder:7b                │
│               │  - deepseek-coder:6.7b             │
├─────────────────────────────────────────────────────┤
│  32GB         │  13B~14B 모델                       │
│               │  - llama3.1:8b, qwen2.5:14b        │
│               │  - codellama:13b                   │
│               │  - qwen2.5-coder:14b               │
├─────────────────────────────────────────────────────┤
│  64GB+        │  32B~70B 모델                       │
│               │  - qwen2.5:32b, llama3.1:70b       │
│               │  - qwen2.5-coder:32b               │
│               │  - deepseek-r1:70b                 │
└─────────────────────────────────────────────────────┘
```

---

## 하드웨어 요구사항

### 최소 사양

| 구성 요소 | 최소 사양 | 권장 사양 |
|-----------|-----------|-----------|
| CPU | 4코어 이상 | 8코어 이상 |
| RAM | 8GB | 32GB |
| 저장공간 | 20GB SSD | 100GB SSD |
| GPU | 없어도 가능 | NVIDIA 8GB+ VRAM |

### GPU 지원

```
┌────────────────────────────────────────────────────────────────┐
│  GPU 종류              │  지원 여부    │  성능                  │
├────────────────────────────────────────────────────────────────┤
│  NVIDIA (CUDA)        │  ✅ 완벽 지원  │  최고 속도             │
│  Apple Silicon (M1~)  │  ✅ 완벽 지원  │  매우 빠름             │
│  AMD (ROCm)           │  ⚠️ 부분 지원  │  보통                  │
│  Intel (없음)         │  ❌ CPU만      │  느림                  │
└────────────────────────────────────────────────────────────────┘
```

### 모델 크기별 대략적인 디스크 사용량

| 모델 크기 | 4-bit 양자화 | 원본 (FP16) |
|-----------|-------------|-------------|
| 3B | ~2GB | ~6GB |
| 7B | ~4GB | ~14GB |
| 13B | ~7GB | ~26GB |
| 32B | ~18GB | ~64GB |
| 70B | ~40GB | ~140GB |

---

## 실전 사용 예제

### 예제 1: 대화형 사용

```bash
# 모델 실행
ollama run qwen2.5:7b

# 대화 시작 (>>> 프롬프트가 나타남)
>>> 안녕하세요, 자기소개 해주세요.

# 응답 받은 후 계속 대화
>>> 파이썬이란 무엇인가요?

# 종료
>>> /bye
```

### 예제 2: 코드 분석

```bash
# Java 파일 분석
ollama run qwen2.5-coder:7b "다음 Java 코드를 분석하고 개선점을 알려줘:

public class UserService {
    public User findUser(String id) {
        User user = userRepository.findById(id);
        if (user == null) {
            return null;
        }
        return user;
    }
}"
```

### 예제 3: 파일 내용 전달

```bash
# 파일 내용을 모델에 전달
ollama run qwen2.5-coder:7b "이 코드를 분석해줘: $(cat src/main/java/App.java)"

# 여러 파일 전달
ollama run qwen2.5-coder:7b "다음 파일들의 관계를 분석해줘:
=== UserController.java ===
$(cat UserController.java)

=== UserService.java ===
$(cat UserService.java)"
```

### 예제 4: 스크립트에서 사용

```bash
#!/bin/bash
# 코드 리뷰 자동화 스크립트

FILE=$1
REVIEW=$(ollama run qwen2.5-coder:7b "다음 코드를 리뷰하고 문제점을 알려줘: $(cat $FILE)")
echo "$REVIEW" > "${FILE}.review.md"
```

### 예제 5: REST API 사용

```bash
# 단일 응답 요청
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5-coder:7b",
  "prompt": "Hello World를 출력하는 Java 코드를 작성해줘",
  "stream": false
}'

# 대화형 API (chat)
curl http://localhost:11434/api/chat -d '{
  "model": "qwen2.5-coder:7b",
  "messages": [
    {"role": "user", "content": "Spring Boot란 무엇인가요?"}
  ],
  "stream": false
}'
```

### 예제 6: Python에서 사용

```python
import ollama

# 단순 생성
response = ollama.generate(
    model='qwen2.5-coder:7b',
    prompt='Python으로 파일 읽기 예제 코드를 작성해줘'
)
print(response['response'])

# 대화형 사용
response = ollama.chat(
    model='qwen2.5-coder:7b',
    messages=[
        {'role': 'user', 'content': 'Java의 Stream API를 설명해줘'}
    ]
)
print(response['message']['content'])
```

---

## 고급 설정

### Modelfile로 커스텀 모델 생성

Modelfile은 모델의 동작을 커스터마이징하는 설정 파일입니다.

```bash
# Modelfile 생성
cat > Modelfile << 'EOF'
FROM qwen2.5-coder:7b

# 파라미터 설정
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER num_ctx 8192

# 시스템 프롬프트 설정
SYSTEM """
당신은 Java와 Spring Boot 전문가입니다.
코드 리뷰를 할 때 다음 기준으로 검토합니다:
1. 코드 품질 및 가독성
2. 보안 취약점
3. 성능 최적화
4. 디자인 패턴 적용
항상 한국어로 답변합니다.
"""
EOF

# 커스텀 모델 생성
ollama create java-reviewer -f Modelfile

# 사용
ollama run java-reviewer "이 코드를 리뷰해줘: ..."
```

### 주요 파라미터 설명

| 파라미터 | 기본값 | 설명 |
|----------|--------|------|
| `temperature` | 0.8 | 창의성 조절 (0=보수적, 1=창의적) |
| `top_p` | 0.9 | 확률 분포 상위 p% 내에서 선택 |
| `top_k` | 40 | 상위 k개 토큰에서 선택 |
| `num_ctx` | 2048 | 컨텍스트 길이 (토큰 수) |
| `repeat_penalty` | 1.1 | 반복 방지 페널티 |
| `num_predict` | -1 | 최대 생성 토큰 수 (-1=무제한) |

### 환경 변수 설정

```bash
# ~/.bashrc 또는 ~/.zshrc에 추가

# Ollama 서버 주소 (기본: localhost:11434)
export OLLAMA_HOST=0.0.0.0:11434

# 모델 저장 경로 변경
export OLLAMA_MODELS=/custom/path/to/models

# 타임아웃 설정 (초)
export OLLAMA_KEEP_ALIVE=300

# 컨텍스트 길이 설정
export OLLAMA_CONTEXT_LENGTH=32768

# GPU 레이어 수 (0=CPU만 사용)
export OLLAMA_NUM_GPU=999
```

### 원격 접속 허용

```bash
# 모든 IP에서 접속 허용
OLLAMA_HOST=0.0.0.0:11434 ollama serve

# 다른 컴퓨터에서 접속
ollama run llama3.2 --host http://192.168.1.100:11434
```

---

## 자주 묻는 질문 (FAQ)

### Q1: 모델이 너무 느려요

**A:** 다음 방법을 시도해보세요:
1. 더 작은 모델 사용 (예: 7B → 3B)
2. 컨텍스트 길이 줄이기: `OLLAMA_CONTEXT_LENGTH=4096`
3. GPU 사용 확인: `nvidia-smi` 또는 `ollama ps`

### Q2: 메모리 부족 오류가 발생해요

**A:**
```bash
# 현재 로드된 모델 확인
ollama ps

# 불필요한 모델 언로드
ollama stop <model-name>

# 더 작은 모델 사용
ollama run qwen2.5:3b  # 7b 대신 3b
```

### Q3: 한국어 응답이 이상해요

**A:** 한국어에 강한 모델을 사용하세요:
- `qwen2.5:7b` (권장)
- `qwen2.5-coder:7b`
- `llama3.1:8b`

시스템 프롬프트에 "한국어로 답변해주세요" 추가:
```bash
ollama run qwen2.5:7b "한국어로 답변해줘. 파이썬이란?"
```

### Q4: 모델 다운로드가 중단됐어요

**A:**
```bash
# 다시 pull 하면 이어서 다운로드됨
ollama pull qwen2.5-coder:7b

# 완전히 새로 받으려면 먼저 삭제
ollama rm qwen2.5-coder:7b
ollama pull qwen2.5-coder:7b
```

### Q5: GPU를 사용하지 않아요

**A:**
```bash
# NVIDIA GPU 확인
nvidia-smi

# CUDA 드라이버 확인
nvcc --version

# Ollama에서 GPU 사용 강제
export OLLAMA_NUM_GPU=999
ollama serve
```

### Q6: 컨텍스트 길이를 늘리고 싶어요

**A:**
```bash
# 환경 변수로 설정
export OLLAMA_CONTEXT_LENGTH=32768
ollama serve

# 또는 Modelfile에서 설정
# PARAMETER num_ctx 32768
```

### Q7: 폐쇄망에서 모델을 어떻게 옮기나요?

**A:**
```bash
# 1. 인터넷 환경에서 모델 다운로드
ollama pull qwen2.5-coder:7b

# 2. 모델 폴더 복사 (USB, 외장하드 등)
# Linux/macOS: ~/.ollama/models/
# Windows: C:\Users\<username>\.ollama\models\

# 3. 폐쇄망에서 같은 경로에 붙여넣기

# 4. 모델 확인
ollama list
```

### Q8: 여러 모델을 동시에 실행할 수 있나요?

**A:** 네, 가능합니다 (충분한 메모리 필요):
```bash
# 터미널 1
ollama run llama3.2

# 터미널 2
ollama run qwen2.5-coder:7b

# 실행 중인 모델 확인
ollama ps
```

---

## 참고 자료

- [Ollama 공식 사이트](https://ollama.com/)
- [Ollama 모델 라이브러리](https://ollama.com/library)
- [Ollama GitHub](https://github.com/ollama/ollama)
- [Ollama 공식 문서](https://docs.ollama.com/)
- [Ollama Python 라이브러리](https://github.com/ollama/ollama-python)
- [Ollama JavaScript 라이브러리](https://github.com/ollama/ollama-js)

---

## 버전 정보

- 문서 작성일: 2026년 1월
- Ollama 버전: 0.15.x 기준
- 모델 정보는 변경될 수 있으니 공식 라이브러리를 확인하세요.
