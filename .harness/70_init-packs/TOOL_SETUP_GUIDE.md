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
name = "{codex_model}"

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
## Placeholder 일괄 치환 방법

이 킷의 `{...}` placeholder는 프로젝트 설치 후 실제 값으로 교체해야 한다.

**PowerShell (Windows):**
```powershell
# 예시: {project_name}을 my-project로 치환
Get-ChildItem -Recurse -Filter "*.md" | ForEach-Object {
    (Get-Content $_.FullName) -replace '\{project_name\}', 'my-project' | Set-Content $_.FullName
}
```

**bash (Unix/Mac):**
```bash
# 예시: {project_name}을 my-project로 치환
find . -name "*.md" -exec sed -i 's/{project_name}/my-project/g' {} +
```

**주요 placeholder 목록:**

| Placeholder | 설명 | 예시 |
|-------------|------|------|
| `{project_name}` | 프로젝트 이름 | `my-service` |
| `{project_root}` | 프로젝트 루트 상대경로 | `my-service` |
| `{plan_llm_model}` | Plan 단계 LLM 모델명 | `claude-sonnet-4-6` |
| `{work_llm_model}` | Work 단계 LLM 모델명 | `gpt-5.4` |
| `{model_name}` | 범용 모델명 | `claude-sonnet-4-6` |
| `{codex_model}` | Codex / Work LLM 모델명 | `gpt-5.4` |
