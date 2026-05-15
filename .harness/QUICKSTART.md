# .harness 빠른 참조 가이드

새 세션에서 `.harness/`를 전부 훑지 않고도 필요한 문서만 바로 열 수 있게 정리해요.

## 반드시 읽어야 하는 ACTIVE 파일

| 상황 | 파일 |
|------|------|
| Claude 새 세션 | `70_init-packs/claude/README.md` |
| 모든 LLM 공통 | `70_init-packs/COMMON_INIT_SEQUENCE.md` |
| Guardian 설치/연결 | `10_bootstrap/guardian-installation-strategy.md` |
| 전역 프로필 정책 | `40_profiles/global-runtime-profile-policy.md` |

## 필요할 때만 읽는 REFERENCE 파일

| 주제 | 위치 |
|------|------|
| foundation 기준 | `00_foundation/` |
| runtime 정책 | `20_runtime/` |
| scoring 기준 | `50_scoring/` |
| 템플릿 기준 | `60_templates/` |
| **핸드오프 계약 (git_state 포함)** | `20_runtime/handoff-contract.md` |
| **work-order.json 스키마** | `.ai/templates/work-order-schema.md` |
| **git dirty-state 훅** | `.claude/hooks/check-dirty-state.ps1` |

## 기본적으로 읽지 않는 ARCHIVE 파일

- `70_init-packs/generic-llm/README.md`

## 전체 분류표

전체 파일 분류는 `ACTIVE_FILES.md`를 봐요.
