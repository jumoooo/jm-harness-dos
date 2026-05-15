<!-- harness-template
source: .harness/60_templates/artifacts/plan.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
title: "(주제 한 줄)"
date: YYYY-MM-DD
status: draft
schema_version: 2026-04-v1
---

# (주제) — 계획 초안

## 배경 / 목표

- 왜 지금 이 계획이 필요한지 (1~3문단)

## 비목표 (Non-goals)

- 이번에 **하지 않을** 것 (범위 고정)

## 현재 상태 / 가정

- 레포·정책·제약·의존성 등 검증 전제

## 대안 비교

| 대안 | 장점 | 단점 |
|------|------|------|
| A — … | | |
| B — … | | |

## 추천안

- 무엇을 할지, 왜 그 안을 택했는지

## 리스크 / 미결정 사항

- 열린 질문, 결정 필요한 지점

---

## 외부 리뷰 (Gemini CLI 등)

_`/gemini-assist-plan` 커맨드 Step 2가 자동으로 채워줍니다. 또는 아래 형식으로 직접 붙여넣기._

```
(붙여넣기)
```

### 제한 사항 (실행 실패 시)

- 사용한 전략:
- 주요 stderr:
- exit code:

---

## Reconciliation (조정)

외부 의견을 **무조건 반영하지 말 것.** 결정과 이유를 남긴다.

| 항목 | 외부 의견 요지 | 결정 (채택/불채택/부분) | 이유 |
|------|--------------|----------------------|------|
| | | | |

---

## 수정된 최종 계획 요약

_Reconciliation 반영 후 5~10줄로 요약._

- (요약 문단)

---

## 실행 핸드오프 — Phase C

_`/gemini-assist-plan` Step 4가 자동으로 채워줍니다. 직접 써도 됩니다._

- **계획 파일**: (이 파일 경로)
- **목표**: (1~3줄)
- **비목표**: (1~3줄)
- **수용 기준**:
  - (체크 가능한 조건 1)
  - (체크 가능한 조건 2)
- **불변/위험**: (핵심 자산 삭제·정합 이탈 등 invariants에 걸릴 항목)
- **다음 액션**: `planner` 선행 필요 / 또는 `feature`·`api` 직행

_이 블록을 채운 뒤 `/cm_run 계획 파일: (경로)` 를 입력하면 오케스트레이터가 실행합니다._
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

