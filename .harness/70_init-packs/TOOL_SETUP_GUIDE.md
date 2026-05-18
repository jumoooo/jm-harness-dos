# TOOL_SETUP_GUIDE

## Claude Code 환경

```bash
# 설치
npm install -g @anthropic-ai/claude-code

# 확인
claude --version
```

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write|Bash",
        "hooks": [{ "type": "command", "command": "powershell -File .claude/hooks/your-hook.ps1" }]
      }
    ]
  }
}
```

## Codex CLI 환경

```bash
# 설치
npm install -g @openai/codex

# 확인
codex --version
```

```toml
[model]
provider = "openai"
name = "codex-davinci-002"

[approval_policy]
mode = "on-request"
```

## Generic LLM 환경

```text
# CLI 없음 또는 별도 CLI 사용
# .md 파일만으로 동작
# 설정 파일 없음 -> profile.md에 역할/단계 수동 기록
```

## 공통 전제 조건
- [ ] Node.js 18 이상
- [ ] git 2.30 이상
- [ ] PowerShell 5.1 이상 (Windows) 또는 bash (Unix)
- [ ] LLM API 키 (환경 변수로 설정, 절대 커밋 금지)
