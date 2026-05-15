<!-- harness-template
source: .harness/60_templates/artifacts/rework.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: rework
task_id: "TASK_ID_PLACEHOLDER"
rework_number: 01
requested_at: "YYYY-MM-DD HH:MM"
requester: "Claude"
reason: "FAIL" # FAIL | USER_FEEDBACK
external_cause: false
schema_version: 2026-04-v1
---

# 재작업 요청 (Rework-NN)

> ⚠️ 재작업 횟수: N/3 (3회 초과 시 사용자 에스컬레이션)
>
> 이 파일은 이력 보존용이에요. 성공 후에도 삭제하지 않아요.
> `external_cause: true`이면 rework count를 소진하지 않아요.

## 재작업 사유
(구체적으로. "미흡"처럼 모호한 표현 금지)

## 불합격 근거

| # | 항목 | 기대 결과 | 실제 결과 | 심각도 |
|---|------|---------|---------|--------|
| 1 | | | | HIGH/MED/LOW |

## 재작업 범위 (이 범위만 수정할 것)

| # | 파일 경로 | 수정 내용 |
|---|-----------|---------|
| 1 | | |

## 비목표 (건드리지 말 것)
-

## 완료 기준
- [ ]
- [ ] 기존 evidence 재실행 exit 0 유지

## Codex 전달 입력 프롬프트 (아래 블록 복붙)

~~~
재작업 요청

핸드오프: {project}/.ai/tasks/TASK_ID/rework/rework-NN.md
task_id: TASK_ID
rework_count: N/3

재작업 범위: (위 표 요약)
완료 기준: (위 체크리스트)

/check-gate 즉시 실행 가능한 closeout 준비 필수.
~~~
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

