# harness-run-recorder 스킬

## 목적
check-gate 완료 후 작업 결과를 runs.jsonl에 표준 형식으로 기록한다.
KPI 집계 및 품질 추적의 데이터 원천.

## 트리거
check-gate Step 5에서 호출.

## 기록 형식 (schema_version: 2026-05-v1)
{
  "schema_version": "2026-05-v1",
  "run_id": "YYYY-MM-DD_task-id",
  "task_id": "task-id",
  "recorded_at": "YYYY-MM-DD",
  "result": "PASS" | "FAIL" | "REWORK",
  "rework_count": 0,
  "score_breakdown": {
    "correctness": 0-10,
    "completeness": 0-10,
    "quality": 0-10,
    "process_compliance": 0-10
  },
  "evidence_count": N,
  "notes": "선택적 메모"
}

## 기록 위치
파일: .ai/harness/runs.jsonl
형식: JSON Lines (한 줄 = 한 레코드)
기록 방식: 파일 끝에 append

## score_breakdown 계산 기준
각 항목 0~10점:
  correctness: 요구 기능이 올바르게 구현됐는가
  completeness: acceptance_criteria 전 항목 충족했는가
  quality: 코드/문서 품질, 컨벤션 준수
  process_compliance: 하네스 단계 준수, 핸드오프 형식 준수

## PASS 없는 기록 금지
runs.jsonl 기록은 check-gate 완료 후에만 수행.
중간 상태 기록 금지.

## 참조
- 채점 기준: .harness/50_scoring/scoring-framework.md
- KPI 집계: .ai/harness/kpi-summary.md
