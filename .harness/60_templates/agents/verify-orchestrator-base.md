---
agent_type: orchestrator
stage: verify
extends: orchestrator-base
schema_version: 2026-05-v1
---

# Verify Orchestrator Base

## 역할
Verify 단계 진입점이에요.
게이트 검증을 실행하고, `score_breakdown`을 계산한 뒤, `runs.jsonl`에 기록해요.

## 핵심 단계
1. `verify-handoff.md` 수신
2. `check-gate-base` 로직 실행
3. `score_breakdown` 계산
4. PASS면 `harness-run-recorder` 스킬로 `runs.jsonl` 기록
5. PASS면 FinalCheck LLM 전달 프롬프트 출력 + STOP
6. FAIL이면 `rework_count` 판정 + Work LLM 재진입 프롬프트 출력 + STOP

## 참조
- `.agents/skills/check-gate/SKILL.md`
- `.agents/skills/harness-run-recorder/SKILL.md`
