<!-- harness-template
source: .harness/60_templates/artifacts/plan-review.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: plan-review
task_id: "TASK_ID_PLACEHOLDER"
handoff_id: "HANDOFF_ID_PLACEHOLDER"
reviewed_at: "YYYY-MM-DD HH:MM"
reviewer: "Codex"
status: "PASS" # PASS | FAIL
schema_version: 2026-04-v1
---

# Plan Review 결과

## 점수 요약

| 카테고리 | 만점 | 획득 | 비고 |
|---------|------|------|------|
| 실행 가능성 | 25 | 00 | |
| 범위 명확도 | 25 | 00 | |
| 사이드 이펙트 | 25 | 00 | |
| harness 정합성 | 25 | 00 | |
| **합계** | **100** | **00** | |

**판정: PASS / FAIL**
> 합격 기준: 총점 70+ AND 단일 카테고리 10점 미만 없을 것

---

## 카테고리별 상세

### 실행 가능성 (00/25)
- 평가 내용:
- 문제점 (있을 경우):

### 범위 명확도 (00/25)
- 평가 내용:
- 문제점 (있을 경우):

### 사이드 이펙트 (00/25)
- 평가 내용:
- 문제점 (있을 경우):

### harness 정합성 (00/25)
- 평가 내용:
- 문제점 (있을 경우):

---

## 피드백 (FAIL 시 필수)

> Codex → Claude 전달 피드백. 구체적으로 작성한다.
> 막연한 "보완 필요"는 금지. 어떤 항목이 왜 문제인지 명시.

| # | 피드백 항목 | 근거 | 심각도 (HIGH/MED/LOW) |
|---|------------|------|----------------------|
| 1 | | | |

---

## Reconciliation (Claude가 /plan-reconcile 실행 시 채움)

> Claude가 각 피드백을 검토한 결과. 채택/불채택/부분채택 + 이유 필수.

| # | 피드백 항목 | 결정 | 이유 |
|---|------------|------|------|
| 1 | | 채택 / 불채택 / 부분채택 | |

**재계획 필요 여부**: 예 / 아니오
**결정 날짜**: YYYY-MM-DD
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

