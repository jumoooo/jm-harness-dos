---
template: verify-handoff
schema_version: 2026-05-v1
---

# Verify Handoff - {handoff_id}

Work 단계가 완료됐을 때 Verify LLM에게 전달하는 핸드오프 문서.
work-order.json의 acceptance_criteria를 기준으로 검증 결과를 기록한다.

## 메타

| 항목 | 값 |
|------|----|
| handoff_id | {handoff_id} |
| schema_version | 2026-05-v1 |
| from | Work LLM |
| to | Verify LLM |
| created_at | YYYY-MM-DD |
| work_branch | {branch_name} |

## 구현 요약

{1~3줄 구현 완료 내용 요약}

## 구현 파일 목록

work-order.json의 batches[].files 기준으로 실제 수정/생성된 파일:

| 파일 | 변경 유형 | 상태 |
|------|----------|------|
| {path} | create_new / modify | ✅ 완료 |

## Exit Code 증빙

work-order.json의 verification_commands 실행 결과:

```json
[
  { "command": "{command}", "exit_code": 0, "summary": "{결과 요약}" }
]
```

## Acceptance Criteria 체크

work-order.json의 acceptance_criteria 기준:

- [ ] {criteria_1}
- [ ] {criteria_2}
- [ ] {criteria_3}

## rework_count

| 항목 | 값 |
|------|----|
| rework_count | 0 |
| external_cause | false |

## Verify LLM 전달 메시지

위 증빙을 바탕으로 check-gate를 실행하고 PASS / FAIL 판정을 내려 주세요.
PASS 시 FinalCheck LLM에 전달 프롬프트를 출력하고 STOP.
FAIL 시 rework_count 판정 후 Work LLM 재진입 프롬프트 출력 후 STOP.
