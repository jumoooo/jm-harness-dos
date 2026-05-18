# Hook 템플릿 가이드

## 역할
이 디렉터리는 하네스 enforcement 레이어 베이스예요.
정책 문서가 "해야 한다"를 서술하면, hook이 실제로 차단해요.

## Hook 종류

| 파일 | 역할 | 트리거 |
|---|---|---|
| `pre-tool-guard-base.md` | 승인 없는 Edit/Write 차단 | PreToolUse |
| `scope-enforcement-base.md` | Work 단계 허용 경로 외 파일 수정 차단 | PreToolUse |

## LLM별 구현 형식

| LLM | 형식 | 등록 방법 |
|---|---|---|
| Claude Code | `.ps1` (Windows) / `.sh` (Unix) | `.claude/settings.json` PreToolUse 등록 |
| Codex | `.js` | `.codex/hooks/` + config.toml hooks 섹션 |
| Generic | N/A | 해당 LLM 문서 확인 |

## 운영 원칙
- fail-open 원칙을 따라요.
- hook 파싱 실패 시 `exit 0`으로 처리해요.
- 차단보다 가용성을 우선하지만, 정상 파싱 시에는 정책 위반을 막아야 해요.

## 참조
- `.harness/10_bootstrap/llm-format-rules.md`
