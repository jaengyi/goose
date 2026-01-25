---
title: 도구 관리
hide_title: true
description: goose 워크플로우를 구동하는 도구와 확장 기능 제어 및 구성
---

import Card from '@site/src/components/Card';
import styles from '@site/src/components/Card/styles.module.css';

<h1 className={styles.pageTitle}>도구 관리</h1>
<p className={styles.pageDescription}>
  도구는 goose에게 기능을 제공하는 <a href="/goose/docs/getting-started/using-extensions">확장 기능</a> 내의 특정 함수입니다. 이러한 도구가 어떻게 작동하는지 제어하고 사용자 정의하는 방법을 알아보세요.
</p>

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>📚 문서 및 가이드</h2>
  <div className={styles.cardGrid}>
    <Card
      title="도구 권한"
      description="goose가 어떤 도구를 언제 사용할 수 있는지 제어하는 세분화된 권한을 구성하여 안전하고 통제된 자동화를 보장합니다."
      link="/docs/guides/managing-tools/tool-permissions"
    />
    <Card
      title="도구 출력 조정"
      description="도구 상호 작용 표시 방식을 사용자 정의합니다. 상세한 자세한 출력부터 깔끔하고 간결한 요약까지."
      link="/docs/guides/managing-tools/adjust-tool-output"
    />
    <Card
      title="코드 모드"
      description="MCP 도구를 필요에 따라 검색하고 호출하는 프로그래밍 방식 접근법."
      link="/docs/guides/managing-tools/code-mode"
    />
    <Card
      title="Ollama 도구 심"
      description="실험적인 로컬 인터프리터 모델 설정을 사용하여 도구 호출을 기본적으로 지원하지 않는 모델에서 도구 호출을 활성화합니다."
      link="/docs/experimental/ollama"
    />
  </div>
</div>

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>📝 주요 블로그 게시물</h2>
  <div className={styles.cardGrid}>
    <Card
      title="에이전트 AI와 MCP 생태계"
      description="AI 에이전트, 도구 호출 및 도구가 LLM과 함께 작동하여 강력한 자동화를 가능하게 하는 방법에 대한 101 소개."
      link="/blog/2025/02/17/agentic-ai-mcp"
    />
    <Card
      title="MCP 생태계 시각적 가이드"
      description="MCP의 시각적 분석: AI 에이전트, 도구 및 모델이 함께 작동하는 방식을 다이어그램과 비유로 설명합니다."
      link="/blog/2025/04/10/visual-guide-mcp"
    />
    <Card
      title="도구 호출을 위한 Toolshim 모델 파인튜닝"
      description="오픈소스 모델을 사용한 도구 호출의 과제와 toolshim 솔루션 뒤에 있는 연구에 대한 기술 심층 분석."
      link="/blog/2025/04/11/finetuning-toolshim"
    />
  </div>
</div>
