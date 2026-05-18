---
schema_version: 2026-05-v1
---

# ROLE_GUIDE - Plan / Work / Verify 역할별 진입 체크리스트

이 파일은 새 LLM이 어떤 역할로 프로젝트에 진입하는지에 따라
**Plan / Work / Verify** 세 경로 중 하나를 선택해 따르는 가이드다.

역할 간 경계는 엄격하다.
Plan LLM이 Work 단계를 수행하거나
Work LLM이 Plan 파일을 수정하는 것은 Stage Contamination이다.

---

## 역할 선택

| 역할 | 담당 범위 | 진입 커맨드 |
|------|----------|------------|
| **Plan** | 계획 파일 작성, 핸드오프 생성 | `/cm_run` 또는 해당 LLM의 Plan 커맨드 |
| **Work** | 구현, 파일 수정, git commit | `/work-start` |
| **Verify** | 게이트 검증, score_breakdown 계산 | `/check-gate` 또는 Verify 커맨드 |

---

## Plan 역할 체크리스트

Plan LLM이 세션 시작 시 확인하는 항목:

### 세션 시작
- [ ] `세션 모드` 선언: ROOT_ORCH 또는 PKG_LOCAL
- [ ] 계획 파일(`.harness/` 또는 `.ai/plans/drafts/`) 확인
- [ ] Guardian 호출 순서 확인: scope-referee -> policy-critic -> harness-guardian

### Plan 단계 산출물 (완료 조건)
- [ ] `work-order.md` 작성 완료 (목표, 비목표, 배치, 수용 기준)
- [ ] `work-order.json` 작성 완료 (`schema_version`, `batches[].files`, `acceptance_criteria`, `verification_commands`)
- [ ] handoff-auditor 검증 PASS
- [ ] Work LLM 전달 프롬프트 출력 후 STOP

### 절대 금지 (Plan 단계에서)
- ❌ Edit / Write (`.ai/plans/`, `.ai/handoffs/` 외 파일)
- ❌ git commit / git push / PR 생성
- ❌ Work 단계 구현 직접 수행

---

## Work 역할 체크리스트

Work LLM이 핸드오프 수신 후 진행하는 7단계:

### 1단계 - 핸드오프 파싱
- [ ] `work-order.md` + `work-order.json` 읽기
- [ ] `git_state.pre_work_steps` 실행 (브랜치 체크아웃 등)
- [ ] `batches[].files` -> `allowed_paths`로 등록 (scope enforcement)

### 2단계 - 구현
- [ ] batches 순서대로 파일 생성/수정
- [ ] `allowed_paths` 외 파일 수정 금지
- [ ] 구현 중 scope 이탈 감지 시 즉시 STOP + Plan LLM 에스컬레이션

### 3단계 - 커밋
- [ ] 각 batch를 별도 커밋으로 분리
- [ ] Conventional Commits 형식 준수
- [ ] `verification_commands` 사전 실행 확인

### 4단계 - verify-handoff 생성
- [ ] `.harness/60_templates/artifacts/verify-handoff.md` 템플릿 기반 작성
- [ ] 구현 파일 목록 기재
- [ ] exit_code 증빙 기재
- [ ] acceptance_criteria 체크 완료

### 5단계 - Verify LLM 전달
- [ ] verify-handoff.md 경로 + 1줄 요약 프롬프트 출력
- [ ] STOP (Verify 단계는 Verify LLM 담당)

### 절대 금지 (Work 단계에서)
- ❌ `batches[].files` 외 파일 수정
- ❌ Plan 파일(`.ai/plans/`) 수정
- ❌ Verify 단계 직접 수행 (Work LLM이 자기 작업을 검증하지 않음)

---

## Verify 역할 체크리스트

Verify LLM이 verify-handoff 수신 후 진행하는 단계:

### 1단계 - 핸드오프 파싱
- [ ] `verify-handoff.md` 읽기
- [ ] `work-order.json`의 `acceptance_criteria` 로드
- [ ] `verification_commands` 목록 확인

### 2단계 - 검증 실행
- [ ] 각 `verification_commands` 실행 -> exit_code 기록
- [ ] acceptance_criteria 체크리스트 실행
- [ ] 전 항목 ✅ -> PASS / 1개 이상 ❌ -> FAIL

### 3단계 - PASS 처리
- [ ] score_breakdown 계산 (correctness 30 + completeness 25 + compliance 25 + efficiency 20)
- [ ] `harness-run-recorder` 스킬로 `runs.jsonl` 기록
- [ ] FinalCheck LLM 전달 프롬프트 출력 + STOP

### 4단계 - FAIL 처리
- [ ] 기능/로직 FAIL -> `rework_count++`
- [ ] 환경/형식 오류 (`external_cause: true`) -> rework_count 유지, `rework.md` 사유 기록
- [ ] `rework_count >= 3` -> 에스컬레이션 + STOP
- [ ] Work LLM 재진입 프롬프트 출력 + STOP

---

## single_llm_mode: true 시 경로

단일 LLM이 모든 단계를 담당하는 경우에도 **역할 경계는 논리적으로 유지**한다.
각 단계 시작 시 `현재 역할: Plan / Work / Verify` 선언 후 해당 체크리스트를 따른다.

자세한 단계 흐름: `.harness/70_init-packs/BUILD_SEQUENCE.md` 참조
