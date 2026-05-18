# scope-enforcement-base - Work 단계 Scope 강제 Hook 베이스

## 목적
Work LLM이 `work-order.json`의 `batches[].files`에 없는 파일을 수정하는 것을 막아요.
허용된 범위 밖 파일 수정은 Stage Contamination의 핵심 증상이에요.

## 트리거
- PreToolUse
- 매처: Edit | Write (파일 수정 툴)

## 핵심 로직

### 입력
- stdin JSON: `tool_name`, `tool_input` (`path` 또는 `file_path` 필드)
- 상태 파일: `.codex/state/active-handoff.json` 또는 `.ai/handoffs/.current`

### allowed_paths 확인 순서
1. `active-handoff.json` 또는 현재 `work-order.json`을 읽어요.
2. `batches[].files` 목록을 추출해 `allowed_paths`를 만들어요.
3. 수정 대상 파일 경로를 `allowed_paths`와 비교해요.
4. `allowed_paths`에 있으면 `exit 0`으로 허용해요.
5. 없으면 `exit 2`로 차단하고 아래 메시지를 출력해요.
   - `이 파일은 현재 핸드오프 범위(batches.files)에 없습니다: {path}`

### 예외 허용 경로 (항상 `exit 0`)
- `.ai/handoffs/` (핸드오프 산출물)
- `.ai/plans/` (계획 파일)
- LLM 메모리 파일 (`MEMORY.md` 등)

### Fail-open 조건 (`exit 0`)
- `active-handoff.json` 없음 (Work 단계 밖으로 판단)
- JSON 파싱 실패

## work-start 연결
`work-start` 스킬의 Step 3에서 `active-handoff.json`을 생성하고,
`batches[].files`를 `allowed_paths`로 기록해요.
(`.agents/skills/work-start/SKILL.md` 참조)

## LLM별 구현 힌트
- Claude Code: `.ps1` 또는 `.sh`로 구현하고 `settings.json`에 등록해요.
- Codex: `pre_tool_policy.js`로 구현하고 `config.toml`에 등록해요.

## 관련 정책
- `.harness/20_runtime/work-start-session-protocol.md`
