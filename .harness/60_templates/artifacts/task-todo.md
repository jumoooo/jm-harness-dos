<!-- harness-template
source: .harness/60_templates/artifacts/task-todo.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
title: "TASK_TITLE_PLACEHOLDER"
task_id: "YYYYMMDD-HHMM_short-slug"
owner: "OWNER_NAME"
status: "OPEN" # OPEN | IN_PROGRESS | DONE | BLOCKED
level: "standard" # light | standard
schema_version: 2026-04-v1
---

## 작업 목표

- 이 작업을 통해 무엇을 달성할지 1~3줄로 요약해 주세요.

## 범위 (Scope)

- 포함되는 내용:
  - 예) 파일/폴더, 시스템, 기능 이름
- 제외되는 내용:
  - 예) 이번 작업에서 다루지 않을 항목

## 제약 사항 (Constraints)

- 기술/시간/의존성 등 제약 조건을 정리합니다.

## 체크리스트

- [ ] Plan / handoff 준비 완료 (`/gemini-assist-plan`, Work Order, JSON)
- [ ] Codex Work 완료 (구현 + 커밋)
- [ ] Codex Verify 완료 (검증 명령 재실행)
- [ ] Evidence 정리 (`.ai/tasks/<task_id>/evidence/` 채우기)
- [ ] `/check-gate` 실행 및 결과 확인

## 검증 결과 (요약)

- 아래 형식으로 명령별 결과를 누적합니다.
  - `command`: 실행한 명령
  - `exit_code`: 종료 코드
  - `summary`: 핵심 결과 1~2줄
  - `environment_note`: 환경 제약 또는 재시도 여부
- 프론트엔드/백엔드 기능 검증 결과를 간단히 기록합니다.
- Port/CORS/env 등 구조적 게이트 결과는 필요 시 요약만 남기고, 상세 증빙은 `evidence/`에 둡니다.

- 예)
  - `command`: `pnpm typecheck`
  - `exit_code`: `0`
  - `summary`: 타입 오류 없음
  - `environment_note`: 없음

## 의사결정 로그 (SoT 1순위)

> 여기에 **최종 확정된 결정/검증 결과/예외 승인·반려**를 누적합니다.
> 자동/반자동 작성 도구는 이 섹션을 기준으로 write-through 해야 합니다.

- YYYY-MM-DD HH:MM — 결정/검증/예외 내용
- YYYY-MM-DD HH:MM — ...

## 관련 Evidence

- `.ai/tasks/{{task_id}}/evidence/` 폴더 내 주요 파일만 링크/설명합니다.
- task 기반 구현 작업은 가능하면 아래 파일 규칙을 기본값으로 사용합니다.
  - `evidence/001_typecheck.txt` — `typecheck` 또는 동등 검증 결과
  - `evidence/002_lint.txt` — `lint` 또는 정적 분석 결과
  - `evidence/003_test.txt` — `test` 또는 핵심 자동 검증 결과
  - `evidence/004_retry_log.txt` — 실패/재시도 기록 (필요 시)
  - `evidence/005_manual_check.txt` — 수동 점검 기록 (필요 시)
- evidence 없는 완료 선언은 금지합니다.
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

