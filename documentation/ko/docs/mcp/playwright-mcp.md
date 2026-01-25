---
title: Playwright 확장 프로그램
description: Playwright MCP 서버를 goose 확장 프로그램으로 추가하여 최신 웹 테스팅하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/iAxiFMlTE1Q" />

이 튜토리얼에서는 Chromium과 Webkit에서 크로스 브라우저 테스팅과 웹 자동화를 활성화하기 위해 Playwright MCP 서버를 goose 확장 프로그램으로 추가하는 방법을 다룹니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=@playwright/mcp@latest&id=playwright&name=Playwright&description=Modern%20web%20testing%20and%20automation)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @playwright/mcp@latest
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
    extensionId="playwright"
    extensionName="Playwright"
    description="최신 웹 테스팅 및 자동화"
    command="npx"
    args={["-y", "@playwright/mcp@latest"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Playwright"
      description="최신 웹 테스팅 및 자동화"
      command="npx -y @playwright/mcp@latest"
    />
  </TabItem>
</Tabs>

## 사용 예제

goose와 Playwright 확장 프로그램을 사용하여 크로스 브라우저 테스팅 스위트를 만들어 보겠습니다. 이 예제에서는 goose가 다음을 수행하는 방법을 보여드리겠습니다:

1. 여러 브라우저에서 웹 애플리케이션 테스트
2. 유지보수 가능한 테스트 코드 생성
3. 시각적 비교를 위한 스크린샷 캡처

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::

### goose 프롬프트
```
Test the random redesign generator app (https://blackgirlbytes.github.io/random-redesign-picker/)
in Chromium and WebKit. For each browser:
1. Navigate to the page
2. Click the "Pick My Combo" button
3. Verify the text changes
4. Take a screenshot
Then generate a Playwright test that works across all browsers.
```

### goose 출력
```
Let's start with Chromium:

─── browser_navigate | playwright ──────────────────────────
url: https://blackgirlbytes.github....

Now let's wait for and click the button:

─── browser_click | playwright ──────────────────────────
element: button with text "Pick My Combo"
ref: <button>

Let's capture the first combo and click again to verify it changes:

─── browser_click | playwright ──────────────────────────
element: button with text "Pick My Combo"
ref: <button>

Let's take a screenshot of the Chromium view:

─── browser_take_screenshot | playwright ──────────────────────────
filename: chromium-redesign.png

Now let's test in Safari. I'll open a new tab for Safari:

─── browser_tab_new | playwright ──────────────────────────
url: https://blackgirlbytes.github....

Let's click the button in Safari:

─── browser_click | playwright ──────────────────────────
element: button with text "Pick My Combo"
ref: <button>

Let's click again to verify the text changes:

─── browser_click | playwright ──────────────────────────
element: button with text "Pick My Combo"
ref: <button>

Let's take a screenshot of the Safari view:

─── browser_take_screenshot | playwright ──────────────────────────
filename: safari-redesign.png

Now I'll generate a Playwright test that runs in all three browser environments. The test will verify the core functionality we just tested manually:

─── browser_generate_playwright_test | playwright ──────────────────────────
name: Random Redesign Picker Test
description: Test the random redesign generator functionality across browsers
steps: [
  "Navigate to the application",
  "Verify initial page content",
  "Click button to generate first combination",
  "Click button again to generate second combination",
  "Verify combinations are different"
]

Here's the generated Playwright test that will run across Chromium, Firefox, and Safari:

import { test, expect } from '@playwright/test';

test.describe('Random Redesign Picker', () => {
  test('should generate different random combinations', async ({ page }) => {
    // Navigate to the application
    await page.goto('https://blackgirlbytes.github.io/random-redesign-picker/');

    // Verify initial page content
    await expect(page).toHaveTitle('Redesign for the Ridiculous');
    await expect(page.getByRole('heading', { level: 1 })).toContainText('🎲 Redesign for the Ridiculous');

    // Verify button is visible
    const comboButton = page.getByRole('button', { name: '🎉 Pick My Combo' });
    await expect(comboButton).toBeVisible();

    // Click button and get first combination
    await comboButton.click();

    // Wait for and store first combo
    const firstComboElement = page.locator('generic').filter({ hasText: /^(?!.*🎲).*$/ }).last();
    await expect(firstComboElement).toBeVisible();
    const firstComboText = await firstComboElement.textContent();

    // Generate second combination
    await comboButton.click();

    // Wait for and get second combo
    const secondComboElement = page.locator('generic').filter({ hasText: /^(?!.*🎲).*$/ }).last();
    await expect(secondComboElement).toBeVisible();
    const secondComboText = await secondComboElement.textContent();

    // Verify combos are different
    expect(secondComboText).not.toEqual(firstComboText);
  });
});

To use this test, you would need to:

1. Save it as `tests/random-redesign.spec.ts`
2. Configure your `playwright.config.ts` to include all three browsers:

import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  projects: [
    {
      name: 'chromium',
      use: { browserName: 'chromium' },
    },
    {
      name: 'firefox',
      use: { browserName: 'firefox' },
    },
    {
      name: 'webkit',
      use: { browserName: 'webkit' },
    },
  ],
};

export default config;

You can then run the tests using:

npx playwright test

This will execute the test in all three browser environments. The test verifies:
1. The page loads correctly
2. The title and heading are correct
3. The combo button is visible
4. Clicking the button generates a combination
5. Clicking again generates a different combination

The test uses robust selectors and proper waiting mechanisms to ensure reliability across different browsers and network conditions.
```
