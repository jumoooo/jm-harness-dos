<!-- harness-template
source: .harness/60_templates/artifacts/codex-handoff-prompt.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: codex-handoff-prompt
schema_version: 2026-04-v1
usage: Claude가 Plan 완료 후 Codex에 전달할 프롬프트 템플릿
---

# Codex 핸드오프 프롬프트 템플릿

## 출력 규칙 (Claude가 반드시 준수)

1. Plan 요약 / 감사 결과 등 설명 텍스트를 **먼저** 출력한다.
2. 설명이 끝난 **맨 마지막**에 아래 구분선 + 복붙용 블록을 단독 출력한다.
3. 복붙용 블록 이후에는 어떤 텍스트도 추가하지 않는다.

## 복붙용 프롬프트 출력 형식

````
---
## 📋 Codex 복붙 프롬프트

> 아래 코드블록 전체를 선택해서 Codex에 붙여넣으세요.

```
## 작업 요청

핸드오프: {{ project_root }}/.ai/handoffs/{{ handoff_id }}/work-order.md
JSON:      {{ project_root }}/.ai/handoffs/{{ handoff_id }}/work-order.json

## 시작 전 필수 확인

- AGENTS.md를 먼저 읽어요.
- task_id가 있으면 구현 전에 아래 두 파일을 읽어요:
  - {{ project_root }}/.ai/tasks/{{ task_id }}/todo.md
  - {{ project_root }}/.ai/tasks/{{ task_id }}/evidence/evidence-checklist.md
- UI / 가시성 / 레이아웃 작업이면 구현 전에 /find-skills로 frontend-design, web-design-guidelines 확인해요.
- (선택) 계획 품질이 의심되면 `/plan-review`를 실행해요.
  - plan_reviewer 에이전트가 점수제(100점)로 검토 → PASS/FAIL 판정.
  - 기본 실행 경로: codex-tdd-playbook.md가 자동 처리.

## 컨텍스트

{{ work-order.md 배경 + 근본 원인 + 수정 방향 3~5줄 }}

## TDD 실행 순서

1. RED:   {{ 실패 재현 명령 + 예상 에러 구체적으로 }}
2. GREEN: {{ 테스트를 통과시키는 최소 변경 }}
3. 검증:  {{ pnpm test --run / pnpm typecheck / pnpm lint }}
(TDD 예외 시: "TDD 예외 — {{ 사유 }}. 기존 테스트 GREEN 확인으로 대체.")

## 수정 파일 목록 (이 목록 외 파일 수정 금지 — SCOPE ENFORCEMENT)

| # | 파일 경로 | 변경 내용 요약 |
|---|-----------|--------------|
{{ 파일별 행 — work-order.json files_to_modify와 1:1 동기화 }}

## 비목표 (건드리지 말 것)

{{ work-order.json non_goals 항목별 - 로 나열 }}

## 완료 기준

{{ work-order.json acceptance_criteria 항목별 - [ ] 로 나열 }}
- [ ] pnpm test --run exit 0
- [ ] pnpm typecheck exit 0
- [ ] pnpm lint exit 0

## Evidence 저장 경로

{{ work-order.json evidence_paths 항목별 나열 }}

## Closeout 완료 조건

구현·커밋만으로 완료가 아니에요. 아래 항목이 모두 충족돼야 완료로 봐요:
- [ ] 구현 + 커밋 완료
- [ ] 검증 명령 재실행 (exit code 확인)
- [ ] .ai/tasks/{{ task_id }}/todo.md 체크리스트 + 검증 결과 섹션 갱신
- [ ] .ai/tasks/{{ task_id }}/todo.md 의사결정 로그에 예외/재시도 기록
- [ ] .ai/tasks/{{ task_id }}/evidence/ 에 evidence 파일 저장
- [ ] Claude /check-gate 즉시 실행 가능한 closeout 요약 준비

## 커밋 요청

브랜치: {{ 브랜치명 }}
형식: <type>(<scope>): <description>
Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>

## 참고 규칙

- TDD: 실패 테스트 먼저 → 최소 구현 → 리팩터 → 재검증 (.codex/rules/codex-tdd-playbook.md)
- SCOPE: 수정 파일 목록 외 파일 편집 시 STOP → "SCOPE QUESTION: <파일> not in handoff"
- frontend: 디자인 토큰 전용(bg-bg-*, text-fg-*), Server Component 기본, 'use client' 최소
- backend: ESM .js 확장자 필수, respond.ok<T>() 제네릭 명시, 서비스에 Context 금지
- 검증 exit 0 없이 완료 선언 금지
```
````

## 채워 넣기 규칙

| 플레이스홀더 | 출처 |
|-------------|------|
| `{{ handoff_id }}` | work-order.json `.handoff_id` |
| `{{ project_root }}` | 절대 경로 또는 `server-pulse` |
| `{{ task_id }}` | work-order.json 연결 task, 없으면 생략 |
| `{{ 컨텍스트 }}` | work-order.md `## 배경` 요약 3~5줄 |
| `{{ TDD RED }}` | 현재 실패 명령 + 예상 에러 구체적으로 (추상 지시 금지) |
| `{{ 수정 파일 목록 }}` | work-order.json `files_to_modify` 1:1 동기화 |
| `{{ 비목표 }}` | work-order.json `non_goals` 1:1 동기화 |
| `{{ 완료 기준 }}` | work-order.json `acceptance_criteria` 1:1 동기화 |
| `{{ Evidence 경로 }}` | work-order.json `evidence_paths` 1:1 동기화 |
| `{{ 브랜치명 }}` | work-order.json `commits[].branch` |
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

