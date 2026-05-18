# HOW_TO_RECORD_RUN

## 언제 기록하나요?
Work 완료 후 Verify PASS가 난 직후에 바로 기록해요.

## 어떻게 기록하나요?
`.agents/skills/harness-run-recorder/SKILL.md`를 참조해서 `harness-run-recorder` 스킬로 기록해요.

## runs.jsonl 레코드 구조
```json
{
  "schema_version": "2026-05-v1",
  "run_id": "RUN-001",
  "date": "YYYY-MM-DD",
  "phase": "work",
  "handoff_id": "YYYY-MM-DD_slug",
  "rework_count": 0,
  "gate": "PASS",
  "score_breakdown": {
    "correctness": 30,
    "completeness": 25,
    "compliance": 25,
    "efficiency": 20,
    "total": 100
  },
  "notes": ""
}
```

## score_breakdown 설명
- `correctness`: 요구 결과를 정확하게 만들었는지 평가해요. 30점 배점이에요.
- `completeness`: 수용 기준을 빠짐없이 충족했는지 평가해요. 25점 배점이에요.
- `compliance`: 하네스 규칙과 핸드오프 계약을 지켰는지 평가해요. 25점 배점이에요.
- `efficiency`: 불필요한 반복 없이 깔끔하게 수행했는지 평가해요. 20점 배점이에요.

## 예외 규칙
외부 요인으로 FAIL이면 `external_cause: true`를 기록해요.
같은 사유는 `rework.md`에도 함께 남겨 재작업 원인을 추적해요.
