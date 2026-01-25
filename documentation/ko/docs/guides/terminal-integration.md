# 터미널 통합

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

셸 프롬프트에서 직접 goose와 대화하세요. 별도의 REPL 세션으로 전환하는 대신 터미널에서 필요할 때 goose를 호출하세요.

## 설정

<Tabs groupId="shells">
<TabItem value="zsh" label="zsh" default>

`~/.zshrc`에 추가:
```bash
eval "$(goose term init zsh)"
```

</TabItem>
<TabItem value="bash" label="bash">

`~/.bashrc`에 추가:
```bash
eval "$(goose term init bash)"
```

</TabItem>
<TabItem value="fish" label="fish">

`~/.config/fish/config.fish`에 추가:
```fish
goose term init fish | source
```

</TabItem>
<TabItem value="powershell" label="PowerShell">

`$PROFILE`에 추가:
```powershell
Invoke-Expression (goose term init powershell)
```

</TabItem>
</Tabs>

터미널을 다시 시작하거나 구성을 소스하면 완료입니다!

## 사용법

`@goose`(또는 줄여서 `@g`)를 입력하고 질문을 이어서 입력하세요:

```bash
npm install express
    npm ERR! code EACCES
    npm ERR! permission denied

@goose "how do I fix this error?"
```

goose는 마지막 질문 이후 실행한 명령을 자동으로 확인하므로 무엇을 하고 있었는지 설명할 필요가 없습니다. 프롬프트에 `?`, `*`, `'`와 같은 특수 문자가 포함된 경우 따옴표를 사용하세요:

```bash
@goose "what's in this directory?"
@g "analyze the error: 'permission denied'"
```

## 이름이 있는 세션
기본적으로 각 터미널은 닫을 때까지 지속되는 자체 goose 세션을 가집니다. 이름이 있는 세션을 사용하면 터미널을 다시 시작해도 대화를 계속하고 창 간에 컨텍스트를 공유할 수 있습니다.

<Tabs groupId="shells">
<TabItem value="zsh" label="zsh" default>

```bash
eval "$(goose term init zsh --name my-project)"
```

</TabItem>
<TabItem value="bash" label="bash">

```bash
eval "$(goose term init bash --name my-project)"
```

</TabItem>
<TabItem value="fish" label="fish">

```fish
goose term init fish --name my-project | source
```

</TabItem>
<TabItem value="powershell" label="PowerShell">

```powershell
Invoke-Expression (goose term init powershell --name my-project)
```

</TabItem>
</Tabs>

이름이 있는 세션은 goose 데이터베이스에 지속되므로 컴퓨터를 다시 시작한 후에도 언제든지 사용할 수 있습니다. 나중에 다시 열고 동일한 명령을 실행하여 계속하세요:

```bash
# 디버깅 시작
eval "$(goose term init zsh --name auth-bug)"
@goose help me debug this login timeout

# 터미널 닫기, 나중에 다시 열기
eval "$(goose term init zsh --name auth-bug)"
@goose "what was the solution we discussed?"
# 컨텍스트와 함께 동일한 대화 계속
```

## 프롬프트에 컨텍스트 상태 표시

터미널 goose 세션 중에 사용한 컨텍스트 양과 활성화된 모델을 확인하려면 프롬프트에 `goose term info`를 추가하세요.

<Tabs groupId="shells">
<TabItem value="zsh" label="zsh" default>

```bash
PROMPT='$(goose term info) %~ $ '
```

</TabItem>
<TabItem value="bash" label="bash">

```bash
PS1='$(goose term info) \w $ '
```

</TabItem>
<TabItem value="fish" label="fish">

```fish
function fish_prompt
    goose term info
    echo -n ' '(prompt_pwd)' $ '
end
```

</TabItem>
<TabItem value="powershell" label="PowerShell">

```powershell
function prompt {
    $gooseInfo = & goose term info
    "$gooseInfo $(Get-Location) PS> "
}
```

</TabItem>
</Tabs>

이제 터미널 프롬프트에 활성 goose 세션의 컨텍스트 사용량과 모델 이름(가독성을 위해 단축됨)이 표시됩니다. 예:

```bash
●●○○○ sonnet ~/projects $
```
## goose 명령에 대한 셸 자동 완성

`@goose`는 명령 기록을 기반으로 컨텍스트 인식 지원을 제공합니다. goose CLI 명령(예: `goose session`, `goose run` 등)의 탭 완성을 활성화하려면 [셸 완성 문서](/docs/guides/goose-cli-commands#completion)를 참조하세요.

## 문제 해결

**goose가 최근 명령을 보지 못함:**
명령을 실행했는데 goose가 최근 활동을 보지 못한다고 하면 터미널 통합이 [셸 구성에 올바르게 설정](#설정)되어 있는지 확인하세요.
현재 터미널에서 goose 세션 ID를 확인할 수도 있습니다:
```bash
# 세션 ID가 있는지 확인
echo $GOOSE_SESSION_ID
# 다음과 같이 표시되어야 함: 20251209_151730
```
터미널 창 간에 컨텍스트를 공유하려면 대신 [이름이 있는 세션](#이름이-있는-세션)을 사용하세요.

**세션이 너무 가득 참** (프롬프트에 `●●●●●` 표시):
goose의 응답이 느려지거나 컨텍스트 제한에 도달하면 터미널에서 새 goose 세션을 시작하세요. 새 goose 세션은 명령 기록을 볼 수 있지만 이전 세션의 대화 기록은 볼 수 없습니다.
```bash
# 동일한 셸에서 새 goose 세션 시작
eval "$(goose term init zsh)"
```
