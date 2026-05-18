# {프로젝트명} - Codex / Work LLM Entry Guide

## 목적
이 문서는 Codex(또는 Work 단계 담당 LLM)가 {프로젝트명}에 들어올 때 읽는 문서입니다.

## 진입 표준 순서
1. 이 문서 (`AGENTS.md` 또는 동등 문서)
2. `.harness/70_init-packs/codex/README.md`
3. `.harness/70_init-packs/COMMON_INIT_SEQUENCE.md`
4. `.codex/config.toml` (또는 LLM별 설정 파일)

## 역할 경계
- Work 단계 담당: 핸드오프 수신 -> 구현 -> `verify-handoff` 생성
- Plan / FinalCheck 단계: 진입 금지 (Plan LLM 담당)

## Work 시작 방법
```text
/work-start
핸드오프: {project}/.ai/handoffs/YYYY-MM-DD_slug/work-order.md
(.agents/skills/work-start/SKILL.md 7단계 프로토콜 따름)
```

## Scope Enforcement
- `work-order.json`의 `batches[].files` 목록 외 파일 수정 금지
- `active-handoff.json`으로 `allowed_paths` 관리
- hook이 자동 차단해요. (`.codex/hooks/` 등록 시)

## Guardian 참조
| 에이전트 | 역할 |
|---|---|
| `harness_guardian` | 단계 준수 감사 |
| `scope_referee` | 범위 이탈 탐지 |
| `policy_critic` | 정책 위반 탐지 |
| `handoff_auditor` | 핸드오프 형식 검증 |

## Verify 완료 후
1. `score_breakdown` 계산
2. `.ai/harness/runs.jsonl` 기록 (`harness-run-recorder` 스킬)
3. FinalCheck LLM에 전달 프롬프트 출력 + STOP

## 참조 경로
- 핸드오프 위치: `.ai/handoffs/`
- 스킬: `.agents/skills/`
- 게이트 기준: `.ai/gates/INTEGRATION_GATE.md`
