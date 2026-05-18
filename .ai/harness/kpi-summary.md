# KPI Summary

## 목적
이 문서는 하네스 런 결과를 집계해서 운영 품질을 빠르게 확인하는 요약판이에요.

## 집계 기준 필드
- `rework_count`
- `score_breakdown.total`
- `gate_pass_rate`

## KPI 테이블

| 기간 | 총 런 수 | PASS율 | 평균 rework | 평균 score |
|---|---:|---:|---:|---:|

## 갱신 방법
runs.jsonl에서 집계해요.
`harness-run-recorder` 스킬 기록 후 수동 또는 스크립트로 갱신해요.
