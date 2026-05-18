# Claude BUILD_SEQUENCE

## 전제
- Phase 1~3 완료 상태예요.
- Claude Code 환경이 설치되어 있어야 해요.

## 환경 확인
- `claude --version`

## 생성 순서

### Step 1 — 디렉터리 구조 생성
```text
.claude/
├── commands/    (슬래시 커맨드 .md 파일)
├── agents/      (서브에이전트 .md 파일)
├── hooks/       (PreToolUse/PostToolUse .ps1/.sh 훅)
├── profiles/    (global-runtime-profile.md)
└── memory/      (MEMORY.md — 프로젝트 메모리)
```

### Step 2 — setup-guide-plan.md 기준으로 Plan 역할 커맨드 생성
- `cm_run.md`, `gemini-assist-plan.md` 또는 프로젝트 Plan 커맨드를 생성해요.
- guardian 에이전트는 `harness-guardian`, `policy-critic`, `scope-referee`, `handoff-auditor`를 둬요.

### Step 3 — setup-guide-work.md 기준으로 Work 역할 커맨드 생성
- `single_llm_mode: true`이거나 Claude가 Work도 담당할 때만 만들어요.
- 예시: `check-gate.md`, `task-init.md`

### Step 4 — llm-format-rules.md의 Claude 섹션 기준으로 훅 파일 생성
- PreToolUse 훅을 등록해요. (`settings.json`)
- Claude Code 환경에서만 동작하는 `.ps1` 또는 `.sh` 파일을 둬요.

### Step 5 — global-runtime-profile.md 초기화
- `single_llm_mode`를 설정해요.
- 담당 단계(Plan / Work / FinalCheck)를 기록해요.

### Step 6 — CLAUDE.md 생성 (브릿지 문서)
- `.harness/`와 `.claude/`를 연결하는 진입 문서를 만들어요.

### Step 7 — 검증
- [ ] `.claude/commands/`에 최소 1개 커맨드 존재
- [ ] `.claude/agents/`에 guardian 4종 존재
- [ ] `.claude/profiles/global-runtime-profile.md` 존재
- [ ] `.claude/settings.json` 훅 등록 확인
- [ ] `claude --version` 실행 가능

## 참조
- `.harness/10_bootstrap/setup-guide-plan.md`
- `.harness/10_bootstrap/setup-guide-work.md`
- `.harness/10_bootstrap/llm-format-rules.md`
