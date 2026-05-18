# Work Order Schema

이 파일이 work-order.json의 스키마 기준입니다. 정책 서술은 handoff-contract.md를 참조.

## 필드 명세

| 필드 | 타입 | 필수 | 설명 |
|---|---|---|---|
| schema_version | string | ✅ | `2026-05-v1` |
| handoff_id | string | ✅ | `YYYY-MM-DD_slug` 형식 |
| created_at | string | ✅ | ISO date |
| from | string | ✅ | `Claude (Plan)` 등 |
| to | string | ✅ | `Codex (Work)` 등 |
| title | string | ✅ | 작업 제목 |
| summary | string | ✅ | 1~2줄 요약 |
| git_state.work_dir | string | ✅ | 절대 경로 |
| git_state.expected_branch | string | ✅ | 작업 브랜치명 |
| git_state.base_branch | string | ✅ | 기준 브랜치 |
| git_state.pre_work_steps | array | ✅ | 브랜치 생성 등 사전 단계 |
| batches | array | ✅ | 커밋 배치 목록 (최소 1개) |
| batches[].id | string | ✅ | `commit-N` |
| batches[].commit_message | string | ✅ | Conventional Commits 형식 |
| batches[].files | array | ✅ | 대상 파일 목록 (빈 배열 불가) |
| batches[].change_type | string | ✅ | `create_new` / `modify` / `delete` |
| batches[].description | string | ✅ | 작업 상세 설명 |
| acceptance_criteria | array | ✅ | PASS 조건 문장 목록 |
| verification_commands | array | ✅ | 검증 명령 목록 |
| non_goals | array | ❌ | 이번 범위 밖 항목 |
| rework_count | number | ✅ | 초기값 0 |
| score_breakdown | object\|null | ✅ | Verify 완료 후 채움, 초기 null |

## 최소 인스턴스 예시
```json
{
  "schema_version": "2026-05-v1",
  "handoff_id": "2026-05-16_sample-task",
  "created_at": "2026-05-16",
  "from": "Claude (Plan)",
  "to": "Codex (Work)",
  "title": "샘플 작업",
  "summary": "공유 작업면 스캐폴드를 추가해요.",
  "git_state": {
    "work_dir": "E:\\MY_PROJECTS\\AI\\my_harness\\jm-harness-dos",
    "expected_branch": "ops/2026-05_harness-kit-expansion",
    "base_branch": "main",
    "pre_work_steps": []
  },
  "batches": [
    {
      "id": "commit-1",
      "commit_message": "chore(workspace): .ai/ 워크스페이스 스캐폴드 추가",
      "files": [".ai/README.md"],
      "change_type": "create_new",
      "description": "공유 작업면 문서를 추가해요."
    }
  ],
  "acceptance_criteria": [
    ".ai/README.md가 존재한다"
  ],
  "verification_commands": [
    "git status --short"
  ],
  "rework_count": 0,
  "score_breakdown": null
}
```
