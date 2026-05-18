<!-- harness-template
source: .harness/60_templates/artifacts/runtime-profile.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
# Global Runtime Profile — PROFILE_NAME

## 프로필 정보

```yaml
profile_name: PROFILE_NAME
default_plan_llm: PLAN_LLM
default_plan_model: PLAN_MODEL
default_work_llm: WORK_LLM
default_work_model: WORK_MODEL
default_verify_llm: VERIFY_LLM
default_verify_model: VERIFY_MODEL
final_check_llm: FINAL_CHECK_LLM
final_check_model: FINAL_CHECK_MODEL
single_llm_mode: false
artifact_roots:
  - .ai/
guardian_policy_ref: .harness/10_bootstrap/guardian-installation-strategy.md
scoring_policy_ref: .harness/50_scoring/scoring-framework.md
role_flow_ref: .ai/role-flow.md
```

## 모드 설명

- 이 프로필은 Plan / Work / Verify / Final Check 역할을 어떻게 나눌지 설명해요.
- 단일 LLM 모드면 하나의 LLM이 모든 단계를 맡아요.
- 다중 LLM 모드면 역할별 handoff가 필요해요.

## 단계별 책임

| 단계 | 담당 LLM | 기대 출력 |
|---|---|---|
| Plan | | 계획 초안 + handoff |
| Work | | 구현 산출물 |
| Verify | | evidence + 검증 요약 |
| Final Check | | PASS / FAIL |

## 참조 정책

- 역할 흐름 전체 기준: `.ai/role-flow.md`
- 정책 기준: `.harness/40_profiles/global-runtime-profile-policy.md`
- multi-LLM 정책: `.harness/20_runtime/multi-llm-role-policy.md`
<!-- CORE:END -->
<!-- ENV:START -->
```yaml
model: {model_name}
project_root: E:/PROJECT_ROOT
artifact_root: .ai/
command_prefix: /
llm_role_binding:
  plan: claude
  work: codex
  verify: codex
  final_check: claude
```
<!-- ENV:END -->

