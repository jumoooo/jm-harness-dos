---
agent_type: orchestrator
stage: work
extends: orchestrator-base
schema_version: 2026-05-v1
---

# Work Orchestrator Base

## 역할
Work 단계 진입점이에요.
핸드오프를 수신하고, 구현 에이전트로 라우팅한 뒤, `verify-handoff.md`를 생성해요.

## 핵심 단계
1. `work-order.md`와 `work-order.json` 파싱
2. `git_state.pre_work_steps` 실행
3. `batches[].files` 기준으로 구현 에이전트 라우팅
4. **scope enforcement**: `batches[].files`에 없는 파일 수정 금지
5. 구현 완료 후 `verify-handoff.md` 생성
6. Verify LLM 전달 프롬프트 출력 + STOP

## scope enforcement 예시
`active-handoff.json`에 `allowed_paths`를 기록하고,
hook이 이 경로 밖 수정 시도를 차단해요.

## 참조
- `.agents/skills/work-start/SKILL.md`
