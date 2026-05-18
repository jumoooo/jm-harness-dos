# 범용 실행 스킬 (.agents/skills/)

## 이 폴더의 용도
Plan→Work→Verify 3단계 흐름을 실제로 연결하는 실행 스킬 모음.
Claude, Codex, 기타 LLM 어느 쪽이든 읽고 적용 가능한 LLM-agnostic 형식.

## 스킬 목록

| 스킬 | 역할 | 사용 시점 |
|------|------|---------|
| work-start | Work LLM 핸드오프 수신 → 실행 | Work 단계 시작 시 |
| plan-orchestrator | Plan LLM 3단계 흐름 실행 | Plan 단계 전체 |
| check-gate | Verify 게이트 실행 + 채점 | Verify 단계 |
| harness-run-recorder | runs.jsonl 런 기록 | Verify 완료 후 |
| root-handoff-packager | 핸드오프 MD+JSON 동쌍 생성 | Plan→Work 전환 시 |

## LLM별 연결 방법
Claude: SKILL.md를 commands/ 또는 agents/에서 참조
Codex: config.toml skills 섹션에 경로 등록 또는 agents/.toml에서 참조
Generic: SKILL.md를 직접 읽고 수동 적용

## 스킬 형식 규칙
- 파일명: SKILL.md (고정)
- 언어: 한국어 (프로젝트 표준)
- LLM 특정 문법 사용 금지 (어느 LLM이든 이해 가능해야 함)
