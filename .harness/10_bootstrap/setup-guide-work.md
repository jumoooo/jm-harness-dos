# Work LLM 셋업 가이드

## Work LLM의 책임

- Plan LLM으로부터 핸드오프를 수신해요.
- `work-start` 스킬로 실행 진입을 시작해요.
- 허용 경로만 수정하도록 Scope enforcement를 유지해요.
- 구현과 커밋을 수행해요.
- `acceptance_criteria` 충족 여부를 확인해요.
- Verify LLM에게 결과 핸드오프를 생성해요.

## 만들어야 할 파일

순서가 중요해요.

### Step 1: 전역 프로필 생성

| 항목 | 값 |
|---|---|
| 파일 | `{llm_local}/profiles/global-runtime-profile.md` |
| 템플릿 | `.harness/60_templates/artifacts/runtime-profile.md` |

커스터마이즈 핵심:

- `work_llm` 필드를 현재 LLM으로 설정해요.
- 단일 LLM이면 `plan_llm`, `verify_llm`도 함께 같은 LLM으로 맞춰요.

### Step 2: work-start 스킬 연결

| 항목 | 값 |
|---|---|
| 스킬 위치 | `.agents/skills/work-start/SKILL.md` |
| 목적 | 핸드오프 파일 경로를 입력받아 Work 세션 7단계 흐름 시작 |

연결 방법:

| LLM | 연결 방식 |
|---|---|
| Claude | `CLAUDE.md` 또는 `commands/work-start.md`에서 스킬 참조 |
| Codex | `config.toml` 스킬 등록 또는 `agents/work_entry.toml` 생성 |
| Generic | Markdown 절차로 수동 연결 |

세부 동작은 `../20_runtime/work-start-session-protocol.md`를 기준으로 해요.

### Step 3: Scope enforcement 설정

목적은 핸드오프의 `allowed_paths` 밖 파일 수정을 차단하는 거예요.

| LLM | 구현 방식 |
|---|---|
| Claude | `hooks/scope-guard.ps1` + `settings.json` `PreToolUse` 등록 |
| Codex | `hooks/pre_tool_policy.js`에 `allowed_paths` 체크 로직 추가 |
| Generic | 자동 훅 없음, 수동 준수 |

상태 파일 위치:

- `{llm_local}/state/active-handoff.json`

이 파일에는 현재 handoff 범위를 기록해요.

### Step 4: 구현 에이전트 최소 세트

| 에이전트 | 역할 | 템플릿 |
|---|---|---|
| orchestrator | 구현 작업 라우팅 | `orchestrator-base.md` |
| feature | 실제 구현 담당 | `feature-base.md` |
| reviewer | 구현 검토 | `reviewer-base.md` |

템플릿 기준 경로는 `.harness/60_templates/agents/`예요.

### Step 5: 커밋 규칙 설정

형식은 Conventional Commits를 사용해요.

- `feat`
- `fix`
- `chore`
- `docs`
- `refactor`
- `test`

등록 위치:

| LLM | 위치 |
|---|---|
| Claude | `CLAUDE.md` |
| Codex | `rules/commit-rules.md` 또는 `config.toml` |
| Generic | 역할 문서 내부에 명시 |

### Step 6: Verify 핸드오프 생성 설정

작업 완료 후 생성할 핸드오프에는 아래를 포함해요.

- 실행한 커밋 목록
- `Evidence: { command, exit_code, summary }[]`
- `verification_commands` 실행 결과
- Verify가 재실행할 수 있는 범위와 기대 결과

권장 위치:

- `.ai/handoffs/YYYY-MM-DD_work-to-verify/`

## rework 처리

- `check-gate` FAIL이면 `rework_count`를 1 증가시켜요.
- `rework_count >= 3`이면 에스컬레이션 STOP이에요.
- 외부 환경 원인 FAIL이면 카운트를 올리지 않고 `rework.md`에 사유를 기록해요.

## 완료 체크리스트

- [ ] 전역 프로필이 존재하고 `work_llm` 필드가 채워져 있어요.
- [ ] `work-start` 스킬 연결이 완료되어 있어요.
- [ ] Scope enforcement가 설정되어 있어요.
- [ ] 구현 에이전트 최소 3종이 존재해요.
- [ ] Atomic commit 규칙이 정의되어 있어요.
- [ ] Conventional Commits 형식이 강제되어 있어요.
- [ ] Verify 핸드오프 생성 형식이 정의되어 있어요.
- [ ] `rework_count` 관리 로직이 존재해요.

## 참조

- Work 시작 프로토콜: `../20_runtime/work-start-session-protocol.md`
- 핸드오프 계약: `../20_runtime/handoff-contract.md`
- 재작업 정책: `../20_runtime/plan-work-verify-runtime.md`
- 역할 x LLM 매트릭스: `setup-matrix.md`
