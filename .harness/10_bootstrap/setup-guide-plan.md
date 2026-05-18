# Plan LLM 셋업 가이드

## Plan LLM의 책임

- 사용자 요청을 수신하고 계획을 수립해요.
- Guardian 에이전트 4종을 적절한 시점에 호출해요.
- 핸드오프 문서 `MD + JSON` 동쌍을 생성해요.
- Work LLM에게 전달할 프롬프트를 출력해요.
- Verify 결과를 받은 뒤 Final Check를 수행해요.

## 만들어야 할 파일

순서가 중요해요.

### Step 1: 전역 프로필 생성

| 항목 | 값 |
|---|---|
| 파일 | `{llm_local}/profiles/global-runtime-profile.md` |
| 템플릿 | `.harness/60_templates/artifacts/runtime-profile.md` |

커스터마이즈 항목:

- `profile_name`: 프로젝트명
- `plan_llm`: 현재 LLM 이름
- `work_llm`: Work 담당 LLM
- `verify_llm`: Verify 담당 LLM
- `single_llm_mode`: `true` 또는 `false`
- `artifact_roots`: `.ai/` 경로

### Step 2: Guardian 에이전트 4종 생성

각 파일 위치는 `{llm_local}/agents/`예요.
템플릿 기준은 `.harness/60_templates/agents/guardian-base.md`예요.

| 에이전트 | 파일명 | 호출 시점 |
|---|---|---|
| scope-referee | `scope-referee.md` | 범위가 불명확할 때 즉시 |
| policy-critic | `policy-critic.md` | Plan 종료 직전 |
| harness-guardian | `harness-guardian.md` | Plan 완료 직전, Final Check 직전 |
| handoff-auditor | `handoff-auditor.md` | 핸드오프 생성 직후 |

각 파일에는 아래 YAML front matter를 반드시 포함해요.

```yaml
agent_type: guardian
extends: guardian-base
schema_version: 2026-04-v1
mode: {역할별 값}
```

### Step 3: 오케스트레이터 커맨드 생성

| 항목 | 값 |
|---|---|
| 파일 | `{llm_local}/commands/` 또는 `{llm_local}/rules/` |
| 템플릿 | `.harness/60_templates/agents/orchestrator-base.md` + `.harness/60_templates/artifacts/handoff.md` |

핵심 흐름은 아래 8단계를 포함해야 해요.

1. 요청 파싱
2. `scope-referee` 호출 조건 판정
3. 계획 수립 단계 실행
4. `policy-critic` 호출
5. `harness-guardian` 호출
6. handoff `MD + JSON` 생성
7. `handoff-auditor` 호출
8. PASS가 아니면 Work LLM 전달 금지

### Step 4: root-handoff-packager 연결

`root-handoff-packager`는 핸드오프 `MD + JSON`을 같은 계약으로 만들기 위한 공유 스킬이에요.

| 항목 | 값 |
|---|---|
| 스킬 위치 | `.agents/skills/root-handoff-packager/SKILL.md` |
| 연결 방식 | 오케스트레이터 커맨드에서 이 스킬을 참조 |

### Step 5: Work LLM 전달 프롬프트 형식 설정

Claude 기준 전달 형식 예시는 아래와 같아요.

```text
/work-start
핸드오프: {project}/.ai/handoffs/YYYY-MM-DD_slug/work-order.md
참고: {1줄 작업 요약}
```

전달 프롬프트는 handoff 경로가 바로 보이도록 유지해요.
전달 이후 Work 단계 범위 선언은 `../20_runtime/work-start-session-protocol.md`를 따라가요.

### Step 6: 설정 파일 등록

| LLM | 등록 위치 | 목적 |
|---|---|---|
| Claude | `settings.json` | 커맨드와 훅 연결 |
| Codex | `config.toml` | 에이전트와 규칙 연결 |
| Generic | 별도 없음 | Markdown 지침 수동 준수 |

## 완료 체크리스트

- [ ] 전역 프로필 파일이 존재하고 `plan_llm` 필드가 채워져 있어요.
- [ ] Guardian 4종 파일이 존재하고 YAML front matter가 완비되어 있어요.
- [ ] 오케스트레이터 커맨드가 존재하고 8단계 흐름이 반영되어 있어요.
- [ ] `root-handoff-packager` 참조가 연결되어 있어요.
- [ ] `handoff-auditor` PASS 없이 Work 전달이 불가능해요.
- [ ] Work LLM 전달 프롬프트 형식이 정의되어 있어요.

## 참조

- 역할 x LLM 매트릭스: `setup-matrix.md`
- 파일 형식 규칙: `llm-format-rules.md`
- 핸드오프 계약: `../20_runtime/handoff-contract.md`
- 역할 분리 정책: `../20_runtime/multi-llm-role-policy.md`
- Work 시작 프로토콜: `../20_runtime/work-start-session-protocol.md`
