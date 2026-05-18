# Bridge 템플릿 가이드

## 역할
이 디렉터리는 LLM 진입 문서 템플릿을 보관해요.
새 LLM 인스턴스가 프로젝트에 들어올 때 가장 먼저 읽는 문서의 형식을 정의해요.

## 파일 목록

| 파일 | 용도 | 기반 |
|---|---|---|
| `claude-entry-template.md` | CLAUDE.md 형식 | Claude Code |
| `codex-entry-template.md` | AGENTS.md / Codex 진입 문서 형식 | Codex |

## 작성 원칙
- 이 템플릿을 복사한 뒤 `{placeholder}` 항목을 프로젝트에 맞게 채워요.
- `.harness/` 링크는 유지하고 실제 경로만 수정해요.
- 커맨드 목록은 LLM 로컬 레이어 구축 후 추가해요.
