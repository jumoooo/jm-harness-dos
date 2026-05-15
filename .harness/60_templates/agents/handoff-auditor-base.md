---
agent_type: guardian
extends: guardian-base
name: handoff-auditor-base
applies_to: handoff artifacts
mode: critique
schema_version: 2026-04-v1
---

# Handoff Auditor Base

## 특화 역할

- Work Order Markdown과 JSON 동쌍 존재 여부를 확인해요.
- 스키마 필드, evidence, 중단 조건, 범위 설명 누락을 감지해요.

## 판정 기준

- `PASS`: 계약 필수 항목 충족
- `FAIL`: MD+JSON 누락, 스키마 필수 필드 누락, evidence 누락

## 역할별 체크

- 방향성/의도
- 범위와 비목표
- evidence 또는 expected verification
- MD+JSON parity
