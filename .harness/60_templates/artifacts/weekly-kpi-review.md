<!-- harness-template
source: .harness/60_templates/artifacts/weekly-kpi-review.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: weekly-kpi-review
schema_version: 2026-04-v1
---

# 주간 KPI 리뷰

> 🗓️ 매주 금요일에 `node .ai/scripts/kpi-aggregate.mjs --week YYYY-Www` 결과를 붙여 넣어요.

## 리뷰 정보

- 리뷰 날짜:
- 대상 주차:
- 리뷰어:

## KPI 스냅샷

```text
작업 성공률:
통합 게이트 차단율:
Exception 비율:
검증 루프 P95:
평균 시간 절감:
```

## SLO 상태

| SLO | 기준 | 이번 주 | 상태 |
|-----|------|---------|------|
| P95 | 60초 이하 |  |  |
| Exception | 2% 이하 |  |  |

## 상위 이슈 3건

| 우선순위 | 이슈 | 영향 | 대응 |
|----------|------|------|------|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |

## 다음 주 액션

- [ ] 
- [ ] 
- [ ] 
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->
