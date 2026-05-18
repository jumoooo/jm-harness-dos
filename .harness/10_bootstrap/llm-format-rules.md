# LLM 파일 형식 규칙

## 이 문서의 용도

LLM 종류에 따라 커맨드, 에이전트, 훅, 설정 파일의 형식이 달라져요.
셋업 시 본인 LLM 섹션을 먼저 확인하고,
그 형식에 맞는 파일을 생성해야 해요.

## Claude 계열 (Claude Code)

| 항목 | 형식 | 위치 | 비고 |
|---|---|---|---|
| 커맨드 | `.md` 슬래시 커맨드 | `.claude/commands/` | `/command-name` 형식 |
| 에이전트 | `.md` 프롬프트 문서 | `.claude/agents/` | YAML front matter 필수 |
| 훅 | `.ps1` (Windows) / `.sh` (Unix) | `.claude/hooks/` | `PreToolUse`, `PostToolUse` 등 |
| 설정 | `settings.json` | `.claude/` | hooks 등록, MCP 설정 |
| 프로필 | `.md` | `.claude/profiles/` | 자연어 + 구조화 혼합 |
| 스킬 | `SKILL.md` | `.agents/skills/` | Claude + Codex 공용 |

훅 등록 예시 (`settings.json`):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": ".claude/hooks/confirm-before-edit.ps1"
          }
        ]
      }
    ]
  }
}
```

## Codex 계열 (OpenAI Codex)

| 항목 | 형식 | 위치 | 비고 |
|---|---|---|---|
| 에이전트 | `.toml` | `.codex/agents/` | `[agent]` 섹션 필수 |
| 규칙 / 플레이북 | `.md` | `.codex/rules/` | 운영 규칙 설명 |
| 훅 | `.js` | `.codex/hooks/` | stdin / stdout JSON |
| 설정 | `config.toml` | `.codex/` | `approval_policy`, `sandbox_mode` 등 |
| 훅 등록 | `hooks.json` | `.codex/` | 훅 파일 경로 + 이벤트 매핑 |
| 프로필 | `.md` | `.codex/profiles/` | 역할과 모델 바인딩 문서 |

`.toml` 에이전트 예시:

```toml
[agent]
name = "feature-agent"
description = "구현 담당 에이전트"
scope = "allowed_paths 내부 구현"
```

## Generic LLM (기타 LLM)

| 항목 | 형식 | 위치 | 비고 |
|---|---|---|---|
| 모든 설정 | `.md` | `.{llm_name}/` 또는 루트 | Markdown-only fallback |
| 에이전트 | `.md` 역할 문서 | `.{llm_name}/agents/` | |
| 규칙 | `.md` | `.{llm_name}/rules/` | |
| 훅 | 없음 | - | 수동 준수 |

## 혼합 운용 시 (Claude + Codex)

- 공유 스킬은 `.agents/skills/` 아래 `SKILL.md` 형식을 사용해요.
- 공유 워크스페이스 `.ai/`는 형식 중립 레이어예요.
- 하네스 정책 `.harness/`는 모든 LLM이 공통으로 참조해요.

## 형식 선택 원칙

1. 각 LLM의 네이티브 형식을 우선 사용해요.
2. 공유 레이어인 `.agents/`와 `.ai/`는 Markdown 중심으로 유지해요.
3. 단일 LLM 운용 시에는 해당 LLM의 형식만 사용해요.
4. 새로운 LLM을 추가하면 이 문서에 섹션을 먼저 추가해요.
