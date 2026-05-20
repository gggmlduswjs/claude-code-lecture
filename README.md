# claude-code-lecture

> **"실리콘밸리 엔지니어의 Claude Code: 바이브 코딩 & 에이전틱 워크플로우 실전 로드맵"** 강의 실습 repo.

이 repo는 강의의 **모든 실습 가이드와 템플릿 파일**을 담고 있습니다. 학습자는 이 repo를 fork해서 본인 작업 공간으로 사용합니다.

## 빠른 시작

1. 우측 상단 **Fork** 버튼으로 본인 계정에 복제.
2. fork한 repo를 clone:
   ```bash
   git clone https://github.com/<your-username>/claude-code-lecture.git
   cd claude-code-lecture
   ```
3. 첫 챕터로 이동:
   ```bash
   cd part0-intro/ch01-instructor-intro  # 또는 한글 폴더명: Part0-실리콘밸리_엔지니어의_하루/Ch01-강사소개_강의개요
   ```
   *(현재 폴더 구조는 한글 기반입니다. 영문 단축 구조로의 마이그레이션은 진행 중.)*
4. `exercises/exercises.md`를 읽고, `exercises/templates/` 안의 빈 양식을 본인 답으로 채워 commit.

## 구조

```
claude-code-lecture/
├── index.html                                          # 학습 자료 허브 (브라우저에서 열기)
├── Part0-실리콘밸리_엔지니어의_하루/
│   ├── slides/                                         # 강의 슬라이드 HTML
│   ├── Ch01-강사소개_강의개요/
│   │   ├── learn.html                                  # 챕터 학습 페이지
│   │   └── exercises/
│   │       ├── exercises.md                            # 이 챕터 실습 가이드
│   │       └── templates/
│   │           └── my-level.md                         # 학습자가 채울 빈 양식
│   └── Ch02-AI_Native_엔지니어로_일하기/
│       └── exercises/
│           ├── exercises.md
│           └── templates/
│               ├── my-workflow.md
│               └── habit-tracker.md
└── Part1-Claude_Code_마스터하기/
    ├── Ch01-Claude_Code_딥다이브/
    │   └── exercises/
    │       ├── exercises.md
    │       └── templates/CLAUDE.md
    ├── Ch02-Vibe_Coding의_본질과_한계/
    │   └── exercises/exercises.md                      # 90분 Vibe 챌린지 (템플릿 없음)
    ├── Ch03-Agentic_Engineering_개론/
    │   └── exercises/
    │       ├── exercises.md
    │       └── templates/
    │           ├── CLAUDE.md
    │           └── .agent-rules.md
    ├── Ch04-Harness_Engineering_개론/
    │   └── exercises/exercises.md                      # 집필 중
    └── Ch05-실전_바이브코딩_테크닉/
        └── exercises/exercises.md                      # 집필 중
```

## 챕터 흐름

| 챕터 | 실습 핵심 | 작업 위치 |
|---|---|---|
| Part 0 · Ch01 | 현재 나의 AI 코딩 레벨 자가진단 (10분) | 이 repo |
| Part 0 · Ch02 | 하루 워크플로우 매핑 + 1주일 습관 트래커 | 이 repo |
| Part 1 · Ch01 | example-vibe-project에 Claude Code 정착 | [example-vibe-project](https://github.com/jha0313/example-vibe-project) (별도 repo) |
| Part 1 · Ch02 | 90분 Vibe Coding 챌린지 (빈 디렉토리) | `~/projects/vibe-90min/` (학습자 로컬) |
| Part 1 · Ch03 | Ch02 vibe-90min repo에 Agentic 패턴 입히기 | Ch02 결과물 |

## 학습 자료 사이트

웹 버전 (슬라이드 + 실습): https://claude-code-lecture.vercel.app *(배포 예정)*

## 라이선스

강의 학습용. 자유롭게 fork·수정 가능.
