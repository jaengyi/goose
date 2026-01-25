---
title: 보안 터널링을 통한 모바일 액세스
sidebar_position: 3
sidebar_label: 모바일 액세스
description: 보안 터널링을 사용하여 모바일 기기에서 goose에 대한 원격 액세스를 활성화합니다.
---

import { PanelLeft } from 'lucide-react';

모바일 액세스를 사용하면 보안 터널링을 사용하여 iOS 모바일 기기에서 goose에 원격으로 연결할 수 있습니다.

:::warning 실험적 기능
모바일 액세스는 활발히 개발 중인 미리보기 기능입니다. 동작과 구성은 향후 릴리스에서 변경될 수 있습니다.
:::

## 모바일 액세스 작동 방식

모바일 액세스는 보안 터널을 통해 iOS 기기를 goose Desktop에 연결합니다. **goose AI** 앱을 설치하고 구성하면 어디서나 goose에 액세스할 수 있습니다.

**주요 세부 사항:**
- Mic Neale이 제공하는 공개 HTTPS 터널 서비스인 [Lapstone](https://github.com/michaelneale/lapstone-tunnel) 사용
- 연결을 보호하기 위한 고유 비밀 키가 있는 QR 코드를 사용한 쉬운 설정
- 터널 URL이 세션 간에 동일하게 유지되므로 모바일 앱을 한 번만 구성하면 됨
- 연결하려면 컴퓨터가 깨어 있고 goose Desktop이 실행 중이어야 함
- 중단되면 자동으로 재연결되고 goose Desktop을 시작하면 재시작

## 설정

### 앱 설치
1. [App Store](https://apps.apple.com/app/goose-ai/id6752889295)에서 iOS 모바일 기기에 **goose AI** 앱 설치

:::tip App Store QR 코드
아래 단계에 따라 `Remote Access` 섹션을 열고 정보 상자에서 "scan QR code"를 클릭하면 App Store에 빠르게 액세스할 수 있습니다.
:::

### 터널 시작
1. goose Desktop 열기
2. 왼쪽 상단의 <PanelLeft className="inline" size={16} /> 버튼을 클릭하여 사이드바 열기
3. 사이드바에서 `Settings` 클릭
4. `App` 클릭
5. `Remote Access` 섹션으로 스크롤하고 `Start Tunnel` 클릭

터널이 시작되면 앱 구성을 위한 `Remote Access Connection` QR 코드가 표시됩니다.

:::info
언제든지 `Stop Tunnel`을 클릭하여 연결을 닫을 수 있습니다.
:::

### 앱 연결
1. iOS 모바일 기기에서 **goose AI** 앱 열기
2. goose Desktop에 표시된 `Remote Access Connection` QR 코드 스캔
3. 앱이 자동으로 연결 구성

이제 모바일 기기에서 goose Desktop에 액세스할 수 있습니다.

## 할 수 있는 일

모바일 앱은 goose에 대한 전체 액세스를 제공합니다:
- 새 대화를 시작하거나 기존 세션 계속
- 모든 goose 확장 프로그램 및 구성 사용
- 컴퓨터가 처리를 수행하는 동안 어디서나 작업

## 추가 리소스

import ContentCardCarousel from '@site/src/components/ContentCardCarousel';
import mobileShots from '@site/blog/2025-12-19-goose-mobile-terminal/mobile_shots.png';

<ContentCardCarousel
  items={[
    {
      type: 'blog',
      title: 'goose 모바일 액세스 및 네이티브 터미널 지원',
      description: 'goose를 사용하는 두 가지 새로운 방법에 대해 알아보세요: 모바일 액세스를 위한 iOS 앱과 원활한 세션 연속성을 갖춘 네이티브 터미널 지원.',
      thumbnailUrl: mobileShots,
      linkUrl: '/goose/blog/2025/12/19/goose-mobile-terminal',
      date: '2025-12-19',
      duration: '4분 읽기'
    }
  ]}
/>
