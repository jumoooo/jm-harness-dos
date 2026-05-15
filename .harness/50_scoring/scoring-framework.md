# Scoring Framework

## 기본 규칙

- 각 카테고리는 100점 만점이에요.
- 점수뿐 아니라 이유가 필수예요.
- high-impact 판단은 점수표 없이 채택하지 않아요.
- low-impact 판단만 제한적으로 자율 진행해요.

## 권장 산출물

- `decision-scorecard.md`
- 필요 시 `decision-scorecard.json`

## runs.jsonl 스키마 정책

- `runs.jsonl`은 `schema_version: 2026-05-v1`부터 `score_breakdown` 필드가 필수예요.
- 마이그레이션 이전에 작성된 `2026-04-v1` 레코드는 `score_breakdown: null`을 허용해요.

---

## 구현 위임 (Implementation Delegation)

| 항목 | 위임 파일 | 강도 | 상태 |
|-----|---------|-----|-----|
| score_breakdown 입력 강제 | `.agents/skills/harness-run-recorder/SKILL.md` | 스킬 출력 계약 | ✅ 활성 |
| 런 기록 절차 가이드 | `.ai/harness/HOW_TO_RECORD_RUN.md` | 가이드 문서 | ✅ 존재 |
| KPI 집계 | `.ai/harness/kpi-summary.md` | 수동 집계 | ✅ 운용 중 |

**소급 처리 선언**: 기존 7건 (2026-05-14 이전) `score_breakdown: null` — 점수 집계 제외.
**신규 기준**: 2026-05-14 이후 신규 런은 `score_breakdown` 4개 필드 필수.
