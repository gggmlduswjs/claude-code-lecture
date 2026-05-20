# CLAUDE.md — <프로젝트 이름>

> 이 문서는 매 Claude 세션 시작 시 자동으로 로드됩니다.
> **목표 길이**: 100~200줄. 200줄을 넘으면 잘라낼 것을 우선 고려하세요.
> **원칙**: Claude가 코드를 읽으면 알 수 있는 정보는 적지 마세요 (예: 함수 시그니처 X, 인터페이스 정의 X).

마지막 업데이트: YYYY-MM-DD

---

## 1. 절대 규칙 (3~5개)

- 절대 X 하지 마라 — (예: `console.log`를 production 코드에 두지 마라. 로깅은 `logger.ts`만 사용)
- 절대 Y 하지 마라 — (예: `process.env`를 직접 읽지 마라. `config/env.ts`를 통해서만)
- 절대 Z 하지 마라 — (예: 새 의존성 추가 전에 `package.json`의 deny list 확인)

> 절대 규칙 = 위반 시 PR 거부될 만한 것만. 3~5개를 넘으면 "절대"가 아닙니다.

---

## 2. 명령어 치트시트

```bash
# 빌드 / 개발 서버
npm run dev
npm run build

# 테스트
npm test                    # 전체
npm test -- --watch         # watch 모드
npm test path/to/file       # 단일 파일

# Lint / 타입체크
npm run lint
npm run typecheck

# DB / 인프라 (있을 경우)
npm run db:migrate
npm run db:reset
```

> 강의 권장: 빌드·테스트·lint·typecheck 4가지는 반드시. DB/배포는 있는 경우만.

---

## 3. 아키텍처 한눈에

```
src/
  app/         # Next.js App Router (라우트 단위)
  components/  # 재사용 React 컴포넌트
  lib/         # 도메인 로직 (auth, payment, ...)
  utils/       # 순수 유틸 함수
  styles/      # 글로벌 CSS / Tailwind config
tests/         # 모든 테스트 (mocks/ 포함)
```

**핵심 모듈 3~5개**:
- `lib/auth/` — 인증·세션 관리. `session.ts`가 진입점.
- `lib/payment/` — 결제. Stripe webhook은 `stripe/webhook.ts`에서만 처리.
- `lib/db/` — Prisma 래퍼. 직접 `prisma.X`를 부르지 말고 `lib/db/queries/`만 사용.

---

## 4. 컨벤션

### 네이밍
- 파일: `kebab-case.ts`
- 컴포넌트: `PascalCase.tsx`
- hook: `useXxx.ts`
- 테스트: `<원본>.test.ts`

### 테스트
- 단위 테스트는 mocking OK
- 통합 테스트는 실제 DB (sqlite in-memory) — mocking 금지
- 모든 테스트는 `tests/` 아래, 절대 `src/` 안에 두지 않음

### 에러 처리
- API 라우트는 `lib/errors.ts`의 `AppError` 던지기
- catch 블록에서 무의미한 `console.log` 금지 — `logger.error`만

---

## 5. 지금 진행 중 (TODO)

- [ ] 인증 미들웨어를 Edge Runtime로 이전 (브랜치: `feat/edge-auth`)
- [ ] Stripe 웹훅 idempotency 키 처리 (이슈 #123)

> Claude에게 컨텍스트를 주는 항목. 작업이 끝나면 즉시 제거.
> 길어지면 `.claude/docs/todos.md`로 분리하고 여기서는 한 줄 참조만.

---

## 6. 참고 자료 (선택)

- 상세 아키텍처 도큐먼트: `@.claude/docs/architecture.md`
- 결제 모듈 deep dive: `@.claude/docs/payment.md`
- 온보딩 가이드: `@docs/onboarding.md`

> `@` 참조는 Claude가 필요할 때만 로드합니다 (lazy load). 이것이 CLAUDE.md를 짧게 유지하는 비결.
