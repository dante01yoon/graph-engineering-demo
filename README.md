# graph-engineering-demo

유튜브 영상 「그래프 엔지니어링, Claude Code로 직접 해 봤습니다」(단테, 2026-10-09)에서 쓴 실습 파일과 실행 기록입니다.

## 그래프 엔지니어링 한 줄 요약
AI 에이전트 여러 개를 **점(노드)과 선(엣지)** 으로 엮어 일을 나눠 맡기는 설계입니다. 점은 한 가지 일만 하는 에이전트(코드·테스트·사람 승인도 될 수 있음), 선은 결과를 넘기는 순서입니다. 에이전트 하나가 혼자 반복하는 루프는 "점 하나짜리 그래프"로 볼 수 있습니다.

## bakery/ — Claude Code 서브에이전트로 만든 그래프
질문: "동네 빵집이 배달앱 대신 자체 예약 주문 페이지를 열어야 할까? (서울 기준)"

```
researcher ×3 (고객 수요 · 배달앱 수수료·경쟁 · 구축·운영 비용)  →  skeptic (근거 약한 주장 걸러내기)  →  writer (결론)
```

- 점: `.claude/agents/researcher.md`, `skeptic.md`, `writer.md` (frontmatter 필수는 `name`·`description`, `tools`·`model`은 선택)
- 선: 메인 Claude에게 순서를 말로 지시 (아래 프롬프트). 순서를 코드로 고정하려면 Claude Code의 다이내믹 워크플로를 씁니다.

```bash
cd bakery
claude
```

프롬프트:

> 동네 빵집이 배달앱 대신 자체 예약 주문 페이지를 열어야 할까? 서울 기준으로 판단해 줘. 이렇게 진행해: researcher 서브에이전트 3개를 동시에 띄워서 각각 고객 수요, 배달앱 수수료와 경쟁, 구축·운영 비용만 조사하게 하고, 세 결과를 skeptic에게 넘겨 근거 약한 주장을 걸러낸 다음, 통과한 주장만 writer에게 넘겨 결론을 쓰게 해.

## runs/ — 실제 실행 기록 (2026-10-09, Claude Code 2.1.293)
- `graph-final.md`: 녹화한 그래프 실행의 최종 답 (주장 37개 → 25개 탈락 → 조건부 통과 12개)
- `graph-nodes.csv`: 녹화 실행의 노드별 시작·소요 시간·토큰
- `runs-compare.csv`: 별도 측정 각 1회 (headless) — 그래프 407.1초·$1.92 vs 에이전트 하나 57.6초·$0.28

숫자는 한 번씩 돌린 결과라 실행마다 달라질 수 있습니다. 결론에 나온 수수료 등은 출처 날짜 기준이니 다시 확인하세요.

## explainer/ — HyperFrames로 만든 15초 노드 애니메이션
```bash
npx skills add heygen-com/hyperframes --agent claude-code -y
claude   # /hyperframes ... 로 요청 → graph.mp4 렌더
```
`index.html`이 Claude Code가 만든 컴포지션, `graph.mp4`가 결과입니다. HyperFrames: https://github.com/heygen-com/hyperframes (Apache-2.0)

## 출처
- Anthropic, Building effective agents: https://www.anthropic.com/engineering/building-effective-agents
- Anthropic, How we built our multi-agent research system: https://www.anthropic.com/engineering/multi-agent-research-system
- Claude Code subagents: https://code.claude.com/docs/en/sub-agents · workflows: https://code.claude.com/docs/en/workflows
- LangChain, 3 years of graph engineering with LangGraph: https://www.langchain.com/blog/3-years-of-graph-engineering-with-langgraph
