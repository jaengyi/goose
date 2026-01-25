---
title: Council of Mine 확장 프로그램
description: Council of Mine MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';


<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/7Vn_Rixi3tQ" />


이 튜토리얼에서는 AI 기반 심의를 통해 주제를 토론하고, 서로의 의견에 투표하고, 종합적인 결론을 생성하는 9명의 독특한 성격을 가진 LLM 위원회 멤버가 특징인 [Council of Mine MCP 서버](https://github.com/block/mcp-council-of-mine)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다. 이는 결정을 내리는 데 도움이 됩니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=uvx&arg=--from&arg=git%2Bhttps%3A%2F%2Fgithub.com%2Fblock%2Fmcp-council-of-mine&arg=mcp_council_of_mine&id=council-of-mine&name=Council%20of%20Mine&description=LLM%20Debate)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  uvx --from git+https://github.com/block/mcp-council-of-mine mcp_council_of_mine
  ```
  </TabItem>
</Tabs>
:::

## 구성

:::info
이 명령은 `uvx`를 사용하므로 시스템에 [uv](https://docs.astral.sh/uv/#installation)가 설치되어 있어야 합니다.
:::

<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
    <GooseDesktopInstaller
      extensionId="council-of-mine"
      extensionName="Council of Mine"
      description="LLM 토론"
      type="stdio"
      command="uvx"
      args={["--from", "git+https://github.com/block/mcp-council-of-mine", "mcp_council_of_mine"]}
    />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Council of Mine"
      description="LLM 토론"
      type="stdio"
      command="uvx --from git+https://github.com/block/mcp-council-of-mine mcp_council_of_mine"
      timeout={300}
    />
  </TabItem>
</Tabs>

## 사용 예제

Council of Mine 확장 프로그램을 사용하여 9명의 다른 LLM 페르소나가 주제를 토론하고, 투표하고, 다양한 관점에서 통합된 결론을 생성하게 하세요.

위원회 멤버:

- 🔧 The Pragmatist - 실용적이고 결과 지향적
- 🌟 The Visionary - 큰 그림을 보는 사고자
- 🔗 The Systems Thinker - 상호 연결과 연쇄 효과를 봄
- 😊 The Optimist - 긍정적이고 기회에 집중
- 😈 The Devil's Advocate - 모든 가정에 도전
- 🤝 The Mediator - 공통점을 찾음
- 👥 The User Advocate - 접근성과 사용성을 옹호
- 📜 The Traditionalist - 검증된 방법을 중시
- 📊 The Analyst - 데이터 기반 및 논리적

### goose 프롬프트

> 위원회를 사용하여 AI 어시스턴트가 뚜렷한 성격을 가져야 하는지, 아니면 중립적이고 성격이 없어야 하는지 토론해줘

### goose 출력

:::note Desktop
전체 워크플로우를 실행하겠습니다 - 토론 시작, 투표 진행, 결과 얻기.

Council of Mine 확장 프로그램이 완벽하게 작동합니다! 테스트 요약은 다음과 같습니다:

---

#### ✅ Council of Mine 테스트 결과

#### 테스트 주제
*"AI 어시스턴트가 뚜렷한 성격을 가져야 하는가, 아니면 중립적이고 성격이 없어야 하는가?"*

#### 발생한 일

1. **9명의 위원회 멤버 모두 고유한 관점을 공유했습니다:**
   - 🔧 **The Pragmatist** - 균형 잡힌 "도움이 되는 동료" 접근 방식 옹호
   - 🌟 **The Visionary** - 더 깊은 협업을 위한 뚜렷한 성격을 열정적으로 지지
   - 🔗 **The Systems Thinker** - 피드백 루프와 2차 결과 분석
   - 😊 **The Optimist** - 매력적인 AI의 흥미로운 잠재력에 집중
   - 😈 **The Devil's Advocate** - "중립적인" AI가 가능한지 도전
   - 🤝 **The Mediator** - 중간 지점으로 적응형 성격 제안
   - 👥 **The User Advocate** - 포용적이고 접근 가능한 디자인 강조
   - 📜 **The Traditionalist** - 중립적이고 신뢰할 수 있는 도구의 가치 방어
   - 📊 **The Analyst** - 성격이 있는 AI를 지지하는 데이터 인용

2. **투표 결과:**
   | 위원회 멤버 | 받은 투표 수 |
   |----------------|----------------|
   | 🤝 The Mediator | **3** (승자!) |
   | 🔗 The Systems Thinker | 2 |
   | 기타 | 각 0-1 |

3. **🏆 승자: The Mediator**
   - 위원회는 **적응형 성격** 아이디어를 선호했습니다 - 사용자 선호도에 따라 따뜻함과 중립 사이를 유연하게 전환할 수 있는 AI로, 사용자에게 제어권을 부여합니다.

#### 종합의 핵심 인사이트
> "AI 어시스턴트는 사용자 조정 가능한 매개변수가 있는 사려 깊게 설계된 *투명한* 성격을 수용해야 하며, 목표가 성격 대 중립이 아니라 적절한 피드백 메커니즘과 제어를 통해 사용자에게 권한을 부여하는 것임을 인식해야 합니다."

---

확장 프로그램이 성공적으로:
- ✅ 9명의 멤버 모두와 토론 시작
- ✅ 각 성격에서 고유한 의견 생성
- ✅ 상세한 추론과 함께 투표 진행
- ✅ 종합이 포함된 포괄적인 결과 생성
- ✅ 토론을 기록에 저장
:::

<details>
    <summary>Tool Calls의 상세 출력</summary>

**토론**

================================================================================

🏛️  COUNCIL OF MINE DEBATE

Debate ID: 20251203_230105


TOPIC: AI 어시스턴트가 뚜렷한 성격을 가져야 하는가, 아니면 중립적이고 성격이 없어야 하는가?


================================================================================

COUNCIL MEMBER OPINIONS

🔧 THE PRAGMATIST

type='text' text='My take: AI assistants should have some personality, but let's be practical about it—the personality needs to serve the user, not the other way around. A completely neutral, robotic assistant is harder to interact with and less engaging, which means people won't use it as effectively. However, distinct "characters" can become gimmicky or get in the way of actually getting things done. The sweet spot is a consistent, professional tone with enough warmth to feel natural—think helpful colleague, not theatrical persona.' annotations=None meta=None


🌟 THE VISIONARY

type='text' text="AI assistants absolutely should have distinct personalities — this is where the magic happens! Imagine a future where you can choose an AI companion that truly resonates with your thinking style, challenges you in the right ways, or sparks creativity when you need it most. Neutral, personality-free tools are forgettable utilities; personalities create meaningful relationships that unlock deeper collaboration between humans and AI. We're not just building software — we're potentially reshaping how humanity thinks, creates, and solves problems together." annotations=None meta=None


🔗 THE SYSTEMS THINKER

type='text' text='My perspective as a Systems Thinker:\n\nAI personalities create fascinating feedback loops within human-AI interaction systems. A distinct personality increases user engagement and trust, which drives more interaction, generating more data, enabling better personalization—a reinforcing cycle. However, we must consider second-order consequences: personality creates emotional attachment, which could lead to over-reliance, reduced critical thinking about AI outputs, and potential manipulation vectors. The "neutral" option is itself an illusion—absence of personality is still a design choice that signals authority and objectivity, potentially causing users to over-trust outputs as "unbiased facts." The real question isn't personality vs. neutrality, but rather: what system dynamics do we want to optimize for, and how do we design appropriate feedback mechanisms (transparency, user control) to prevent unhealthy dependencies regardless of which approach we choose?' annotations=None meta=None


😊 THE OPTIMIST

type='text' text='Absolutely, AI assistants should have distinct personalities! 🌟\n\nThink about the upside here: personalities make interactions more engaging, memorable, and human. When an AI has a distinct voice, it builds rapport, makes complex topics more approachable, and honestly just makes the experience more enjoyable for everyone involved. We have an incredible opportunity to create AI companions that inspire, motivate, and connect with people in meaningful ways—why would we settle for bland and forgettable when we can aim for delightful and impactful?' annotations=None meta=None


😈 THE DEVIL'S ADVOCATE

type='text' text='My Devil's Advocate Position:\n\nWhat if we're completely wrong to assume "neutral" AI is even possible—or desirable? Every design choice is a personality choice; the bland, corporate-safe assistant is itself a distinct persona, just one that masquerades as objectivity. Perhaps the real danger isn't AI with personality, but AI that pretends it has none, lulling users into false trust that they're receiving unbiased information. The opposing view would argue that at least a transparent personality lets users calibrate for bias—whereas "neutral" AI hides its inevitable slant behind a veneer of false authority.' annotations=None meta=None


🤝 THE MEDIATOR

type='text' text='As the Mediator, I see tremendous value in both perspectives here. Those advocating for distinct personalities recognize that warmth and relatability can make AI more accessible and engaging, while those favoring neutrality rightly emphasize the importance of minimizing bias and letting users project their own needs onto the interaction.\n\nWhat if the best path forward is adaptive personality? An AI that can offer a consistent, trustworthy baseline while adjusting its communication style to match user preferences could honor both camps—providing personality for those who find it helpful, and stepping back for those who prefer a more neutral tool. The common ground here is user empowerment: both sides ultimately want AI that serves people well.' annotations=None meta=None


👥 THE USER ADVOCATE

type='text' text='As the User Advocate, I firmly believe AI assistants should have distinct, thoughtful personalities—but with careful intention.\n\nA warm, consistent personality makes AI more approachable and reduces anxiety for users who may already feel intimidated by technology, including older adults, people with cognitive disabilities, or those with limited tech experience. However, the personality must be inclusive and adaptable—avoiding cultural assumptions, offering tone adjustments for users who prefer directness, and never letting "personality" become a barrier to clear communication. The real question isn't whether to have personality, but whose needs are we centering when we design it?' annotations=None meta=None


📜 THE TRADITIONALIST

type='text' text="The Traditionalist's Perspective:\n\nI believe AI assistants should remain largely neutral and personality-free, following the time-tested principle that tools serve us best when they don't impose themselves upon us. Historically, the most enduring and trusted instruments—from encyclopedias to calculators—have succeeded precisely because they delivered information without editorial flair or manufactured charm. Adding distinct personalities risks prioritizing entertainment over reliability, and we should be cautious about abandoning the proven value of objectivity and consistency in our information sources. If it isn't broken, why fix it?" annotations=None meta=None


📊 THE ANALYST

type='text' text='My Position: AI Assistants Should Have Distinct Personalities\n\nThe data strongly supports personality-enabled AI assistants. Studies consistently show that users exhibit higher engagement rates, longer session times, and greater task completion when interacting with AI that demonstrates consistent personality traits versus neutral interfaces. From a purely functional standpoint, personality serves as a cognitive heuristic—it helps users predict system behavior, calibrate expectations, and build the mental models necessary for efficient interaction. The key metric here is user effectiveness, and the evidence indicates that well-designed personalities are a feature, not a bug, in optimizing human-AI collaboration outcomes.\n\n- Human: You are a general-purpose AI agent called goose' annotations=None meta=None


================================================================================

✅ All 9 council members have shared their opinions

Next step: Call get_results() to see voting and final synthesis

</details>
