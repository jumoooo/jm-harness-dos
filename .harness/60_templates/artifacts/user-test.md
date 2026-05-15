<!-- harness-template
source: .harness/60_templates/artifacts/user-test.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: user-test
task_id: "TASK_ID_PLACEHOLDER"
created_at: "YYYY-MM-DD HH:MM"
author: "Claude"
schema_version: 2026-04-v1
---

# 사용자 테스트 체크리스트

> 이 체크리스트를 순서대로 따라하면 됩니다.
> 모르는 용어는 각 항목 설명에 있습니다.

## 테스트 환경 준비
- [ ] 로컬 서버 실행: `pnpm dev` (frontend: localhost:3000, backend: localhost:4000)

## 기능 테스트

| # | 테스트 항목 | 방법 | 기대 결과 | 결과 (O/X) |
|---|------------|------|---------|----------|
| 1 | | | | |

## 예외 케이스 확인
| # | 시나리오 | 기대 결과 | 결과 (O/X) |
|---|---------|---------|----------|
| 1 | | | |

## 테스트 완료 후
- 이상 없으면: 사용자가 Claude에게 "합격" 또는 "PR 진행해줘" 입력
- 이슈 있으면: 구체적으로 어떤 항목이 어떻게 다른지 Claude에게 전달
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->

