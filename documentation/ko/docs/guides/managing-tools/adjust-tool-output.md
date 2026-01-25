---
sidebar_position: 2
title: 도구 출력 상세도 조정
sidebar_label: 도구 출력 조정
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft } from 'lucide-react';

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
응답 스타일은 goose 데스크톱 채팅 창에서 도구 상호 작용이 표시되는 방식을 사용자 정의합니다.

이 설정을 변경하려면:
1. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바를 엽니다.
2. 사이드바에서 `Settings` 버튼을 클릭합니다.
3. `Chat`을 클릭합니다.
4. `Response Styles` 아래에서 `Detailed` 또는 `Concise`를 선택합니다.

- **Concise** (기본값)
    - 도구 호출이 기본적으로 축소됨
    - goose가 사용한 도구만 표시
    - 기술적 세부 사항보다 결과에 집중하는 사용자에게 적합

- **Detailed**
    - 도구 호출이 기본적으로 확장됨
    - 도구 호출의 세부 사항과 응답을 표시
    - 디버깅하거나 goose 작동 방식을 학습하는 데 적합

이 설정은 대화에서 도구 호출의 기본 상태에만 영향을 미칩니다. 선택한 스타일에 관계없이 언제든지 모든 도구 호출을 수동으로 확장하거나 축소할 수 있습니다.

</TabItem>
  <TabItem value="cli" label="goose CLI">
goose CLI로 작업할 때 도구 출력의 상세도를 제어할 수 있습니다.

도구 출력을 조정하려면 다음을 실행합니다:

```sh
goose configure
```

그런 다음 `Adjust Tool Output`을 선택합니다

```sh
┌   goose-configure
│
◆  What would you like to configure?
│  ○ Configure Providers
│  ○ Add Extension
│  ○ Toggle Extensions
│  ○ Remove Extension
// highlight-next-line
│  ● Adjust Tool Output (Show more or less tool output)
└
```

다음으로 사용 가능한 모드 중 하나를 선택합니다:

```sh
┌   goose-configure
│
◇  What would you like to configure?
│  Adjust Tool Output
│
// highlight-start
◆  Which tool output would you like to show?
│  ○ High Importance
│  ○ Medium Importance
│  ○ All
// highlight-end
└
```

- **High Importance**
    - 가장 중요한 도구 출력만 표시
    - 가장 최소한의 출력 수준

- **Medium Importance**
    - 중간 및 높은 중요도 출력 표시
    - 예: 파일 쓰기 작업의 결과

- **All**
    - 모든 도구 출력 표시
    - 예: 셸 명령 출력
    - 가장 상세한 수준
 </TabItem>
</Tabs>
