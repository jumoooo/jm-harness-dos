# Codex BUILD_SEQUENCE

## 전제
- Phase 1~3 완료 상태예요.
- Codex CLI 환경이 설치되어 있어야 해요.

## 환경 확인
- `codex --version` 또는 `codex -v`

## 생성 순서

### Step 1 — 디렉터리 구조 생성
```text
.codex/
├── agents/      (에이전트 .toml 파일)
├── rules/       (플레이북 .md 파일)
├── hooks/       (JS 훅 파일)
├── state/       (.gitignore 대상)
└── config.toml  (Codex 설정)
```

### Step 2 — setup-guide-work.md 기준으로 Work 역할 에이전트 생성
- `work_orchestrator.toml`, `feature_agent.toml` 등을 만들어요.
- `work-start` 스킬 연결 방법은 `.agents/skills/work-start/SKILL.md`를 참조해요.

### Step 3 — setup-guide-verify.md 기준으로 Verify 역할 에이전트 생성
- `reviewer.toml`, `check_gate_agent.toml` 등을 만들어요.
- `harness-run-recorder` 스킬 연결도 함께 둬요.

### Step 4 — llm-format-rules.md의 Codex 섹션 기준으로 훅 파일 생성
- JS 훅 파일(`.js`)을 만들어요.
- `config.toml`의 hooks 섹션에 등록해요.

### Step 5 — guardian 에이전트 생성 (toml 형식)
- `harness_guardian.toml`, `policy_critic.toml`, `scope_referee.toml`, `handoff_auditor.toml`을 만들어요.
- `guardian-base.md` 상속 구조를 `.toml` 주석으로 표시해요.

### Step 6 — config.toml 초기화
- `approval_policy`, `sandbox_mode`를 설정해요.
- 담당 단계(Work / Verify)를 기록해요.

### Step 7 — 검증
- [ ] `.codex/agents/`에 최소 work + guardian 에이전트 존재
- [ ] `.codex/config.toml` 존재
- [ ] `.codex/hooks/`에 최소 1개 훅 존재
- [ ] `codex --version` 실행 가능

## 참조
- `.harness/10_bootstrap/setup-guide-work.md`
- `.harness/10_bootstrap/setup-guide-verify.md`
- `.harness/10_bootstrap/llm-format-rules.md`
