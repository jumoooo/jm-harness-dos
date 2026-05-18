# Verify LLM 셋업 가이드

## Verify LLM의 책임

- Work LLM 결과 핸드오프를 수신해요.
- `verification_commands`를 전체 실행해요.
- `acceptance_criteria` 항목별로 PASS 또는 FAIL을 판정해요.
- `score_breakdown`을 계산해요.
- `runs.jsonl`에 결과를 기록해요.
- Plan LLM에게 전달할 Final Check 핸드오프를 생성해요.

## 만들어야 할 파일

### Step 1: 전역 프로필 생성

| 항목 | 값 |
|---|---|
| 파일 | `{llm_local}/profiles/global-runtime-profile.md` |
| 템플릿 | `.harness/60_templates/artifacts/runtime-profile.md` |

커스터마이즈 핵심:

- `verify_llm` 필드를 현재 LLM으로 설정해요.
- 단일 LLM 모드면 Final Check 역할 연결도 함께 정의해요.

### Step 2: check-gate 스킬 연결

| 항목 | 값 |
|---|---|
| 스킬 위치 | `.agents/skills/check-gate/SKILL.md` |
| 입력 | Verify 핸드오프 파일 경로 |
| 출력 | PASS/FAIL 판정, Evidence 목록, `score_breakdown` |

`check-gate`는 Verify 단계의 기본 진입점이에요.

### Step 3: harness-run-recorder 스킬 연결

| 항목 | 값 |
|---|---|
| 스킬 위치 | `.agents/skills/harness-run-recorder/SKILL.md` |
| 기록 위치 | `.ai/harness/runs.jsonl` |
| 기록 시점 | `check-gate` 완료 직후 |

PASS와 FAIL 모두 기록해야 해요.

필수 필드:

- `schema_version`
- `task_id`
- `result`
- `score_breakdown`
- `rework_count`

### Step 4: reviewer 에이전트 연결

이 단계는 선택이지만 권장해요.

| 항목 | 값 |
|---|---|
| 템플릿 | `.harness/60_templates/agents/reviewer-base.md` |
| 역할 | 코드와 문서 품질 상세 검토 |

### Step 5: Final Check 핸드오프 형식 정의

PASS 시 포함 항목:

- PASS 판정
- Evidence 요약
- 커밋 프롬프트 출력

FAIL 시 포함 항목:

- FAIL 항목 목록
- 재작업 범위
- 갱신된 `rework_count`

Plan LLM은 이 결과를 받아 최종 판정을 수행해요.

## 완료 체크리스트

- [ ] 전역 프로필이 존재하고 `verify_llm` 필드가 채워져 있어요.
- [ ] `check-gate` 스킬이 연결되어 있어요.
- [ ] `harness-run-recorder` 스킬이 연결되어 있어요.
- [ ] `score_breakdown` 계산 방식이 정의되어 있어요.
- [ ] PASS/FAIL 후 Plan LLM 전달 형식이 정의되어 있어요.
- [ ] FAIL 시 `rework_count` 증가 로직이 존재해요.

## 참조

- 채점 기준: `../50_scoring/scoring-framework.md`
- Work 런타임 정책: `../20_runtime/plan-work-verify-runtime.md`
- 핸드오프 계약: `../20_runtime/handoff-contract.md`
- 역할 x LLM 매트릭스: `setup-matrix.md`
