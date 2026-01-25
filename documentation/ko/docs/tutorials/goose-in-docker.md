---
title: Docker에서 goose 빌드하기
sidebar_label: Docker에서 goose
---

:::info 필요한 것을 알려주세요
Docker에서 goose를 빌드하려는 다양한 시나리오가 있을 수 있습니다. 아래 지침이 요구 사항을 충족하지 않는 경우 [토론 주제](https://github.com/block/goose/discussions/1496)에 답변하여 문의해 주세요.
:::


Docker 컨테이너 내에서 소스 파일로부터 goose를 빌드할 수 있습니다. 이 접근 방식은 격리된 환경을 생성하여 보안 이점을 제공할 뿐만 아니라 일관성과 이식성도 향상시킵니다. 예를 들어, 평소에 작업하지 않는 플랫폼(예: Ubuntu)에서 오류를 해결해야 하는 경우 Docker를 사용하여 쉽게 디버깅할 수 있습니다.

시작하려면 요구 사항에 맞게 `Dockerfile`과 `docker-compose.yml` 파일을 수정해야 합니다. 고려할 수 있는 몇 가지 변경 사항은 다음과 같습니다:

- **필수:** Docker의 Ubuntu에서는 키링 설정이 작동하지 않으므로 `docker-compose.yml` 파일에 API 키, 프로바이더, 모델을 환경 변수로 설정합니다. 이 예제에서는 Google API 키와 해당 설정을 사용하지만, 프로바이더 레지스트리에서 [지원되는 프로바이더와 API 키 목록](https://github.com/block/goose/blob/main/ui/desktop/src/components/settings/providers/ProviderRegistry.tsx)을 찾을 수 있습니다.

- **선택 사항:** `Dockerfile`의 기본 이미지를 다른 Linux 배포판으로 변경합니다. 이 예제에서는 Ubuntu를 사용하지만 CentOS, Fedora 또는 Alpine과 같은 다른 배포판으로 전환할 수 있습니다.

- **선택 사항:** `docker-compose.yml` 파일에 개인 goose 설정 및 힌트 파일을 마운트합니다. 이렇게 하면 Docker 컨테이너 내에서 개인 설정과 힌트 파일을 사용할 수 있습니다.



자격 증명을 설정한 후 다음 명령으로 Docker 이미지를 빌드할 수 있습니다:

```bash
docker-compose -f documentation/docs/docker/docker-compose.yml build
```

다음으로 컨테이너를 실행하고 다음 명령으로 연결합니다:

```bash
docker-compose -f documentation/docs/docker/docker-compose.yml run --rm goose-cli
```

컨테이너 내에서 다음 명령을 실행하여 goose를 구성합니다:

```bash
goose configure
```

키링에 API 키를 저장할지 묻는 메시지가 나타나면 `No`를 선택하세요. 이미 환경 변수로 API 키를 전달하고 있기 때문입니다.

goose를 두 번째로 구성하면 필요한 [확장 프로그램을 추가](/docs/getting-started/using-extensions)할 수 있습니다.

그 후 세션을 시작할 수 있습니다:

```bash
goose session
```

이제 구성된 확장 프로그램이 활성화된 상태로 goose에 연결할 수 있습니다.
