---
title: Knowledge Graph Memory 확장 프로그램
description: Knowledge Graph Memory MCP 서버를 goose 확장 프로그램으로 추가하기
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import YouTubeShortEmbed from '@site/src/components/YouTubeShortEmbed';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';

<YouTubeShortEmbed videoUrl="https://www.youtube.com/embed/PF6hpDaI9Mc" />


이 튜토리얼에서는 [Knowledge Graph Memory MCP 서버](https://github.com/modelcontextprotocol/servers/tree/main/src/memory)를 goose 확장 프로그램으로 추가하는 방법을 다룹니다. 이를 통해 goose가 관계를 분석하고, 패턴을 감지하고, 데이터에 대한 더 깊은 이해를 얻을 수 있습니다. 지식 그래프는 개념 간의 복잡한 관계를 매핑하고 goose 세션 간에 영구 메모리를 제공하여 [memory 확장 프로그램](/docs/mcp/memory-mcp)을 기반으로 합니다.

:::tip 요약
<Tabs groupId="interface">
  <TabItem value="ui" label="goose Desktop" default>
  [설치 프로그램 실행](goose://extension?cmd=npx&arg=-y&arg=%40modelcontextprotocol%2Fserver-memory&id=knowledge_graph_memory&name=Knowledge%20Graph%20Memory&description=Graph-based%20memory%20system%20for%20persistent%20knowledge%20storage)
  </TabItem>
  <TabItem value="cli" label="goose CLI">
  **Command**
  ```sh
  npx -y @modelcontextprotocol/server-memory
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
    extensionId="knowledge_graph_memory"
    extensionName="Knowledge Graph Memory"
    description="영구 지식 저장을 위한 그래프 기반 메모리 시스템"
    command="npx"
    args={["-y", "@modelcontextprotocol/server-memory"]}
  />
  </TabItem>
  <TabItem value="cli" label="goose CLI">
    <CLIExtensionInstructions
      name="Knowledge Graph Memory"
      description="영구 지식 저장을 위한 그래프 기반 메모리 시스템"
      command="npx -y @modelcontextprotocol/server-memory"
    />
  </TabItem>
</Tabs>

## 사용 예제

이 예제에서는 goose가 연결된 지식 패턴을 사용하여 취약점을 감지하고 분석하는 지능형 보안 검토자가 되는 방법을 보여드리겠습니다. goose는 보안 문제, 영향 및 완화 방법 간의 관계를 이해할 수 있습니다.

즉, goose는 문제만 발견하는 것이 아니라 취약점이 실제 영향과 어떻게 연결되는지 이해하고 포괄적인 솔루션을 제안할 수 있습니다.

### 1단계: goose에게 보안 패턴 가르치기
    #### goose 프롬프트 #1
    ```
    goose, 다음 보안 취약점 패턴과 그 관계를 학습해줘:

    1. SQL Injection은 다음과 관련:

        - 유효성 검사되지 않은 데이터베이스 입력
        - 데이터 도난 위험
        - 매개변수화된 쿼리 솔루션

    2. XSS 취약점은 다음과 연결:

        - 이스케이프되지 않은 사용자 출력
        - 세션 하이재킹 위험
        - 콘텐츠 새니타이제이션 수정

    3. 인증 약점은 다음과 연결:

        - 세션 관리
        - 계정 침해
        - JWT 기반 솔루션
    ```

    #### goose 출력

    ```
    이 보안 지식을 적절한 엔티티 및 관계와 함께 Knowledge Graph Memory에 저장하겠습니다

    [.... 엔티티 생성 .....]

    이제 이러한 보안 개념 간의 관계를 생성하겠습니다:

    [.... 관계 생성 .....]

    저장된 내용을 확인하겠습니다:

    [... 그래프 읽기 ...]

    보안 지식을 그래프에 저장했습니다:

    1. 취약점 엔티티:

        - SQL Injection (유효성 검사되지 않은 입력 및 데이터 도난에 대한 관찰 포함)
        - XSS 취약점 (이스케이프되지 않은 출력 및 세션 위험에 대한 관찰 포함)
        - 인증 약점 (세션 관리에 대한 관찰 포함)

    2. 솔루션 엔티티:

        - 매개변수화된 쿼리
        - 콘텐츠 새니타이제이션
        - JWT 인증

    3. 연결된 관계:

        - 취약점 → 잠재적 영향
        - 솔루션 → 완화하는 취약점

    지식이 이제 다음을 수행할 수 있는 방식으로 구조화되었습니다:

        - 취약점 영향 체인 추적
        - 각 취약점에 적합한 솔루션 식별
        - 다른 보안 문제가 어떻게 관련되어 있는지 이해
    ```
### 2단계: 코드 검토에 연결된 지식 사용
    #### goose 프롬프트 # 2

    ```
    이 API 엔드포인트에서 보안 문제를 검토해줘:
    ```
    ```js
    app.post('/api/login', (req, res) => {
        const { username, password } = req.body;
        const query = `SELECT * FROM users WHERE username = '${username}'`;
        db.query(query, (err, result) => {
            if (err) throw err;
            if (result.length > 0) {
            res.cookie('user', username);
            res.json({ success: true });
            }
        });
    });
    ```
    #### goose 출력
    ```
    연결된 지식 그래프를 사용하여 이 코드를 분석하여 보안 취약점과 그 영향을 추적하겠습니다:

    [.... 노드 검색 ... 노드 열기 ...]

    지식 그래프 분석을 기반으로 여러 중요한 보안 문제를 발견했습니다:

    1. SQL Injection 취약점:
        - 문제: 쿼리에서 문자열 연결 사용 WHERE username = '${username}'
        - 영향 체인: SQL Injection → 데이터 도난
        - 위험 수준: Critical
        - 솔루션 (parameterized_queries에서): 대신 매개변수화된 쿼리 사용

    2. 인증 약점:
        - 문제: 기본 쿠키 기반 세션 사용 res.cookie('user', username)
        - 영향 체인: 인증 약점 → 계정 침해
        - 위험 수준: High
        - 솔루션 (jwt_auth에서): 적절한 만료가 있는 JWT 구현

    ```

    ```js
    저장된 패턴을 기반으로 한 보안 구현입니다:

    app.post('/api/login', async (req, res) => {
        try {
            // 입력 유효성 검사
            if (!req.body.username || !req.body.password) {
            return res.status(400).json({
                success: false,
                error: 'Username and password required'
            });
            }

            // 매개변수화된 쿼리 (SQL injection 방지)
            const query = 'SELECT * FROM users WHERE username = ?';
            const result = await db.query(query, [req.body.username]);

            if (!result.length) {
            return res.status(401).json({
                success: false,
                error: 'Invalid credentials'
            });
            }

            // 비밀번호 검증이 여기에 들어갑니다
            // const validPassword = await bcrypt.compare(req.body.password, result[0].password);

            // JWT 기반 세션 관리 (세션 하이재킹 방지)
            const token = jwt.sign(
            { userId: result[0].id },
            process.env.JWT_SECRET,
            { expiresIn: '1h' }
            );

            res.json({
            success: true,
            token: token
            });

        } catch (error) {
            res.status(500).json({
            success: false,
            error: 'Internal server error'
            });
        }
    });
   ```
   ```
    수정 사항은 지식 그래프에서 식별한 취약점 체인을 해결합니다:

        1. SQL Injection → 데이터 도난: 매개변수화된 쿼리로 완화
        2. 인증 약점 → 계정 침해: JWT 구현으로 완화

    연결된 패턴을 기반으로 한 추가 보안 개선:

        - 입력 유효성 검사 추가
        - 적절한 오류 처리
        - 내부 오류 노출 없음
        - 쿠키 기반 대신 토큰 기반 인증
        - 인증에 만료 시간

    원하시면:

        1. 이러한 보안 관계에 대해 더 자세히 설명해 드릴까요?
        2. 추가 보안 패턴 구현 방법을 보여드릴까요?
        3. 다른 엔드포인트를 분석해 드릴까요?
    ```
