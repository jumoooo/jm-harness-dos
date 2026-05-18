# pre-tool-guard-base - 승인 없는 Edit/Write 차단 Hook 베이스

## 목적
사용자가 의견이나 분석만 요청했을 때 LLM이 무단으로 파일을 수정하는 것을 막아요.
"어떻게 생각해?", "확인해봐" 같은 분석 요청에는 Edit/Write를 실행하지 않아요.

## 트리거
- PreToolUse
- 매처: Edit | Write | Bash (또는 LLM별 파일 수정 툴명)

## 핵심 로직

### 입력
- stdin JSON: `tool_name`, `tool_input`, `transcript_path` (있으면)

### 판정 순서
1. `transcript_path`에서 마지막 사용자 메시지를 읽어요.
2. 긍정 응답 패턴을 매칭해요.
   - 긍정: `해줘`, `수정해줘`, `추가해줘`, `적용해줘`, `바꿔줘`, `만들어줘`, `응`, `ㅇㅇ`, `네`, `yes`, `ok`, `go ahead`
   - 부정/분석: `어떻게 생각해?`, `맞아?`, `확인해봐`, `분석해줘`, `왜`
3. 긍정 패턴이 매칭되면 `exit 0`으로 허용해요.
4. 매칭이 없으면 `exit 2`로 차단하고 아래 메시지를 출력해요.
   - `승인 없이 파일을 수정할 수 없습니다. '적용해줘' 또는 '수정해줘'로 명시적으로 요청해 주세요.`

### Fail-open 조건 (`exit 0`)
- `transcript_path` 없음
- JSONL 파싱 실패
- 사용자 메시지 없음

## LLM별 구현 힌트
- Claude Code: PowerShell (`.ps1`) 또는 bash (`.sh`)로 구현하고, stdin JSON 파싱 후 `exit 2`를 반환해요.
  `settings.json`의 PreToolUse에 등록해요.
- Codex: JavaScript (`.js`)로 같은 로직을 구현하고, `config.toml` hooks에 등록해요.

## 관련 정책
- `.harness/20_runtime/approval-enforcement-protocol.md`
