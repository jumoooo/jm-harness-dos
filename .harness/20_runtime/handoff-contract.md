# Handoff Contract

> schema_version: 2026-04-v1
> 최종 갱신: 2026-05-08
> 기계 읽기용 스키마 SSOT: `.ai/templates/work-order-schema.md`
> 에이전트는 work-order-schema.md를 먼저 확인할 것.

## 목적

Plan → Work, Work → Verify, Verify → Final Check 간 핸드오프 산출물의 최소 요구 사항을 정의해요.
이 계약을 만족하지 못한 핸드오프는 수신 측에서 Work 시작을 거부할 수 있어요.

---

## 필수 산출물 (MD + JSON 동쌍)

모든 핸드오프는 아래 두 파일을 `.ai/handoffs/YYYY-MM-DD_slug/`에 함께 생성해야 해요.

| 파일 | 역할 |
|------|------|
| `work-order.md` | 사람이 읽는 작업 지시서 |
| `work-order.json` | 기계가 읽는 구조화 산출물 — 스키마 필드 필수 준수 |

스키마 정의: `.ai/templates/work-order-schema.md`

---

## work-order.json 필수 필드

| 필드 | 필수 | 설명 |
|------|------|------|
| `schema_version` | ✅ | `"2026-04-v1"` |
| `handoff_id` | ✅ | `"YYYY-MM-DD_slug"` |
| `task_id` | ✅ | 연결된 태스크 SoT 경로의 ID |
| `from` / `to` | ✅ | 핸드오프 방향 (`claude-plan` → `codex-work` 등) |
| `branch` | ✅ | 작업 대상 브랜치명 |
| `base_ref` | ✅ | 기준 ref (예: `origin/main`) |
| **`git_state`** | ✅ | 아래 참조 |
| `batches[].files` | ✅ | 변경 대상 파일 목록 |
| `acceptance_criteria` | ✅ | 완료 기준 목록 |
| `verification_commands` | ✅ | 검증 명령 목록 |

---

## git_state 필드 (필수)

`git_state`는 수신 측(Codex)이 Work를 시작하기 전에 git 환경을 정확히 재현할 수 있도록 보장해요.

```json
"git_state": {
  "expected_branch": "fix/issue-xxx",
  "base_ref": "origin/main",
  "pre_work_steps": [
    "git fetch origin",
    "git reset --hard origin/main",
    "git status"
  ],
  "expected_clean_after_reset": true,
  "untracked_to_commit": ["경로/"],
  "untracked_skip": [".ai/tmp/"]
}
```

### 수신 측(Codex) 의무

1. `pre_work_steps`를 순서대로 실행
2. `git branch --show-current` 결과가 `expected_branch`와 일치하는지 확인
3. `expected_clean_after_reset: true`이면 `git status --short`로 tracked 변경 없음 확인
4. **상태 불일치 → 구현 시작 전 사용자에게 즉시 보고**
5. 불명확한 git 상태에서 커밋 절대 금지

### 발신 측(Claude) 의무

핸드오프 생성 시:
1. 현재 `git branch --show-current`, `git status --short` 실행해서 실제 상태 확인
2. `git_state` 필드에 Codex가 재현해야 할 상태를 정확히 기술
3. `untracked_to_commit`과 `untracked_skip`을 명시해서 Codex가 추측하지 않도록 함

---

## 금지 사항

- `git_state` 필드 없는 `work-order.json` 생성 금지 (schema_version: 2026-04-v1 이후)
- git 상태 불명확 상태에서 Work 시작 금지
- 텍스트 설명만으로 git 상태 대체 금지 (항상 필드로 명시)
- 핸드오프 없이 Work/Verify 단계 진입 금지

---

## 구현 위임 (Implementation Delegation)

| 항목 | 위임 파일 | 강도 | 상태 |
|-----|---------|-----|-----|
| 핸드오프 생성 워크플로우 | `.claude/commands/cm_run.md` Step 4a | 워크플로우 | ✅ 활성 |
| 핸드오프 검증 (auditor) | `.claude/agents/handoff-auditor.md` | 서브에이전트 | ✅ 활성 (MANDATORY 호출) |
| dirty-state 감지 | `.claude/hooks/check-dirty-state.ps1` | ⚠️ 경고 | ✅ 활성 |
| Work 범위 선언 (Codex) | `.agents/skills/work-start/SKILL.md` | 스킬 | ✅ 활성 (Root/Frontend/Backend 등록 완료) |

**계약 갱신 이력**:
- 2026-05-08: `git_state` 필드 필수화 (이전 핸드오프 13개 소급 적용 제외)

## 관련 파일

| 파일 | 역할 |
|------|------|
| `.ai/templates/work-order-schema.md` | `work-order.json` 전체 스키마 레퍼런스 |
| `.ai/templates/codex-handoff-prompt.md` | Codex 수신 프롬프트 템플릿 |
| `.claude/hooks/check-dirty-state.ps1` | Claude Code 발신 측 dirty-state 자동 경고 훅 |
| `.claude/settings.json` | PreToolUse 훅 등록 |
| `20_runtime/plan-work-verify-runtime.md` | 단계 런타임 전체 규칙 |
