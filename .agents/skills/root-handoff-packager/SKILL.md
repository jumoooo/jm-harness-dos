# root-handoff-packager 스킬

## 목적
Plan 단계 완료 후 Work LLM에게 전달할
work-order.md + work-order.json 동쌍을 생성한다.

## 트리거
plan-orchestrator Step 6에서 호출.

## 출력 위치
.ai/handoffs/YYYY-MM-DD_slug/
  ├── work-order.md   (사람이 읽는 형식)
  └── work-order.json (기계가 파싱하는 형식)

## work-order.json 필수 필드
{
  "schema_version": "2026-05-v1",      // 필수
  "handoff_id": "YYYY-MM-DD_slug",     // 필수
  "created_at": "YYYY-MM-DD",          // 필수
  "from": "Claude (Plan)",              // 필수
  "to": "Codex (Work)",                 // 필수
  "title": "작업 제목",                  // 필수
  "summary": "1~2줄 요약",              // 필수
  "git_state": {                        // 필수
    "expected_branch": "브랜치명",
    "base_branch": "main",
    "pre_work_steps": []
  },
  "batches": [                          // 필수, 최소 1개
    {
      "id": "commit-N",
      "commit_message": "type(scope): 요약",
      "files": [],                      // 빈 배열 불가
      "change_type": "create_new|modify|delete",
      "description": "상세 설명"
    }
  ],
  "acceptance_criteria": [],            // 필수, 최소 1개
  "verification_commands": [],          // 필수, 최소 1개
  "non_goals": [],
  "rework_count": 0,
  "score_breakdown": null
}

## work-order.md 필수 섹션
- 메타 표 (handoff_id, from, to, 브랜치, 생성일)
- 배경
- 사전 작업 (pre_work_steps)
- 커밋별 상세 (파일, 변경 내용)
- 비목표
- 수용 기준 (체크박스)
- 검증 명령

## handoff-auditor 검증 항목
packager 실행 후 반드시 handoff-auditor 호출:
  - schema_version 존재
  - git_state 필드 존재
  - batches[].files 비어있지 않음
  - acceptance_criteria 최소 1개 이상
  - verification_commands 최소 1개 이상

## 참조
- 계약: .harness/20_runtime/handoff-contract.md
- 감사: .claude/agents/handoff-auditor.md (또는 LLM 등가)
