# Part 1 · Ch01 실습 — Claude Code 딥다이브

> **이 챕터의 실습 목적**
> 강의를 다 보고 끝나면 "기능은 알지만 손이 안 움직이는" 상태가 됩니다. 이 챕터는 **강의가 제공하는 `example-vibe-project` repo에 Claude Code를 정착시키고**, **반복 작업 1개를 자동화하고**, **한 번의 완전한 작업 사이클(Plan→Execute→Verify)을 끝까지 돌리는** 것이 목표입니다.

**예상 소요 시간**: 약 3시간 (실습 1: 45분 · 실습 2: 60분 · 통합 실습: 90분)

**실습 대상 repo — `example-vibe-project`**
- 별도 GitHub repo: https://github.com/jha0313/example-vibe-project
- Next.js 15 App Router 단일 프로젝트 (frontend는 `app/`, backend는 `app/api/`).
- Claude Code 공식 문서(https://code.claude.com/docs/ko/overview)의 핵심 개념(에이전트 루프, CLAUDE.md 계층)을 한국어 학습자용 시각 자료로 정리한 페이지 2개를 포함.
- 의도적으로 미완성 기능과 경쟁 패턴이 심어져 있어 실습 소재로 적합.

**준비물**
- `example-vibe-project` repo를 fork 또는 clone:
   ```bash
   git clone https://github.com/jha0313/example-vibe-project.git
   cd example-vibe-project
   npm install
   npm run dev      # http://localhost:3000 — 브라우저로 확인
   ```
- Claude Code 설치 (`claude --version` 출력 확인) — 아직 안 했다면 지금이 첫 설치 타이밍.
- (선택) Claude Code Max 요금제 — 컨텍스트 많이 쓰므로 권장.

**누적 산출물** — 이 챕터를 끝낸 뒤 `example-vibe-project/` 안에 다음이 있어야 합니다:
- `CLAUDE.md` (100~200줄, 의미 있게 채워짐)
- `.claude/settings.json` (permissions + 통계라인)
- `.claude/skills/<name>/SKILL.md` **또는** hook 엔트리 1개
- `LEARNED.md` (통합 실습 회고)

---

## 🟡 실습 1 — example-vibe-project에 Claude Code 정착시키기 (45분)

**대응 클립**: 01 작동방식 · 02 컨텍스트 · 03 메모리/권한 · 04 설치/세팅 · 05 컨텍스트/권한 관리

### 무엇을 하는가
`/init`이 자동 생성하는 CLAUDE.md는 강의에서 본 대로 "Claude가 안 읽는 길고 의미 없는 문서"가 되기 쉽습니다. **100~200줄짜리 진짜 쓸모 있는 CLAUDE.md**를 손으로 만들고, permissions·statusline을 본인 환경에 맞춥니다.

### 단계

#### Step 1. CLAUDE.md 손수 작성 (20분)
강의 실습 repo(`claude-code-lecture`)의 템플릿을 작업 repo(`example-vibe-project`) 루트로 복사한 뒤 채웁니다.

```bash
# 두 repo가 같은 부모 폴더에 있다고 가정 (~/projects/claude-code-lecture, ~/projects/example-vibe-project)
cd ~/projects/example-vibe-project
cp ../claude-code-lecture/Part1-Claude_Code_마스터하기/Ch01-Claude_Code_딥다이브/exercises/templates/CLAUDE.md ./CLAUDE.md
```

5개 섹션을 example-vibe-project의 실제 정보로 채웁니다.

1. **절대 규칙** (3~5개): "이것만은 하지 마라" 형태.
2. **명령어 치트시트**: `npm run dev` · `npm run build` · `npm run lint` · `npm run typecheck` · `npm test` · `npm run format` 등.
3. **아키텍처 한눈에**: `app/` (페이지·API routes) · `components/` · `lib/` · `tests/` 폴더 + 핵심 모듈 한 줄.
4. **컨벤션**: 네이밍·테스트·에러 처리. *힌트*: 이 repo에는 의도적으로 경쟁 패턴(axios/fetch, console/logger)이 혼재 — Ch03에서 정리할 자리.
5. **TODO / 진행 중 작업**: `IDEAS.md`의 미완성 기능 중 1~2개를 그대로 옮겨 적기.

> 총 **150줄 ± 50줄**. 100줄 미만이면 부족, 250줄 초과면 잘라내기. Claude가 매 세션 처음에 읽으므로 길수록 비용 ↑.

#### Step 2. `.claude/settings.json` 작성 (10분)
permissions 화이트리스트 5개와 차단 1개를 설정.

```json
{
  "permissions": {
    "allow": [
      "Bash(npm test:*)",
      "Bash(npm run build:*)",
      "Bash(npm run lint:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)"
    ],
    "deny": [
      "Bash(git push --force:*)"
    ]
  }
}
```
> 본인 환경에 맞춰 교체. deny에 진짜 위험한 한 가지 (force push, `rm -rf`, prod DB 명령 등) 추가.

#### Step 3. `/statusline` 커스터마이징 (5분)
세션에서 `/statusline` 실행 → 다음 정보가 한 줄에 보이도록:
- 현재 git 브랜치
- 컨텍스트 사용률 (%)
- 사용 중인 모델

> `/statusline` 명령에 자연어로 "브랜치 + 컨텍스트% + 모델 보여줘" 라고 부탁하면 Claude가 셸 스크립트를 알아서 만듭니다.

#### Step 4. 컨텍스트 위생 체험 (10분)
1. 새 세션 시작.
2. example-vibe-project의 파일 3~4개를 의도적으로 읽게 합니다 (`Read app/agent-loop/page.tsx`, `Read components/LoopDiagram.tsx`, `Read lib/api/axios-client.ts`, `Read lib/api/fetch-client.ts`).
3. `/context`로 사용률 확인 — 50% 근처가 됐을 때:
   - 먼저 `/compact focus on <지금 하던 일>` 시도 → 컨텍스트 변화 관찰
4. 새 세션을 열고 같은 일을 한 뒤 이번엔 `/clear` 사용 → 차이 비교
5. 결과를 `LEARNED.md`(없으면 만들기) 맨 위에 두 문장으로 기록.

### ✅ 산출물
- `CLAUDE.md` (의미 있게 채워진 100~200줄)
- `.claude/settings.json` (allow 5개 + deny 1개)
- `/statusline` 커스터마이징 완료
- `LEARNED.md` 첫 두 문장 (compact vs clear 체험)

### 🔍 검증 신호
- **새 세션을 열고** "이 프로젝트 빌드 명령이 뭐야?" 라고 물으면 CLAUDE.md만 보고 1~2초 안에 정답.
- "이 repo의 절대 규칙 3개 알려줘" → CLAUDE.md의 절대 규칙 섹션을 그대로 요약.
- statusline에 브랜치·컨텍스트%·모델이 한 줄로 보임.

---

## 🟡 실습 2 — 반복 작업 1개를 Skill 또는 Hook으로 자동화 (60분)

**대응 클립**: 06 서브에이전트/병렬 · 07 Skill/MCP/Hook · 08 Hook/Loop/Routine

### 무엇을 하는가
**Claude를 쓰면서 반복적으로 손풀기로 해준 일** 1개를 골라 자동화합니다. MCP는 이번 실습에서 의도적으로 제외 — 강의 메시지(Skill·Hook 1순위, MCP는 예외)를 손으로 체감합니다.

### 단계

#### Step 1. 후보 찾기 (10분)
다음 세 곳에서 후보를 모읍니다.
- example-vibe-project에서 본인이 "매번 같은 명령을 친다" 싶은 셸 패턴 (`history | tail -200`)
- Claude에 매번 똑같이 친 프롬프트 (예: "이 PR 요약해줘", "diff 정리해줘")
- example-vibe-project의 PR description 같은 형식 글

**후보 3개를 적습니다**. 각각 "얼마나 반복?" "결정적인가 컨텍스트가 필요한가?" 두 컬럼.

#### Step 2. 판단 분기 (5분)
1개를 골라 아래 분기:

```
결정적 + 매번 정확히 같은 동작
  → Hook (PostToolUse / PreToolUse / Stop)
  예: 파일 저장 후 자동 prettier, 위험한 git 명령 차단

컨텍스트와 약간의 추론이 필요
  → Skill (skill-creator로 자동 생성)
  예: PR diff 보고 요약, 변경된 테스트만 실행, 커밋 메시지 작성
```

#### Step 3-A. Hook 만들기 (30분, Hook을 골랐다면)
`.claude/settings.json`의 `hooks` 섹션에 추가:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [{
          "type": "command",
          "command": "npx prettier --write \"$CLAUDE_FILE_PATHS\" 2>/dev/null || true"
        }]
      }
    ]
  }
}
```
> example-vibe-project의 실제 도구(prettier 이미 셋업됨)로 활용.

**테스트**: Claude로 파일 한 줄 수정 → 저장 후 자동 포맷 적용 확인.

#### Step 3-B. Skill 만들기 (30분, Skill을 골랐다면)
Claude 세션에서:
```
/skill-creator
```
대화형으로 다음을 답합니다:
- 스킬 이름 (kebab-case): 예 `pr-summary`
- 언제 트리거되어야 하나 (description에 반영): "PR diff를 사람이 5초 안에 읽을 요약으로 압축"
- 입력은 무엇인가 (gh CLI 출력, diff, …)
- 출력 형식 (마크다운 3섹션: 의도/변경 파일/리스크)

**테스트**: 새 세션에서 자연어로 "이번 PR 요약해줘" → 만든 Skill이 자동 호출되는지 확인.

#### Step 4. 시간 측정 (5분)
- 자동화 전: 같은 일에 평소 ___ 분
- 자동화 후: ___ 초/분
- 한 달 환산 절약: ___ 분/월 (반복 횟수 × 절약 시간)

`LEARNED.md`에 한 줄 추가.

### ✅ 산출물
- `.claude/skills/<name>/SKILL.md` **또는** `.claude/settings.json`의 hooks 엔트리
- `LEARNED.md`에 자동화 1개 시간 측정

### 🔍 검증 신호
- Hook: 트리거 조건(파일 저장 등)에서 사람 개입 없이 동작 + 콘솔에 한 줄 로그.
- Skill: 새 세션 + 다른 컨텍스트에서 자연어 한 문장으로 자동 호출됨.
- 시간 절약이 정량 (0%가 아니어야 함 — 0%면 자동화 후보가 잘못된 것).

---

## 🔴 통합 실습 — Plan → Worktree → Verify 한 사이클 완수 (90분)

**대응 클립**: 09 워크플로우 철학 (+ Ch01 전체)

### 무엇을 하는가
지금까지의 모든 것(CLAUDE.md · permissions · Skill/Hook)을 동원해 **example-vibe-project에 작은 기능 하나를 처음부터 끝까지** 만듭니다. 그냥 만드는 게 아니라 **Plan → Worktree(병렬) → Verify(독립 리뷰)** 사이클을 의식적으로 돕니다.

### 단계

#### Step 0. 작업 선정 (5분)
`example-vibe-project/IDEAS.md`에 있는 7개 후보 중 1개 선정:
- 처음 한 사이클이면 #1(호버 툴팁) 또는 #5(trackingId)가 가장 작고 명확
- Vibe vs Agentic 차이 체감을 원하면 #3(`/api/feedback` 완성)
- Ch03까지 이어 쓸 자료가 필요하면 #7(axios → fetch 마이그레이션)

#### Step 1. Plan Mode로 계획 받기 (15분)
1. `claude` 세션 시작.
2. `Shift+Tab` 두 번 → Plan Mode 진입.
3. 작업 설명 + **검증 미리 알리기** 패턴:
   ```
   IDEAS.md #<번호> 작업을 진행한다.
   - 변경 예상 파일: <목록>
   - 성공 기준: <IDEAS.md에 적힌 성공 기준 그대로 + 추가>
   - 위험: <기존 동작 회귀 / API 응답 형식 변경 / ...>
   ```
4. Claude의 계획을 받은 뒤 **4체크**:
   - [ ] 범위 적절 (필요 이상 안 건드림)
   - [ ] 기존 패턴 준수 (CLAUDE.md 컨벤션과 일치)
   - [ ] 테스트 포함 (없으면 추가 요청)
   - [ ] 리스크 명시 (없으면 보강 요청)
5. 4체크가 다 통과되면 plan 승인. 통과 안 되면 1~3번 항목을 보강해 재계획 요청.

> 📌 4체크는 강의의 핵심 습관. **계획이 한 번에 통과되는 게 정상이 아닙니다** — 2~3번 왕복하는 게 더 흔합니다.

#### Step 2. Worktree에서 구현 (30분)
1. 새 터미널 탭. example-vibe-project 디렉토리에서:
   ```bash
   claude -w
   ```
   → 자동으로 임시 워크트리 생성, Claude 세션 시작.
2. 승인된 plan을 새 세션에 붙여넣고 Auto Mode (`Shift+Tab`)로 전환.
3. 구현 진행. Claude가 막히면 **새 메시지에서** 추가 컨텍스트 (CLAUDE.md 외 다른 파일).
4. 테스트가 통과할 때까지 반복.

> 워크트리에서 작업하면 main 브랜치와 작업 디렉토리가 격리되어 중간에 본 작업장에서 다른 일을 해도 안전합니다.

#### Step 3. Verify — 독립 컨텍스트 리뷰 (25분)
1. **다른 터미널 탭**에서 example-vibe-project (워크트리 아님)로 새 `claude` 세션.
2. 다음 프롬프트:
   ```
   워크트리 <경로>의 diff를 Explore 서브에이전트로 독립 리뷰해줘.
   체크할 것:
   - CLAUDE.md의 절대 규칙 위반 없는지
   - 테스트가 진짜 성공 기준을 검증하는지 (assertion 헛돌지 않는지)
   - 위험 시나리오 (Plan에서 명시한 것) 처리되었는지
   ```
3. 리뷰 결과를 받고, 지적된 부분이 있으면 워크트리 세션에서 수정.
4. 모두 통과되면 PR 생성 (`gh pr create` 또는 강의에서 본 워크플로우).

#### Step 4. 회고 (15분)
`LEARNED.md`에 1쪽 (200~400자) 작성:
- **어디서 막혔나** (Plan? Implementation? Review?)
- **AI가 가장 잘한 부분 / 약한 부분**
- **다음 번에 다르게 할 한 가지**

### ✅ 산출물
- PR 1개 (merge는 본인 판단)
- `LEARNED.md` 회고 1쪽

### 🔍 검증 신호
- 워크트리·메인·리뷰 세션 **세 가지가 동시에 살아 있던** 경험
- 4체크에서 한 번이라도 plan을 되돌려보낸 경험 (한 번에 통과는 의심스러움)
- 독립 리뷰가 **실제로 무언가 잡아냄** (Style 코멘트 말고, 진짜 버그/누락 1개)

---

## 챕터 회고 (선택, 10분)

`LEARNED.md` 맨 아래:
> "Ch01을 마쳤다. example-vibe-project에 남은 자산: ___. 가장 큰 깨달음 한 문장: ___"

---

## 다음 챕터
**Part 1 · Ch02** — 90분 Vibe Coding 챌린지. example-vibe-project는 잠시 두고, 빈 디렉토리에서 **CLAUDE.md / Skill / Hook 전부 없이** 무언가 만들어봅니다. 그 결과물은 Ch03(Agentic Engineering)에서 다시 살립니다.
