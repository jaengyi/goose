---
title: CI/CD 환경
description: CI/CD 파이프라인에서 goose를 설정하여 작업 자동화하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

goose는 로컬 머신에서만 유용한 것이 아니라 CI/CD 환경에서도 작업을 간소화할 수 있습니다. goose를 파이프라인에 통합하면 다음과 같은 작업을 자동화할 수 있습니다:

- 코드 리뷰
- 문서 검사
- 빌드 및 배포 워크플로우
- 인프라 및 환경 관리
- 롤백 및 복구 프로세스
- 지능형 테스트 실행

이 가이드에서는 CI/CD 파이프라인에서 goose를 설정하는 방법을 안내하며, 코드 리뷰를 위한 GitHub Actions 사용에 중점을 둡니다.


## GitHub Actions와 함께 goose 사용하기
GitHub Actions 내에서 goose를 직접 실행할 수 있습니다. 워크플로우를 설정하려면 다음 단계를 따르세요.

:::info 요약
<details>
   <summary>GitHub Workflow 복사</summary>

   ```yaml title="goose.yml"


name: goose

on:
   pull_request:
      types: [opened, synchronize, reopened, labeled]

permissions:
   contents: write
   pull-requests: write
   issues: write

env:
   PROVIDER_API_KEY: ${{ secrets.REPLACE_WITH_PROVIDER_API_KEY }}
   PR_NUMBER: ${{ github.event.pull_request.number }}
   GH_TOKEN: ${{ github.token }}

jobs:
   goose-comment:
      name: goose Comment
      runs-on: ubuntu-latest
      steps:
         - name: Check out repository
           uses: actions/checkout@v4
           with:
              fetch-depth: 0

         - name: Gather PR information
           run: |
              {
              echo "# Files Changed"
              gh pr view $PR_NUMBER --json files \
                 -q '.files[] | "* " + .path + " (" + (.additions|tostring) + " additions, " + (.deletions|tostring) + " deletions)"'
              echo ""
              echo "# Changes Summary"
              gh pr diff $PR_NUMBER
              } > changes.txt

         - name: Install goose CLI
           run: |
              mkdir -p /home/runner/.local/bin
              curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh \
              | CONFIGURE=false GOOSE_BIN_DIR=/home/runner/.local/bin bash
              echo "/home/runner/.local/bin" >> $GITHUB_PATH

         - name: Configure goose
           run: |
              mkdir -p ~/.config/goose
              cat <<EOF > ~/.config/goose/config.yaml
              GOOSE_PROVIDER: REPLACE_WITH_PROVIDER
              GOOSE_MODEL: REPLACE_WITH_MODEL
              keyring: false
              EOF

         - name: Create instructions for goose
           run: |
              cat <<EOF > instructions.txt
              Create a summary of the changes provided. Don't provide any session or logging details.
              The summary for each file should be brief and structured as:
              <filename/path (wrapped in backticks)>
                 - dot points of changes
              You don't need any extensions, don't mention extensions at all.
              The changes to summarise are:
              $(cat changes.txt)
              EOF

         - name: Test
           run: cat instructions.txt

         - name: Run goose and filter output
           run: |
              goose run --instructions instructions.txt | \
              # Remove ANSI color codes
              sed -E 's/\x1B\[[0-9;]*[mK]//g' | \
              # Remove session/logging lines
              grep -v "logging to /home/runner/.config/goose/sessions/" | \
              grep -v "^starting session" | \
              grep -v "^Closing session" | \
              # Trim trailing whitespace
              sed 's/[[:space:]]*$//' \
              > pr_comment.txt

         - name: Post comment to PR
           run: |
              cat -A pr_comment.txt
              gh pr comment $PR_NUMBER --body-file pr_comment.txt

   ```
</details>

:::

### 1. 워크플로우 파일 생성

저장소에 `.github/workflows/goose.yml` 경로로 새 파일을 생성합니다. 여기에 GitHub Actions 워크플로우가 포함됩니다.

### 2. 워크플로우 트리거 및 권한 정의

다음과 같이 액션을 구성합니다:

- PR이 열리거나, 업데이트되거나, 다시 열리거나, 레이블이 지정될 때 워크플로우 트리거
- goose가 저장소와 상호 작용하는 데 필요한 권한 부여
- 선택한 LLM 프로바이더에 대한 환경 변수 구성

```yaml
name: goose

on:
    pull_request:
        types: [opened, synchronize, reopened, labeled]

permissions:
    contents: write
    pull-requests: write
    issues: write

env:
   PROVIDER_API_KEY: ${{ secrets.REPLACE_WITH_PROVIDER_API_KEY }}
   PR_NUMBER: ${{ github.event.pull_request.number }}
```


### 3. goose 설치 및 구성

워크플로우에 goose를 설치하고 설정하려면 다음 단계를 추가합니다:

```yaml
steps:
    - name: Install goose CLI
      run: |
          mkdir -p /home/runner/.local/bin
          curl -fsSL https://github.com/block/goose/releases/download/stable/download_cli.sh \
            | CONFIGURE=false GOOSE_BIN_DIR=/home/runner/.local/bin bash
          echo "/home/runner/.local/bin" >> $GITHUB_PATH

    - name: Configure goose
      run: |
          mkdir -p ~/.config/goose
          cat <<EOF > ~/.config/goose/config.yaml
          GOOSE_PROVIDER: REPLACE_WITH_PROVIDER
          GOOSE_MODEL: REPLACE_WITH_MODEL
          keyring: false
          EOF
```

:::info 교체 필요
`REPLACE_WITH_PROVIDER`와 `REPLACE_WITH_MODEL`을 LLM 프로바이더 및 모델 이름으로 교체하고 필요한 다른 구성을 추가하세요.
:::

### 4. PR 변경 사항 수집 및 지침 준비

이 단계에서는 PR 세부 정보를 추출하고 goose를 위한 구조화된 지침으로 형식화합니다.

```yaml
    - name: Create instructions for goose
      run: |
          cat <<EOF > instructions.txt
          Create a summary of the changes provided. Don't provide any session or logging details.
          The summary for each file should be brief and structured as:
            <filename/path (wrapped in backticks)>
              - dot points of changes
          You don't need any extensions, don't mention extensions at all.
          The changes to summarise are:
          $(cat changes.txt)
          EOF
```

### 5. goose 실행 및 출력 정리

이제 형식화된 지침으로 goose를 실행하고 ANSI 색상 코드와 불필요한 로그 메시지를 제거하여 출력을 정리합니다.

```yaml
    - name: Run goose and filter output
      run: |
          goose run --instructions instructions.txt | \
            # ANSI 색상 코드 제거
            sed -E 's/\x1B\[[0-9;]*[mK]//g' | \
            # 세션/로깅 줄 제거
            grep -v "logging to /home/runner/.config/goose/sessions/" | \
            grep -v "^starting session" | \
            grep -v "^Closing session" | \
            # 후행 공백 제거
            sed 's/[[:space:]]*$//' \
            > pr_comment.txt
```

### 6. PR에 댓글 게시

마지막으로 goose 출력을 PR의 댓글로 게시합니다:

```yaml
    - name: Post comment to PR
      run: |
          cat -A pr_comment.txt
          gh pr comment $PR_NUMBER --body-file pr_comment.txt
```

이 워크플로우를 사용하면 goose가 PR에서 실행되어 변경 사항을 분석하고 PR에 요약을 댓글로 게시합니다.

이것은 가능한 것의 한 예일 뿐입니다. 필요에 맞게 GitHub Action을 자유롭게 수정하세요.

---

## 보안 고려 사항

CI/CD 환경에서 goose를 실행할 때 다음 보안 관행을 염두에 두세요:

1. **비밀 관리**
      - 민감한 자격 증명(예: API 키)을 GitHub Secrets로 저장하세요.
      - 로그나 PR 댓글에 이러한 자격 증명을 절대 노출하지 마세요.

2. **최소 권한 원칙**
      - 워크플로우에 필요한 권한만 부여하고 정기적으로 감사하세요.

3. **입력 검증**
      - goose에 전달되는 모든 입력이 예상치 못한 동작을 방지하기 위해 검사되고 검증되었는지 확인하세요.
