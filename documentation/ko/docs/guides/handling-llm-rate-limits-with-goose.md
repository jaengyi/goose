---
title: LLM 속도 제한 설정
sidebar_label: LLM 속도 제한
sidebar_position: 60
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import { PanelLeft } from 'lucide-react';


속도 제한은 사용자 또는 애플리케이션이 특정 시간 내에 LLM API로 보낼 수 있는 요청 수를 제한하는 프로세스입니다. LLM 공급자는 리소스를 관리하고 남용을 방지하기 위해 이를 시행합니다.

goose는 작업을 매우 빠르게 구현하기 때문에 공급자가 부과하는 속도 제한을 관리해야 할 수 있습니다. 속도 제한에 자주 도달하는 경우 LLM 요금제를 업그레이드하여 더 높은 수준의 제한에 액세스하거나 기본 속도 제한이 내장된 [공급자를 구성](/docs/getting-started/providers#configure-provider-and-model)하는 것을 고려하세요:

:::info
goose는 OAuth 계정 생성과 안전한 API 키 생성을 안내하는 두 공급자 모두에 대한 자동 설정을 지원합니다.
:::

- **Tetrate Agent Router**: Claude, Gemini, GPT, 오픈 웨이트 모델 등을 포함한 AI 모델을 위한 통합 API 게이트웨이입니다. 엔터프라이즈급 라우팅, 기본 속도 제한 및 자동 장애 조치 기능을 갖춘 개발자를 위한 가장 빠른 모델 경로입니다.

  [router.tetrate.ai](https://router.tetrate.ai/dashboard)에서 계정을 관리하세요.

- **OpenRouter**: 다양한 공급자 간에 자동으로 선택하고 전환할 수 있게 해주는 LLM을 위한 통합 인터페이스를 제공합니다 - 모두 단일 청구 요금제로. OpenRouter를 사용하면 무료 모델을 활용하거나 유료 모델에 대한 크레딧을 구매할 수 있습니다.

  [openrouter.ai](https://openrouter.ai)에서 계정을 관리하세요.

goose가 이러한 공급자 중 하나를 통해 요청을 보내면 공급자는 속도 제한으로 인한 중단을 피하기 위해 필요할 때 자동으로 모델을 전환합니다.
