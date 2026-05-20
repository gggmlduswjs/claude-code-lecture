# Part 1 · Ch03 실습 — Agentic Engineering 개론

> **이 챕터의 실습 목적**
> 5 Pillars(Context · Validation · Tooling · Codebases · Compound)를 머리로만 이해하면 다음 주에 잊혀집니다. 이 챕터는 **Ch02에서 90분간 바이브코딩한 그 결과물**(또는 본인 작업 repo) 위에 **CLAUDE.md를 계층화하고**, **검증 루프 1개와 friction 자동화 1개를 더하고**, 마지막으로 그 위에 **여러 Agentic 패턴을 한 사이클** 돌리는 것이 목표입니다.

**예상 소요 시간**: 약 3시간 (실습 1: 45분 · 실습 2: 60분 · 통합 실습: 90분)

**작업 대상 repo (둘 중 하나 선택)**
- A. **Ch02의 `~/projects/vibe-90min/`** — Vibe로 만든 결과물 위에 Agentic을 입혀 BEFORE/AFTER 체감 (추천)
- B. 본인 회사·사이드 repo — 더 실전 같음. 단, Track 1 영역에서만 작업 (외부 사용자·결제·PII 없는 곳)

**누적 산출물** — 이 챕터를 끝낸 뒤 대상 repo에 추가될 것:
- `CLAUDE.md` (계층화된, 80~150줄)
- `.claude/docs/` 디렉토리 (도메인별 deep dive 3~5개)
- `.agent-rules.md` (50% 룰 적발 + 가드레일)
- 검증 루프 1개 (claude hook 또는 husky pre-commit)
- friction 자동화 1개 (Skill 또는 hook)
- `LEARNED.md` (회고)

---

## 🟡 실습 1 — CLAUDE.md 계층화 + @참조 트리 (45분)

**대응 클립**: 01 Paradigm Shift · 02 Pillar I Context Engineering

### 무엇을 하는가
Ch02에서 90분간 만든 repo는 CLAUDE.md조차 없습니다. 본격적인 작업을 시작하기 전, **루트 CLAUDE.md를 entry point로 두고**, 도메인별 deep dive는 **`.claude/docs/<area>.md`** 로 분리하는 구조를 손으로 만듭니다.

### 단계

#### Step 1. 루트 CLAUDE.md 작성 (20분)
강의 실습 repo(`claude-code-lecture`)의 `Part1-Claude_Code_마스터하기/Ch03-Agentic_Engineering_개론/exercises/templates/CLAUDE.md` 를 작업 repo 루트에 복사한 뒤, 5개 섹션을 본인 코드 정보로 채웁니다.

```bash
cp ~/projects/claude-code-lecture/Part1-Claude_Code_마스터하기/Ch03-Agentic_Engineering_개론/exercises/templates/CLAUDE.md ./CLAUDE.md
```

1. **절대 규칙** (3~5개): "이것만은 하지 마라" 형태.
2. **명령어 치트시트**: 빌드/테스트/lint/타입체크 명령 (없으면 추가).
3. **아키텍처 한눈에**: 폴더 구조 + 핵심 모듈 한 줄 설명.
4. **컨벤션**: 네이밍·테스트·에러 처리 패턴.
5. **TODO / 진행 중 작업**: 지금 작업 중인 기능 1~2개.

> 총 **80~150줄**. Vibe로 만든 작은 repo면 더 짧아도 OK — 핵심은 *"코드를 읽으면 알 수 있는 정보는 빼는 것"*.

#### Step 2. `.claude/docs/` 디렉토리 만들기 (15분)

도메인별 deep dive 파일을 분리합니다.

```
.claude/docs/
├── architecture.md      # 아키텍처 deep dive (다이어그램·결정·트레이드오프)
├── decisions.md         # 핵심 결정 3개의 "왜"
└── testing.md           # 테스트 전략·관례
```

> Vibe로 만든 작은 repo면 2~3개로 충분. 본인 회사 repo면 도메인별로 3~5개.

`decisions.md`에 최소 **3개의 결정**을 *Architectural Rigor* 형식으로 기록:
```markdown
## D1. Server Component 우선 (Next.js 15 App Router)

**언제 결정**: 2026-04-15
**선택**: Server Components를 기본으로, "use client"는 인터랙션 필요 시만
**왜**: SEO + 초기 로드 + 번들 크기.
**버린 대안**: Pages Router 마이그레이션.
**미래 재검토**: React 19 안정화 이후 Client Component 비율 30% 이내 유지.
```

#### Step 3. 루트 CLAUDE.md를 entry point로 슬림화 (15분)
도메인 deep dive를 옮긴 뒤, 루트 CLAUDE.md 하단에 `@` 참조를 추가:

```markdown
## 참고 자료

- 아키텍처 deep dive: @.claude/docs/architecture.md
- 핵심 결정: @.claude/docs/decisions.md
- 테스트 전략: @.claude/docs/testing.md
```

> `@`로 참조된 파일은 매 세션 시작 시 **함께 로드**됩니다 (eager). 매 세션 비용을 절감하려면 정말 필요한 docs만 `@`로 묶고, 나머지는 일반 경로(`.claude/docs/X.md`)로 두어 Claude가 필요 시 Read하게 두세요.

루트 CLAUDE.md 목표 길이 = **80~150줄**.

### ✅ 산출물
- `.claude/docs/` 디렉토리 (2~5개 파일)
- 슬림한 루트 `CLAUDE.md` (80~150줄)
- `.claude/docs/decisions.md`에 3개의 결정 기록

### 🔍 검증 신호
- 새 세션에서 "이 repo 빌드 명령이 뭐야?" → CLAUDE.md만 보고 1~2초 안에 정답.
- "핵심 결정 3개" → `decisions.md` 내용을 그대로 요약.

---

## 🟡 실습 2 — Validation 루프 1개 + Friction 자동화 1개 (60분)

**대응 클립**: 03 Pillar II Agentic Validation · 04 Pillar III Agentic Tooling

### 무엇을 하는가
Agent가 자기 결과를 **스스로 검증**할 수 있는 신호 1개를 작업 repo에 박습니다 (closed-loop). 그리고 강의의 *"Build a Tool, Don't Do the Task"* 원칙대로, "Claude에 매번 손풀기로 부탁한 일" 1개를 도구로 격상합니다.

### 단계

#### Step 1 — Validation 루프 추가 (30분)

작업 repo에서 **핵심 기능 1개** 선택 (Vibe repo면 만든 결과물 그 자체, 회사 repo면 어떤 핵심 함수든). 다음 둘 중 **하나** 선택해 매 commit마다 검증이 자동으로 돌게 만듭니다.

**A. Claude Code Hook으로 매 commit 후 테스트 자동 실행** (권장)

`.claude/settings.json`:
```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Bash(git commit*)",
        "hooks": [{
          "type": "command",
          "command": "npm test 2>&1 | tail -20"
        }]
      }
    ]
  }
}
```
→ Claude가 git commit을 실행하면 즉시 테스트가 돌고, 실패 출력이 다시 컨텍스트로 들어와 자기 수정 루프가 가능해집니다.

**B. Husky pre-commit hook으로 git commit 시점에 강제 검증**

```bash
npm install --save-dev husky
npx husky init
echo "npm test" > .husky/pre-commit
```
→ Claude가 만들든 사람이 만들든 모든 commit이 테스트를 통과한 것만 들어갑니다.

> A는 *agent의 self-correction 루프* 강화, B는 *결과물의 baseline 보장*. Vibe repo에서 처음 도입하는 거면 **B 먼저** 해보세요 — 테스트 0개라도 husky 셋업해놓고 1~2개 테스트를 채우면 효과 즉시.

**CLAUDE.md (또는 `.claude/docs/testing.md`)** 에 다음 명문화:

```markdown
## 검증 신호

- 매 commit 전 `npm test` 통과해야 함 (husky pre-commit으로 강제됨)
- 테스트 실패 출력이 보이면 → 실패 케이스 읽고 코드 수정 → 다시 commit. 최대 3회. 3회 후에도 실패하면 사람에게 보고.
```

#### Step 2 — Friction 자동화 1개 추가 (25분)

"매번 사람이 똑같이 해주고 있는 일"을 도구로 격상합니다. 다음 후보 중 하나 선택:

**후보 1 — Vercel 셋업: UI 클릭 vs MCP 위임**
- 비교: 본인이 Vercel UI에서 직접 클릭으로 deploy 셋업 vs **Vercel MCP**를 Claude에 붙여서 "이 repo Vercel에 배포 셋업해줘"라고 자연어로 위임.
- 후자가 매번 가능해지면 다음 새 프로젝트 셋업 시 친구 친화적.

**후보 2 — Code Review: 매번 부탁 vs 자동 hook**
- 비교: 매번 `git diff`를 보고 "리뷰해줘" 라고 부탁 vs **PreToolUse 또는 Stop hook**으로 `Bash(git commit*)` 직전에 Claude가 diff를 자동 리뷰한 결과를 보여주는 흐름.
- 후자는 commit 시점에 한 번 더 검수 — *"리뷰해줘"라는 말을 영구히 안 해도 됨*.

**후보 3 — PR description 초안: 매번 손풀기 vs Skill**
- 비교: 매 PR마다 "이 PR description 초안 작성해줘"를 손으로 입력 vs `pr-description-draft` Skill 만들고 자연어 한 문장에 트리거.

> Ch01 실습 2에서 이미 1개 만들었다면, **이번 건 다른 종류** 작업이어야 합니다 (같은 일 두 번 자동화 ❌).

#### Step 3 — 체감으로 비교 (5분)

자동화 전후를 **정확한 시간 측정 없이** 체감으로만 비교. `LEARNED.md`에 한 줄씩:
- Validation 추가 후 같은 작업을 시켜봤더니, 사람 개입 횟수가 (줄었다 / 비슷하다)
- 자동화 후 같은 일을 다시 시켜봤더니, "이게 손풀기 안 들어가는 게 신기"한 부분이 있었다 / 없었다

> 정량 측정은 챕터 흐름을 끊습니다. *체감*이 더 정직한 신호.

### ✅ 산출물
- 검증 루프 1개 (claude hook 또는 husky pre-commit) + CLAUDE.md/docs 명문화
- 자동화 도구 1개 (Skill, hook, 또는 MCP 연결)
- `LEARNED.md` 체감 비교 한두 줄

### 🔍 검증 신호
- 같은 작업을 새 세션에서 시켰을 때 Claude가 **친절한 설명 없이 도구를 자기 호출**해서 자기 검증
- 자동화 후 같은 일을 다시 시켰을 때 사람의 "매번 같은 손풀기" 입력이 사라짐

---

## 🔴 통합 실습 — 작업 repo에 Agentic Engineering 입히기 (90분)

**대응 클립**: 05 Pillar IV Codebases · 06 Pillar V Compound · 07 통합 사례

### 무엇을 하는가
실습 1·2에서 쌓인 자산(CLAUDE.md 계층화 + 검증 루프 + 자동화 1개) 위에, **나머지 Pillar들을 한 번에 적용**해 작업 repo를 본격적으로 Agentic하게 만듭니다. 마지막에는 같은 종류의 요청을 다시 시켜보고, Ch02 BEFORE 대비 무엇이 달라졌는지 한 페이지 회고.

### 단계

#### Step 1. `.agent-rules.md` 작성 — 50% 룰 적발 (30분)

같은 일을 하는 경쟁 패턴이 코드에 두 개 이상 공존하면 agent가 50% 확률로 잘못된 쪽을 선택합니다. 작업 repo에서 그런 패턴을 1~3개 적발하고, 한 쪽을 DEPRECATED로 표시합니다.

```bash
cp ~/projects/claude-code-lecture/Part1-Claude_Code_마스터하기/Ch03-Agentic_Engineering_개론/exercises/templates/.agent-rules.md ./.agent-rules.md
```

본인 repo에 맞춰 채워 넣기:
- **50% 룰 적발**: 같은 일을 하는 경쟁 패턴 1~3개. 예: "HTTP 클라이언트가 `fetch`와 `axios` 두 가지로 혼재 → `lib/http.ts`의 fetch 래퍼만 사용. axios는 DEPRECATED."
- **에이전트별 가드레일**: Sub-agent로 위임할 수 있는 일 / 위임 못 하는 일.
- **구조화 로그 포맷**: JSON line 한 줄 예시.

> Vibe repo는 90분짜리라 패턴이 많지 않을 수 있습니다. 그러면 "지금 적발할 패턴은 없지만 다음에 생기면 여기 적는다" 같은 **선언만** 해두는 것도 OK — 이게 *Compound* 정신 (자산을 미리 자리 잡아두기).

#### Step 2. 구조화 로그 1개 + Sub-agent 위임 1개 (30분)

**구조화 로그**: 작업 repo에서 가장 자주 디버깅하게 될 핵심 함수 1개에 JSON line 로그 추가.
```typescript
logger.info({ ts: new Date().toISOString(), module: 'core', event: 'process_input', input: x })
```
→ jq로 파싱 가능한 형식 (Pillar IV의 self-correction 입력).

**Sub-agent 위임 1번**:
Claude 세션에서 다음을 시도:
```
Explore agent로 이 repo의 모든 TODO/FIXME 주석을 찾아서 카테고리별로 묶어줘.
```
또는:
```
독립 리뷰 서브에이전트한테 직전 commit diff를 리뷰시켜줘.
```
→ 메인 컨텍스트 손상 없이 큰 출력을 흡수하는 패턴 체감.

#### Step 3. 같은 요청 다시 시켜보기 — Ch02 BEFORE와 체감 비교 (15분)

Ch02 90분 챌린지 동안 가장 답답했던 순간(`vibe-notes.md`의 질문 3 답변)을 떠올린 뒤, **유사한 요청을 지금 작업 repo에서 새 세션으로** 다시 시켜봅니다.

`LEARNED.md`에 한 문단:
- Ch02 BEFORE 한 줄 요약: ___
- 지금 (AFTER) 한 줄 요약: ___
- 가장 큰 차이 한 가지: ___ (예: "Claude가 `@.claude/docs/architecture.md`를 자발적으로 읽고 패턴을 그대로 따랐다")

#### Step 4. 다음 90일 cadence 한 줄 (15분)

`LEARNED.md` 마지막에 *Compound Engineering* 정신으로 한 문장:
- "다음 90일 동안 이 repo에 매주 추가할 1자산 종류: ___" (예: "매주 1개 결정을 `decisions.md`에 기록")
- 첫 주의 자산: ___ (구체적인 한 가지)

### ✅ 산출물
- `.agent-rules.md` (50% 룰 + 가드레일 + 로그 포맷)
- 핵심 함수 1개에 구조화 로그 적용
- Sub-agent 위임 시도 1회 + 결과 한 줄 메모
- `LEARNED.md` — Ch02 BEFORE vs 지금, 다음 90일 cadence

### 🔍 검증 신호
- 새 세션에서 같은 종류의 요청을 시켰을 때 Claude가 **이름으로 파일 경로를 정확히** 가리킴 (Ch02에서는 "어딘가에 있다"였더라도 지금은 `src/lib/X.ts`처럼 구체)
- `.agent-rules.md`의 50% 룰이 한 줄이라도 명문화됨 (없으면 "지금은 없지만 다음에 X 패턴이 보이면 여기 적는다" 선언만이라도)
- 다음 90일 cadence가 추상적이지 않고 **자산 종류 + 빈도**가 명시됨

---

## 챕터 회고 (선택, 10분)

`LEARNED.md` 맨 아래:
> "Ch03을 마쳤다. 작업 repo가 Ch02 → 지금으로 가장 달라진 단 한 가지: ___."

---

## Part 1 통합 회고 (선택, 10분 — Ch01·02·03 누적)

작업 repo에 다음이 **모두** 쌓였는지 체크:
- [ ] `CLAUDE.md` (계층화, 80~150줄)
- [ ] `.claude/docs/` (2~5개 도메인 문서)
- [ ] `.claude/settings.json` (permissions + hook)
- [ ] `.agent-rules.md`
- [ ] 검증 루프 1개 (claude hook 또는 husky)
- [ ] 자동화 1개 이상 (Skill / hook / MCP)
- [ ] 구조화 로그 1개
- [ ] `LEARNED.md`

> 6개 이상이면 Part 1 통과. 다음 챕터(Ch04 Harness Engineering)에서 이 자산들이 **본격적 시스템**으로 격상됩니다.

---

## 다음 챕터
**Part 1 · Ch04** — Harness Engineering 개론. 지금까지 만든 CLAUDE.md / Rules / Hooks를 팀 공유 자산으로 격상하고, 영구한 가드레일 시스템을 설계합니다.
