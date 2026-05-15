<!-- harness-template
source: .harness/60_templates/artifacts/detail-plan-backend.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: detail-plan-backend
task_id: "TASK_ID_PLACEHOLDER"
created_at: "YYYY-MM-DD HH:MM"
author: "Codex"
schema_version: 2026-04-v1
---

# 백엔드 상세 계획

## 작업 목표 요약
(1~2줄)

## 수정 파일 목록

| # | 파일 경로 | 변경 내용 |
|---|-----------|---------|
| 1 | | |

## 작업 단계 (직렬/병렬 명시)

> 판단 기준:
> - 작업 B가 A 결과물에 의존 → 직렬
> - 서로 다른 파일/도메인에서 독립 → 병렬 (sub-agent)
> - 공유 상태(env, DB, 전역 설정) 동시 수정 → 직렬
> - 불확실하면 → 직렬 (안전 우선)

| # | 작업 | 처리 방식 | 사용 에이전트/스킬 | 의존 단계 |
|---|------|----------|-----------------|---------|
| 1 | | 직렬/병렬 | | - |

## 비목표 (건드리지 않을 것)
-

## 완료 기준
- [ ]
- [ ] pnpm typecheck exit 0
- [ ] pnpm test --run exit 0
- [ ] pnpm lint exit 0

## ESM/Hono 주의사항 체크리스트 (작업 전 확인)
- [ ] 상대 import에 `.js` 확장자 있음
- [ ] `respond.ok<T>()` 제네릭 명시
- [ ] 서비스 레이어에 Context 없음
- [ ] Zod 검증 적용
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

