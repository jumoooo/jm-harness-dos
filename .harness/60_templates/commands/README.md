# 커맨드 템플릿 가이드

## 목적

`.harness/60_templates/commands/`는 LLM-agnostic 커맨드 베이스를 보관해요.
각 LLM이 자기 로컬 레이어에서 구현할 때 참조하는 기준 문서예요.

## 커맨드 베이스 목록

| 파일 | 역할 | 담당 단계 |
|---|---|---|
| `cm-run-base.md` | Plan 진입점, orchestrator 라우팅 | Plan |
| `check-gate-base.md` | 통합 게이트 검증 + PASS/FAIL 판정 | Verify / FinalCheck |
| `task-init-base.md` | 태스크 스캐폴딩 | Plan / Work |

## LLM별 구현 위치
- Claude: `.claude/commands/`
- Codex: `.codex/rules/`

## 원칙
이 파일은 What(의도)을 서술해요.
How(구현)는 LLM 전용 레이어에서 담당해요.
