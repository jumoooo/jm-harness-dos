<!-- harness-template
source: .harness/60_templates/artifacts/closeout.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: closeout
task_id: "TASK_ID_PLACEHOLDER"
completed_at: "YYYY-MM-DD HH:MM"
author: "Codex"
rework_count: 0
schema_version: 2026-04-v1
---

# 작업 완료 보고 (Closeout)

## 완료 요약
(2~3줄. 무엇을 했고, 어떤 결과를 냈는지)

## 구현 내역

| # | 파일 경로 | 변경 내용 |
|---|-----------|---------|
| 1 | | |

## 검증 결과

| 명령 | exit code | 결과 요약 |
|------|-----------|---------|
| pnpm test --run | 0 | |
| pnpm typecheck | 0 | |
| pnpm lint | 0 | |

## TDD 실행 기록
- RED: (어떤 테스트를 먼저 실패시켰는지)
- GREEN: (어떤 최소 구현으로 초록 상태 만들었는지)
- 검증: (마지막에 무엇을 다시 검증했는지)

## Evidence 파일 목록
- `.ai/tasks/TASK_ID/evidence/001_typecheck.txt`
- `.ai/tasks/TASK_ID/evidence/002_lint.txt`
- `.ai/tasks/TASK_ID/evidence/003_test.txt`

## 예외/재시도 기록
(없으면 "없음")

## Claude /check-gate 즉시 실행 가능 여부
- [ ] todo.md 체크리스트 반영 완료
- [ ] evidence/ 파일 저장 완료
- [ ] 의사결정 로그 갱신 완료
- [ ] 커밋 완료

## 반복 작업 감지 (스킬 제안용)
(이번 작업에서 3회 이상 반복된 패턴이 있으면 기록)
- 없음
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

