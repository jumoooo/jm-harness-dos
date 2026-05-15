<!-- harness-template
source: .harness/60_templates/artifacts/handoff.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
# Work Order: 작업 제목을 적어요

---
schema_version: 2026-04-v1
task_id: TASK_ID_PLACEHOLDER
from: PLAN_LLM
to: WORK_LLM
created: YYYY-MM-DD
plan_ref: .ai/plans/drafts/YYYY-MM-DD_slug.md
---

## 작업 배경

- 왜 이 작업이 필요한지 2~4줄로 적어요.

## 핵심 설계 결정

| 결정 | 내용 |
|------|------|
| 예시 | 결정 이유를 적어요 |

## TDD 적용 여부

- 적용 여부 또는 예외 사유를 적어요.

---

## 수정 파일 목록 (이 목록 외 파일 수정 금지)

### 신규 생성 파일

| # | 파일 경로 | 변경 내용 요약 |
|---|-----------|--------------|
| 1 | | |

### 이동/리네임

| # | 원본 경로 | 대상 경로 | 방법 |
|---|-----------|----------|------|
| 1 | | | |

### 수정 파일

| # | 파일 경로 | 변경 내용 요약 |
|---|-----------|--------------|
| 1 | | |

---

## 완료 기준

- [ ] 체크 가능한 조건을 적어요.

---

## Evidence 저장 경로

- `.ai/tasks/TASK_ID/evidence/001_file_tree.txt`
- `.ai/tasks/TASK_ID/evidence/002_git_log.txt`
- `.ai/tasks/TASK_ID/evidence/003_ref_check.txt`

---

## 커밋 전략

1. `type(scope): description`
2. 필요하면 Atomic 커밋 순서를 적어요.
<!-- CORE:END -->
<!-- ENV:START -->
- model: handoff를 받는 실행 모델을 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: `.ai/` 같은 공유 산출물 루트를 적어요.
- command_prefix: `/` 또는 환경별 접두어를 적어요.
- llm_role_binding: Plan / Work / Verify / Final Check 담당 LLM을 적어요.
<!-- ENV:END -->
