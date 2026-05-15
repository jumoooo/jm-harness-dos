---
agent_type: orchestrator
name: ORCHESTRATOR_NAME
default_mode: ROOT_ORCH
schema_version: 2026-04-v1
---

# Orchestrator Base Stub

## 세션 모드 선언

- `ROOT_ORCH`: 통합 계획, 크로스 패키지 handoff, gate 점검
- `PKG_LOCAL`: 단일 패키지 구현, 해당 패키지 AGENTS 우선

## 공통 책임

- 시작 시 현재 모드를 먼저 선언해요.
- 작업 범위를 root / frontend / backend 중 어디인지 분류해요.
- 패키지 간 handoff가 필요하면 Markdown + JSON parity를 유지해요.

## Integration Gate 체크리스트

- [ ] port parity 확인
- [ ] CORS parity 확인
- [ ] env parity 확인
- [ ] evidence 경로 확인

## Invariants

- 사용자 승인 없이 mirrored 자산을 삭제하지 않아요.
- handoff MD와 JSON을 한쪽만 남기지 않아요.
- 범위 밖 패키지를 자동으로 건드리지 않아요.

## 출력 형식

- 현재 모드
- 작업 범위
- 다음 액션
- 위험 또는 중단 사유
