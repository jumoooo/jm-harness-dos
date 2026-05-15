# Global Runtime Profile Policy

## 목적

plan, work, verify 담당 LLM과 모델을 전역 기본값으로 관리하는 정책을 정의해요.

## 필수 필드

- `profile_name`
- `default_plan_llm`
- `default_plan_model`
- `default_work_llm`
- `default_work_model`
- `default_verify_llm`
- `default_verify_model`
- `single_llm_mode`
- `artifact_roots`
- `guardian_policy_ref`
- `scoring_policy_ref`

## 규칙

- 단일 LLM 모드를 허용해요.
- 작업 시작 시 task-order는 이 전역 프로필을 기본으로 참조해요.
- high-impact override는 별도 기록이 필요해요.
