---
title: 실험적 기능
hide_title: true
description: 실험적이고 매우 불안정하지만, 많은 재미가 있습니다.
---

import Card from '@site/src/components/Card';
import styles from '@site/src/components/Card/styles.module.css';

<h1 className={styles.pageTitle}>실험적 기능</h1>
<p className={styles.pageDescription}>
  goose는 끊임없이 개선되고 확장되는 오픈 소스 프로젝트입니다. 이러한 실험적 기능과 프로젝트는 아직 개발 중이며 완전히 안정적이거나 프로덕션 사용 준비가 되지 않았을 수 있지만, AI 자동화의 미래에 대한 흥미로운 가능성을 보여줍니다.
</p>

:::note
실험적 기능 목록은 goose 개발이 진행됨에 따라 변경될 수 있습니다. 일부 기능은 안정적인 기능으로 승격될 수 있고, 다른 기능은 수정되거나 제거될 수 있습니다. 이 섹션은 특정 실험적 기능이 사용 가능해지면 업데이트됩니다.
:::

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>🧪 실험적 기능</h2>
  <div className={styles.cardGrid}>
    <Card
      title="Ollama Tool Shim"
      description="실험적인 로컬 인터프리터 모델 설정을 사용하여 도구 호출을 기본적으로 지원하지 않는 언어 모델(예: DeepSeek)에 도구 호출 기능을 활성화합니다."
      link="/docs/experimental/ollama"
    />
    <Card
      title="보안 터널링을 통한 모바일 액세스"
      description="보안 터널링을 통해 goose AI 모바일 앱에서 goose Desktop에 대한 원격 액세스를 활성화합니다."
      link="/docs/experimental/mobile-access"
    />
    <Card
      title="VS Code용 goose 확장 프로그램"
      description="ACP를 통해 VS Code에서 직접 goose와 상호 작용합니다."
      link="/docs/experimental/vs-code-extension"
    />
    <Card
      title="ACP 클라이언트에서 goose 사용"
      description="Zed와 같은 ACP 호환 클라이언트에서 goose와 네이티브로 상호 작용합니다."
      link="/docs/guides/acp-clients"
    />
  </div>
</div>

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>📝 주요 블로그 게시물</h2>
  <div className={styles.cardGrid}>
    <Card
      title="도구 호출을 위한 Toolshim 모델 파인튜닝"
      description="전용 toolshim 모델 개발을 통해 기본 도구 호출 지원이 없는 모델의 성능 제한을 해결합니다."
      link="/blog/2025/04/11/finetuning-toolshim"
    />
    <Card
      title="goose와 Ollama로 로컬 AI 구현하기"
      description="구조화된 출력 및 도구 호출 기능을 포함하여 완전히 로컬 AI 경험을 위해 goose와 Ollama를 통합하는 방법을 알아봅니다."
      link="/blog/2025/03/14/goose-ollama"
    />
    <Card
      title="커뮤니티에서 영감을 받은 벤치마킹: goose Vibe Check"
      description="toolshim 성능 분석을 포함하여 첫 번째 goose 에이전트 벤치마크 테스트에서 오픈 소스 AI 모델이 어떻게 측정되는지 확인하세요."
      link="/blog/2025/03/31/goose-benchmark"
    />
  </div>
</div>

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>💬 피드백 및 지원</h2>
  <div className={styles.cardGrid}>
    <Card
      title="GitHub Issues"
      description="버그 보고, 기능 요청 또는 실험적 기능 개발에 기여합니다."
      link="https://github.com/block/goose/issues"
    />
    <Card
      title="Discord 커뮤니티"
      description="실험적 기능에 대해 토론하고, 피드백을 공유하고, 다른 사용자와 연결하려면 커뮤니티에 참여하세요."
      link="https://discord.gg/goose-oss"
    />
  </div>
</div>
