<!-- harness-template
source: .harness/60_templates/artifacts/task-evidence.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
title: "TASK_EVIDENCE_CHECKLIST_TEMPLATE"
schema_version: 2026-04-v1
level: "standard" # light | standard
---

## Evidence 체크리스트 개요

- 작업 크기/리스크에 따라 **경량(light)** / **표준(standard)** 레벨을 선택합니다.
- 아래 템플릿은 **표준(standard)** 기준이며, 경량 레벨에서는 일부 항목을 생략할 수 있습니다.

## 1. 공통 항목 (Light / Standard 공통)

- [ ] `/cm_run` 입력 또는 동등한 실행 계획이 존재한다.
- [ ] `.ai/tasks/<task_id>/todo.md`가 생성되어 있고, 목표/범위/제약/체크리스트가 채워져 있다.
- [ ] 주요 의사결정이 `.ai/tasks/<task_id>/todo.md`의 **의사결정 로그** 섹션에 기록되어 있다.

## 2. Light 레벨 전용 (소규모·저위험)

- [ ] 핵심 명령 1~2개의 실행 결과를 텍스트로 저장했다.
  - 예) 빌드/간단 테스트/포맷터 실행 결과 등
- [ ] 에러가 발생했다면, 원인/수정 여부를 짧게 기록했다.
- [ ] 텍스트 설명만으로 PASS를 주장하지 않고, 최소한 핵심 로그/명령 결과를 남겼다.

## 3. Standard 레벨 전용 (기본값)

- [ ] 최소 1건 이상의 테스트/검증 스텝이 있다.
  - 예) `pnpm test`, `pnpm lint`, `GET /health` 등
- [ ] 테스트/검증 결과가 `.ai/tasks/<task_id>/evidence/`에 파일(또는 스크린샷)로 보존되어 있다.
- [ ] 실패 케이스가 있었다면, 재시도 결과 및 최종 판단을 함께 기록했다.
- [ ] 구현 작업이면 가능하면 아래 파일 규칙을 사용했다.
  - `001_typecheck.txt`
  - `002_lint.txt`
  - `003_test.txt`
  - 필요 시 `004_retry_log.txt`
  - 필요 시 `005_manual_check.txt`
- [ ] evidence 없는 완료 선언을 하지 않았다.

## 4. 검증 책임 (front / back / root)

- [ ] **Frontend/Backend** 관점에서 기능 요구사항이 충족되었는지 확인했다.
- [ ] **Root** 관점에서 구조(SoT, `.ai/` 자산, 게이트 정책)가 깨지지 않았는지 확인했다.
- [ ] 필요한 경우 `/check-gate`를 실행하여, 게이트 통과 여부를 Evidence로 남겼다.
- [ ] 실패/재시도/환경 제약이 있었다면 `todo.md` 의사결정 로그와 evidence 양쪽에 기록했다.

## 5. HITL 전환 관련

- [ ] 동일 작업에 대한 재작업 횟수를 카운트하고 있다.
- [ ] 재작업이 합산 3회를 넘거나, 동일 실패가 2회 연속 발생한 경우 **HITL 전환 여부를 검토했다.**
- [ ] HITL 전환 시, `.ai/tasks/<task_id>/todo.md` 상단 메타 정보에 에스컬레이션 이유와 담당자를 기록했다.
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

