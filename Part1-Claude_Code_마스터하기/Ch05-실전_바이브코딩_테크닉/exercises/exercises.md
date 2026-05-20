# Part 1 · Ch05 — 실전 바이브코딩 테크닉 실습

> **이 챕터의 실습은 현재 집필 중입니다.** Ch04 (Harness) 까지 5계층을 갖춘 본인 repo를 **프로덕션 품질**로 끌어올리는 마지막 챕터입니다. 아래는 챕터의 흐름과 **공개 예정인 실습 구성**의 미리보기입니다.

---

## 챕터에서 다룰 핵심

| 클러스터 | 주요 클립 | 한 줄 요약 |
|---|---|---|
| **A. CLAUDE.md 프로덕션화** | 01 작성전략 · 02 템플릿 | "코드에 없는 것"만 쓴다. Overview/Quick Start/Architecture/Do NOT/Task Guides |
| **B. AI-Ready 코드베이스** | 03 5가지 조건 · 04 체크리스트 | 디렉토리 · 네이밍 · 결합도 · 타입 · 파일 크기 |
| **C. 비용·보안 안전망** | 05 컨텍스트 비용 · 06 API키 · 07 Git 안전망 | `/clear`·`/compact` 타이밍, 시크릿 스캔, 보호 브랜치·훅 |
| **D. TDD / SDD** | 08 TDD · 09 SDD · 10 선택기준 | Red→Green→Refactor를 Claude와. 스펙 우선 vs 테스트 우선의 판단표 |
| **E. AI 코드리뷰** | 11 AI 리뷰 · 12 리뷰기준 · 13 사람vsAI | PR 리뷰 자동화 + 사람이 봐야 할 것 분리 |
| **F. Legacy 마이그레이션** | 14 Legacy Migration | 기존 코드베이스를 Agent-Ready로 단계적 이식 |

---

## 공개 예정 실습 (구성 미리보기)

### 🟡 실습 1: "프로덕션 CLAUDE.md v2 + AI-Ready 체크리스트" (클러스터 A+B, ~60분)
- Ch01에서 만든 CLAUDE.md를 **프로덕션 템플릿**(Overview / Quick Start / Architecture / Key Decisions / Do NOT / Task Guides)으로 재구성
- AI-Ready 5조건 체크리스트로 본인 repo 자가진단 (디렉토리 · 네이밍 · 결합도 · 타입 · 파일 크기)
- 가장 점수 낮은 1개 항목을 PR로 개선
- ✅ `CLAUDE.md` v2 + `ai-ready-checklist.md` 진단표 + 개선 PR 1건
- 🔍 새 세션에서 "X 추가하려면 어떤 파일을 봐야 해?"에 Claude가 Task Guides 섹션을 인용

### 🟡 실습 2: "비용·보안·Git 3중 안전망" (클러스터 C, ~60분)
- 컨텍스트 모니터링 루틴 정착: `/compact` vs `/clear` vs `/rewind` 사용 시점을 본인 패턴으로 문서화
- 시크릿 스캔: `git-secrets` 또는 `trufflehog` 1회 실행 → 의심 문자열 0건 또는 화이트리스트 명문화
- Git 안전망: 보호 브랜치 + pre-push 훅 1개(`npm test` 등) + force-push 차단
- ✅ `cost-and-safety.md` (3 안전망 적용 결과) + `.git/hooks/pre-push` 또는 동등 CI 단계
- 🔍 의도된 위험 푸시(force-push, 시크릿 포함 커밋)가 실제로 차단됨

### 🟡 실습 3: "TDD 또는 SDD 한 사이클" (클러스터 D, ~75분)
- 본인 repo의 새 기능 1개를 **선택기준 표**에 따라 TDD 또는 SDD로 진행
- TDD: Red(실패 테스트 작성) → Green(Claude가 통과시킴) → Refactor 3바퀴
- SDD: 스펙 1쪽 작성 → Claude가 스펙→테스트→구현 순으로 생성 → 사람이 스펙 vs 구현 diff 검토
- ✅ 선택 근거 + 1 사이클의 PR (Test 또는 Spec + 코드)
- 🔍 동료가 PR을 보고 "어떤 방식으로 만든 건지" 추측 가능 (테스트 또는 스펙 트리거가 명확)

### 🔴 통합 실습: "Legacy 모듈 1개를 Agent-Ready로 마이그레이션" (클러스터 E+F, ~120분)
- 본인 repo(또는 회사 repo 일부)에서 "Agent에게 맡기기 어려운" 레거시 모듈 1개 식별
- 4단계 마이그레이션: (1) 매핑 — 파일·의존성·암묵 지식 추출, (2) Documentation — Co-located 문서·타입 보강, (3) Refactor — 결합도·파일 크기 개선, (4) AI 리뷰 — Claude로 PR 리뷰 + 사람 리뷰 동시 진행 후 의견 비교
- ✅ Before/After 모듈 diff + `migration-log.md` + AI vs 사람 리뷰 비교 1쪽
- 🔍 같은 모듈에 대해 Claude의 변경 제안 variance가 마이그레이션 후 명백히 감소 (정성 기록 OK)

---

## 이 챕터를 마치면 학습자 repo에 추가로 쌓이는 것

- `CLAUDE.md` v2 (프로덕션 템플릿) + `ai-ready-checklist.md`
- `cost-and-safety.md` + Git 안전망 (보호 브랜치 / pre-push 훅 / 시크릿 스캔)
- TDD 또는 SDD 사이클 1건 (테스트 또는 스펙 문서)
- AI 코드리뷰 워크플로우 1개 (PR 봇 또는 수동 Claude 리뷰 스킬)
- Legacy 마이그레이션 사례 1건 + `migration-log.md`

---

## 잠시 기다리는 동안 할 수 있는 것

1. **작업 repo(또는 회사 repo)의 가장 "AI에게 맡기기 무서운" 모듈 1개 식별** — Ch05 통합 실습의 입력값이 됩니다.
2. **현재 CLAUDE.md를 다시 펼쳐서** Overview/Architecture/Do NOT/Task Guides 4개 섹션이 모두 있는지 점검. 없다면 비어 있는 채로 헤더만 추가해두세요.
3. **Notion 치트시트 페이지** 의 Ch05 항목에서 TDD vs SDD 선택기준 표를 미리 훑어두세요.

> 이번 챕터 실습이 공개되면 **`exercises/` 디렉토리에 `CLAUDE.md.v2.template`, `ai-ready-checklist.md.template`, `migration-log.md.template`이 함께 추가**됩니다. Ch01~Ch04와 동일하게 본인 repo에 누적 적용하는 형태이며, 정답 코드는 제공되지 않습니다.
