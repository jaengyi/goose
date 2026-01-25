---
title: Puppeteer 확장 프로그램
description: Puppeteer MCP 서버를 goose 확장 프로그램으로 추가하기
unlisted: true
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

<YouTubeShortEmbed videoUrl="https://youtube.com/embed/rms0wVGnlXA" />

서버 아카이브됨

이 튜토리얼에서는 goose가 웹사이트와 상호 작용할 수 있도록 [Puppeteer MCP 서버](https://www.pulsemcp.com/servers/merajmehrabi-puppeteer)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다. 페이지 탐색, 양식 작성, 버튼 클릭, 스크린샷 촬영, 실제 브라우저 환경에서 JavaScript 실행이 가능해집니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40modelcontextprotocol%2Fserver-puppeteer&id=puppeteer&name=Puppeteer&description=Headless%20browser%20automation)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @modelcontextprotocol/server-puppeteer
  ```
  </TabItem>
</Tabs>
:::


## 구성

:::info
이 명령은 `npx`를 사용하므로 시스템에 [Node.js](https://nodejs.org/)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  <GooseDesktopInstaller
    extensionId="puppeteer"
    extensionName="Puppeteer"
    description="헤드리스 브라우저 자동화"
    command="npx"
    args={["-y", "@modelcontextprotocol/server-puppeteer"]}
  />
</TabItem>
  <TabItem value="cli" label="goose CLI">
  1. `configure` 명령 실행:
  ```sh
  goose configure
  ```

  2. `Command-line Extension` 추가 선택
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◆  What type of extension would you like to add?
    │  ○ Built-in Extension
    // highlight-start
    │  ● Command-line Extension (Run a local command or script)
    // highlight-end
    │  ○ Remote Extension (Streamable HTTP)
    └
  ```

  3. 확장 프로그램 이름 지정
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    // highlight-start
    ◆  What would you like to call this extension?
    │  Puppeteer
    // highlight-end
    └
  ```

  4. 명령 입력
  ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  Puppeteer
    │
    // highlight-start
    ◆  What command should be run?
    │  npx -y @modelcontextprotocol/server-puppeteer
    // highlight-end
    └
  ```

  5. goose가 작업 완료를 기다리는 타임아웃 시간(초) 입력. 기본값은 300초입니다
   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  Puppeteer
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-puppeteer
    │
    // highlight-start
    ◆  Please set the timeout for this tool (in secs):
    │  300
    // highlight-end
    └
  ```

  6. 설명 추가 여부 선택. "Yes"를 선택하면 확장 프로그램 설명을 입력하라는 메시지가 표시됩니다.
   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  Puppeteer
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-puppeteer
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    // highlight-start
    ◇  Would you like to add a description?
    │  No
    // highlight-end
    └
  ```

  7. 환경 변수 추가 여부를 묻는 메시지에서 No 선택

   ```sh
    ┌   goose-configure
    │
    ◇  What would you like to configure?
    │  Add Extension (Connect to a new extension)
    │
    ◇  What type of extension would you like to add?
    │  Command-line Extension
    │
    ◇  What would you like to call this extension?
    │  Puppeteer
    │
    ◇  What command should be run?
    │  npx -y @modelcontextprotocol/server-puppeteer
    │
    ◇  Please set the timeout for this tool (in secs):
    │  300
    │
    ◇  Would you like to add a description?
    │  No
    │
    // highlight-start
    ◆  Would you like to add environment variables?
    │  No
    │
    // highlight-end
    └  Added puppeteer extension
  ```

  </TabItem>
</Tabs>


## 사용 예제
이 예제에서는 goose와 Puppeteer 확장 프로그램을 사용하여 웹사이트의 접근성 감사를 수행하는 방법을 보여드리겠습니다. 브라우저 상호 작용을 자동화하여 goose가 사이트를 탐색하고, 키보드 탐색을 테스트하고, 색상 대비를 확인하고, ARIA 속성을 분석하고, 권장 사항과 코드 수정 사항이 포함된 상세 보고서를 생성할 수 있습니다.

이를 통해 각 페이지를 수동으로 검사하지 않고도 접근성 문제를 빠르게 식별하고 해결할 수 있습니다.

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
   1. goose Desktop에서 새 세션 열기
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  1. 터미널을 열고 새 goose 세션 시작:

  ```sh
  goose session
  ```

  </TabItem>
</Tabs>

### goose 프롬프트
```
Can you check if my website is accessible? Please conduct a full accessibility audit, focusing on the following:

1. Keyboard Navigation: Test all interactive elements (links, buttons, dropdowns, etc.) to ensure they are accessible without a mouse.

2. Color Contrast Analysis: Verify that all text and UI elements meet WCAG contrast ratio standards.

3. Screen Reader Compatibility: Test whether a screen reader can properly interpret content and navigation.

4. ARIA & Semantic HTML Validation: Identify any missing or misused ARIA attributes and ensure proper semantic structure.

5. Error Identification & Screenshots: Provide a comprehensive report with identified issues, along with screenshots if necessary.

6. Fixes & Code Recommendations: Suggest HTML, CSS, or JavaScript changes to resolve any compliance issues.

Here is the website to test: https://broken-workshop.dequelabs.com/. I want to ensure that our documentation meets accessibility standards. Please generate a detailed report with steps to fix any issues found.

```

### goose 출력

:::note Desktop
I'll help you conduct a comprehensive accessibility audit of the website. I'll use a combination of automated and manual testing approaches to evaluate different accessibility aspects.

    1. First, let's navigate to the site
    2. Take screenshots and perform keyboard navigation testing
    3. Run accessibility audits including color contrast analysis
    4. Document findings

**Let's begin:**

    `─── Puppeteer Navigate ──────────────────────────`

**Let's take a screenshot of the homepage for reference:**

    `─── Puppeteer Screenshot ────────────────────────`


I'll conduct a detailed accessibility audit of the recipe dashboard website. From the initial view, I can already identify several accessibility concerns. Let me break down the findings by category:

**1. Keyboard Navigation Issues:**
- The "COOK" buttons for recipes appear to lack proper focus indicators
- The edit icons (pencil icons) on recipe cards don't seem to have keyboard focus states
- The navigation sequence may not be logical as there's no clear tab order

**2. Color Contrast Issues:**
- The light gray text used for "Difficulty" labels may not meet WCAG contrast requirements
- The green "Beginner" text appears to have insufficient contrast against the white background
- The bar chart in the "YUMMINESS" section uses colors that may be difficult to distinguish

**3. Semantic Structure Problems:**
- The heading hierarchy is unclear - "RECIPE DASHBOARD" appears to be an h1, but recipe titles don't seem to use proper heading tags
- The statistics (9 EGGS USED, 10 RECIPES MADE, etc.) don't appear to use semantic list structures
- The chart lacks proper ARIA labels for screen readers

**4. ARIA and Screen Reader Issues:**
- The recipe cards don't appear to have proper aria-labelledby associations
- The edit buttons lack aria-labels describing their purpose
- The chart data isn't properly accessible to screen readers
- No ARIA landmarks are visible for main navigation areas

Let me run some additional tests to verify these issues:

    `─── Puppeteer Evaluate ──────────────────────────`

Let me capture some specific sections for detailed analysis:

    `─── Puppeteer Screenshot ──────────────────────────`

Based on my analysis, here's a detailed report with recommendations:

**ACCESSIBILITY AUDIT REPORT**

**1. Structural Issues:**

```html
<!-- Current -->
<div class="recipe-title">Chocolate Cake</div>

<!-- Recommended -->
<h2 class="recipe-title">Chocolate Cake</h2>
```
**2. Interactive Elements:**

```html
<!-- Current -->
<button class="cook-button">COOK CHOCOLATE CAKE</button>

<!-- Recommended -->
<button
  class="cook-button"
  aria-label="Cook Chocolate Cake recipe"
  tabindex="0">
  COOK CHOCOLATE CAKE
</button>
```
**3. Image Accessibility:**
```html
<!-- Current -->
<img src="chocolate-cake.jpg">

<!-- Recommended -->
<img
  src="chocolate-cake.jpg"
  alt="Chocolate cake with dark chocolate frosting"
  role="img">
```

**4. Chart Accessibility:**

```html
<!-- Recommended addition -->
<div class="yumminess-chart"
     role="region"
     aria-label="Monthly yumminess ratings">
  <table class="sr-only">
    <!-- Add hidden table for screen readers -->
  </table>
</div>
```
**5. Color Contrast Fixes:**

```css
/* Current */
.difficulty-beginner {
  color: #90EE90;
}

/* Recommended */
.difficulty-beginner {
  color: #2E7D32; /* Darker green for better contrast */
}
```
**6. Focus Management:**

```css
/* Add to CSS */
:focus {
  outline: 3px solid #2196F3;
  outline-offset: 2px;
}

.cook-button:focus {
  box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.4);
}
```
**Priority Fixes Needed:**

**1. High Priority:**

- Add proper heading structure using h1-h6 tags
- Implement keyboard focus indicators
- Add alt text to all recipe images
- Fix color contrast for difficulty labels

**2. Medium Priority:**

- Add ARIA labels to interactive elements
- Implement proper list structure for statistics
- Add skip links for keyboard navigation
- Make chart data accessible via screen reader

**3. Low Priority:**

- Enhance focus order
- Add ARIA landmarks
- Implement announcement of dynamic content changes

**Additional Recommendations:**

**1. Add a skip link:**

```html
<a href="#main-content" class="skip-link">
  Skip to main content
</a>
```
**2. Implement proper form labels:**

```html
<label for="prep-time">Preparation Time</label>
<input id="prep-time" type="text" value="20 min">
```
**3. Add ARIA live regions for dynamic updates:**

```html
<div aria-live="polite" role="status">
  <!-- Dynamic content updates -->
</div>
```

**Testing Tools Recommended:**

1. WAVE Web Accessibility Evaluation Tool
2. axe DevTools
3. Keyboard-only navigation testing
4. Screen reader testing with VoiceOver (Mac)

This audit reveals several critical accessibility issues that should be addressed to ensure WCAG 2.1 compliance. The most urgent concerns are the lack of proper semantic structure, insufficient color contrast, and missing ARIA attributes. Would you like me to provide more specific details about implementing any of these fixes?
:::

### GitHub 확장 프로그램을 통한 추가 자동화

[GitHub 확장 프로그램](/docs/mcp/github-mcp)과 Puppeteer 확장 프로그램을 결합하면 이 접근성 감사를 한 단계 더 발전시킬 수 있습니다. 이 설정을 사용하면 goose가 문제를 찾고 수정 사항을 적용할 뿐만 아니라 전체 Git 워크플로우도 처리할 수 있습니다. GitHub 확장 프로그램을 사용하면 goose가 변경 사항을 커밋하고, 풀 리퀘스트를 만들고, PR 설명까지 생성할 수 있으므로 검토하고 병합하기만 하면 됩니다.

1. **[GitHub 확장 프로그램 튜토리얼](/docs/mcp/github-mcp#configuration)**의 단계를 따라 GitHub 확장 프로그램을 활성화하세요.


:::tip
이 통합 접근 방식을 사용할 때 GitHub Personal Access Token에 저장소 액세스 및 풀 리퀘스트 생성에 필요한 권한이 있는지 확인하세요.
:::

2. goose에게 다음을 요청하세요:

   - 새 브랜치 생성
   - 접근성 개선 사항 커밋
   - 변경 사항으로 풀 리퀘스트 열기

### goose 프롬프트:

```
Can you create a new branch called 'accessibility-improvements', apply the accessibility fixes you suggested, and open a pull request with these changes?
```
그러면 goose가:
   - ✅ 브랜치 생성: `accessibility-improvements`
   - ✅ 권장 접근성 수정 사항 적용
   - ✅ 설명적인 메시지와 함께 변경 사항 커밋
   - ✅ 개선 사항 요약이 포함된 풀 리퀘스트 열기
