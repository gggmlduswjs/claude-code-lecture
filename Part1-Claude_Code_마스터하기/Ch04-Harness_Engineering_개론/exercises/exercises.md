# Part 1 · Ch04 — Harness Engineering 개론 실습

> **이 챕터의 실습은 현재 집필 중입니다.** Ch01~Ch03 (Vibe → Agentic) 의 학습을 마친 뒤, 본격적인 Harness 도입 단계로 넘어가는 챕터입니다. 아래는 챕터의 흐름과 **공개 예정인 실습 구성**의 미리보기입니다.

---

## 챕터에서 다룰 핵심

| 클러스터 | 주요 클립 | 한 줄 요약 |
|---|---|---|
| **A. Harness란 무엇인가** | 01 OS 없는 CPU · 02 4축 12요소 | Agent = Model + Harness. 모델 밖으로 꺼낼 것들. |
| **B. Claude Code 5계층** | 03 5계층 · 04 Before/After 데모 | CLAUDE.md → Skills → Hooks → Subagents → Permissions |
| **C. 패턴과 안티패턴** | 05 12 패턴 · 06 안티패턴 | 실전 패턴 모음 + 도구 폭증 / React 안티패턴 / 과허용 |
| **D. 평가·안전·점진 도입** | 07 평가/안전 · 08 미니실습 | Gen-Eval, Allow/Ask/Deny, 5계층 점진 도입 로드맵 |

---

## 공개 예정 실습 (구성 미리보기)

### 🟡 실습 1: "작업 repo의 4축 매핑" (클러스터 A, ~45분)
- Ch01~Ch03에서 쌓인 자산(`CLAUDE.md` · `.claude/docs/` · `.claude/settings.json` · `.agent-rules.md` · 검증 루프)을 **4축(Memory / Skills / Protocols / Mediating Mechanisms)** 격자에 배치
- 비어 있는 칸 1개 식별 → "왜 비어 있나" 1단락
- ✅ `harness-map.md` (4축 격자) + 보완 우선순위 표
- 🔍 동료에게 격자만 보여줘도 "이 repo는 Skill이 약하다"처럼 약점이 한눈에 읽힘

### 🟡 실습 2: "5계층 Before/After 비교 데모" (클러스터 B, ~60분)
- Ch02 Vibe repo 시점(아무것도 없던 baseline) vs 현재 Ch03 끝나는 시점 — 5계층별로 어떤 것이 들어왔는지 표로 정리
- 비어 있는 계층 1개를 새로 추가 (예: Subagent 또는 Permissions 화이트리스트)
- 같은 프롬프트 3개를 Before(원시) / After(5계층) 두 환경에서 각각 실행, 차이 기록
- ✅ `before-after-harness.md` (5계층 표 + 프롬프트 결과 차이)
- 🔍 5계층 중 최소 4개에 본인 repo 항목이 채워짐

### 🔴 통합 실습: "12 패턴 중 2개 + 안티패턴 진단" (클러스터 C+D, ~90분)
- 실전 12 패턴 중 작업 repo에 가장 부족한 2개를 골라 적용 (예: 구조화 로그 + 검증 자동화)
- 안티패턴 3종(도구 폭증 / React 패턴 남용 / 과허용 권한) 자가진단 → 적발 1건 이상 시 PR로 수정
- Gen-Eval 한 사이클: 작업 repo의 핵심 기능 1개에 대해 "생성 → 자동 평가" 루프 1개 구축
- ✅ PR 1개 + `safety-audit.md` (안티패턴 진단 + 권한 정책 변경 diff)
- 🔍 새 세션에서 위험 명령(예: `rm -rf`, `git push --force`)이 Deny에 걸리고, Skill/Hook이 의도대로 발화

---

## 이 챕터를 마치면 학습자 repo에 추가로 쌓이는 것

- `harness-map.md` — 4축 격자 (현재 상태 + 보완 계획)
- `before-after-harness.md` — 5계층 적용 전후 비교
- `.claude/settings.json` 권한 정책 업데이트 (Allow/Ask/Deny 3단)
- Gen-Eval 루프 1개 (`eval.sh` 또는 동등 CI 단계)
- `safety-audit.md` — 안티패턴 진단 결과

---

## 잠시 기다리는 동안 할 수 있는 것

1. **Ch03 통합 실습의 산출물 정리** — `CLAUDE.md` 계층화, `.agent-rules.md`, 검증 루프 결과를 Ch04에서 5계층 격자로 재해석할 때 입력값으로 쓰입니다.
2. **본인 작업 repo의 `.claude/settings.json` 권한 정책 점검** — 현재 Allow가 너무 넓진 않은지, Deny에 들어가야 할 위험 명령이 빠지지 않았는지 살펴보세요.
3. **Notion 치트시트 페이지** 의 Ch04 항목에서 5계층 표를 미리 훑어두면 본 영상 시청 속도가 빨라집니다.

> 이번 챕터 실습이 공개되면 **`exercises/` 디렉토리에 `harness-map.md.template`, `safety-audit.md.template`이 함께 추가**됩니다. Ch01~Ch03와 동일하게 본인 repo에 누적 적용하는 형태이며, 정답 코드는 제공되지 않습니다.
