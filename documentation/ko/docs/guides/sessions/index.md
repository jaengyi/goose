---
title: 세션 관리
hide_title: true
description: goose와의 세션 수명 주기 및 지속적인 상호작용 관리
---

import Card from '@site/src/components/Card';
import styles from '@site/src/components/Card/styles.module.css';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<h1 className={styles.pageTitle}>세션 관리</h1>
<p className={styles.pageDescription}>
  세션은 goose와의 지속적인 상호작용입니다. 각 세션은 컨텍스트와 대화 기록을 유지하여 goose가 진행 중인 작업을 이해하고 관련 지원을 제공할 수 있게 합니다.
</p>

<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>📚 문서 및 가이드</h2>
  <div className={styles.cardGrid}>
    <Card
      title="세션 관리"
      description="세션 시작, 재개, 검색 및 기타 세션 관리 작업 방법을 알아보세요."
      link="/docs/guides/sessions/session-management"
    />
    <Card
      title="세션 내 작업"
      description="세션 중 goose와 정보를 공유하고 소통하는 데 사용할 수 있는 기능을 알아보세요."
      link="/docs/guides/sessions/in-session-actions"
    />
    <Card
      title="스마트 컨텍스트 관리"
      description="컨텍스트 및 대화 제한을 관리하는 기능을 사용하여 생산적인 세션을 유지하세요."
      link="/docs/guides/sessions/smart-context-management"
    />
  </div>
</div>
<div className={styles.categorySection}>
  <h2 className={styles.categoryTitle}>📝 주요 블로그 게시물</h2>
  <div className={styles.cardGrid}>
    <Card
      title="goose 활용을 위한 6가지 필수 팁"
      description="집중된 세션, 단계별 안내 및 프롬프트 개선이 더 생산적인 세션으로 이어지는 방법을 알아보세요."
      link="/blog/2025/03/06/goose-tips"
    />
    <Card
      title="AI 프롬프팅 101: AI 에이전트에서 최상의 응답을 얻는 방법"
      description="프롬프트에 구조를 추가하여 세션을 더 효과적으로 만드세요."
      link="/blog/2025/03/19/better-ai-prompting"
    />
    <Card
      title="AI 회의론자를 위한 컨텍스트 윈도우 가이드"
      description="컨텍스트 윈도우, 토큰 및 goose가 메모리와 긴 대화를 관리하는 방법을 알아보세요."
      link="/blog/2025/08/18/understanding-context-windows"
    />
  </div>
</div>
