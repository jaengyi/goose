---
title: Cloudinary 자산 관리 확장 기능
description: Cloudinary 자산 관리 MCP 서버를 goose 확장 기능으로 추가
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/4diEvoRFVrQ" />

이 튜토리얼은 [Cloudinary 자산 관리 MCP 서버](https://github.com/cloudinary/asset-management-js)를 goose 확장 기능으로 추가하여 일반적으로 전문 디자인 소프트웨어나 수동 편집이 필요한 복잡한 이미지 처리 워크플로우를 자동화하는 방법을 다룹니다.

:::tip 요약

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=--package&arg=@cloudinary/asset-management&arg=--&arg=mcp&arg=start&id=cloudinary&name=Cloudinary%20Asset%20Management&description=Powerful%20media%20processing%20and%20transformation&env=CLOUDINARY_URL%3DCloudinary%20URL)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **명령**
  ```sh
  npx -y --package @cloudinary/asset-management -- mcp start
  ```
  </TabItem>
</Tabs>
  **환경 변수**
  ```
  CLOUDINARY_URL: cloudinary://<your_api_key>:<your_api_secret>@<your_cloud_name>
  ```
:::

## 구성

:::info
이 명령을 실행하려면 [Node.js](https://nodejs.org/)가 시스템에 설치되어 있어야 합니다(`npx` 사용). 또한 [Cloudinary 계정](https://cloudinary.com/users/register/free)이 필요합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose 데스크톱" default>
  <GooseDesktopInstaller
    extensionId="cloudinary"
    extensionName="Cloudinary Asset Management"
    description="강력한 미디어 처리 및 변환"
    command="npx"
    args={["-y", "--package", "@cloudinary/asset-management", "--", "mcp", "start"]}
    envVars={[{ name: "CLOUDINARY_URL", label: "Cloudinary URL (cloudinary://api_key:api_secret@cloud_name)" }]}
    apiKeyLink="https://console.cloudinary.com/settings/api-keys"
    apiKeyLinkText="Cloudinary URL"
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Cloudinary"
      description="강력한 미디어 처리 및 변환"
      command="npx -y --package @cloudinary/asset-management -- mcp start"
      envVars={[
        { key: "CLOUDINARY_URL", value: "▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪" }
      ]}
      infoNote={
        <>
          대시보드에서 <a href="https://console.cloudinary.com/settings/api-keys" target="_blank" rel="noopener noreferrer">Cloudinary URL</a>을 얻어 붙여넣으세요. 예: <code>cloudinary://&lt;your_api_key&gt;:&lt;your_api_secret&gt;@&lt;your_cloud_name&gt;</code>
        </>
      }
    />
  </TabItem>
</Tabs>

## 사용 예시

Cloudinary 확장 기능을 사용하여 고급 시각 효과로 제품 이미지를 찾고 변환해 보겠습니다. 이 예시에서는 goose가 다음을 도와드리는 방법을 보여줍니다:

1. 미디어 라이브러리에서 특정 이미지 검색
2. 배경 제거를 포함한 복잡한 변환 적용
3. 정확한 위치로 텍스트 오버레이 추가

:::info LLM
이 작업에는 Anthropic의 Claude 4 Sonnet이 사용되었습니다.
:::

### goose 프롬프트
```
1. 파일명이나 공개 ID에 'shoe'가 있는 내 Cloudinary 샘플에서 신발 이미지를 찾아줘.
2. 첫 번째 결과를 변환하고 신발만 추출해줘
3. "I love shoes"라는 흰색 텍스트 오버레이를 추가해줘. 신발 위에 있도록 해줘
4. 이미지에 배경을 추가해줘. 검은색이어야 해
```

### goose 출력
```
Cloudinary 샘플에서 신발 이미지를 찾고 요청하신 변환을 적용하겠습니다. 먼저 파일명이나 공개 ID에 'shoe'가 있는 이미지를 검색하겠습니다.

─── list-images | cloudinaryassetmanagement ──────────────────────────
maxResults: 50
prefix: samples
type: upload

완벽합니다! Cloudinary 샘플에서 신발 이미지를 찾았습니다. 다음을 볼 수 있습니다:

1. **`samples/shoe`** - 단일 신발 이미지 (publicId: `samples/shoe`)
2. **`samples/ecommerce/shoes`** - 한 켤레의 신발 (publicId: `samples/ecommerce/shoes`)

요청하신 대로 첫 번째 결과(`samples/shoe`)를 사용하고 원하는 모든 변환을 적용하겠습니다:

1. 신발만 추출 (배경 제거 사용)
2. 신발 위에 흰색 텍스트 오버레이 "I love shoes" 추가
3. 검은색 배경 추가

요청하신 모든 수정 사항이 포함된 변환된 이미지 URL입니다:

https://res.cloudinary.com/dd8qwbjtv/image/upload/e_background_removal/b_black/l_text:Arial_60:I%20love%20shoes,co_white,g_north,y_50/samples/shoe

각 변환이 수행하는 작업을 분석해 드립니다:

- `e_background_removal` - 배경을 제거하고 신발만 추출
- `b_black` - 제거된 배경을 대체할 검은색 배경 추가
- `l_text:Arial_60:I%20love%20shoes,co_white,g_north,y_50` - 흰색 텍스트 오버레이 추가:
  - `Arial_60` - Arial 글꼴, 크기 60
  - `I%20love%20shoes` - 텍스트 (URL 인코딩됨)
  - `co_white` - 흰색
  - `g_north` - 상단(북쪽)에 위치
  - `y_50` - 상단에서 50픽셀 오프셋

파일명/공개 ID에 "shoe"가 있는 이미지들입니다:

1. **첫 번째 결과 (변환에 사용됨)**: `samples/shoe`
   - 원본: https://res.cloudinary.com/dd8qwbjtv/image/upload/v1708364167/samples/shoe.jpg
   - **변환됨**: https://res.cloudinary.com/dd8qwbjtv/image/upload/e_background_removal/b_black/l_text:Arial_60:I%20love%20shoes,co_white,g_north,y_50/samples/shoe

2. **두 번째 결과**: `samples/ecommerce/shoes`
   - 원본: https://res.cloudinary.com/dd8qwbjtv/image/upload/v1708364144/samples/ecommerce/shoes.png

변환된 이미지가 이제 다음을 갖게 되었습니다:
✅ 신발만 추출됨 (배경 제거됨)
✅ 신발 위에 흰색 텍스트 "I love shoes" 배치됨
✅ 검은색 배경 적용됨

변환된 URL을 클릭하여 최종 결과를 확인할 수 있습니다!
```
