# Common Init Sequence

## 목적

모든 init-pack이 공통으로 따르는 하네스 진입 순서를 정의해요.

## 이 문서의 위치

- 이 문서는 시작 문서가 아니에요.
- 각 LLM 전용 init-pack 문서를 읽은 뒤 확인하는 공통 순서 문서예요.
- 다음 읽기 문서는 현재 대상 LLM의 로컬 runtime bridge 문서예요.
- 아래의 공통 진입 순서는 실제 init 작업에 들어갈 때 참고하는 규칙이에요.

## 표준 진입 패턴

각 LLM은 아래 4단 흐름을 따라요.

1. `.harness/` 또는 루트 진입 문서
2. 대상 LLM init pack `README.md`
3. 현재 문서: `Common Init Sequence`
4. 대상 LLM 로컬 runtime bridge 문서

예시는 아래처럼 읽어요.

| 대상 | 표준 흐름 |
|---|---|
| Claude | `CLAUDE.md` -> `.harness/70_init-packs/claude/README.md` -> 현재 문서 -> `.claude/README.md` |
| Codex | `.harness/70_init-packs/codex/README.md` -> 현재 문서 -> `.codex/README.md` |
| Generic LLM | `.harness/70_init-packs/generic-llm/README.md` -> 현재 문서 -> 대상 로컬 bridge 문서 |

## 공통 진입 규칙

1. `../00_foundation/README.md`
2. `../00_foundation/philosophy.md`
3. `../00_foundation/non-negotiables.md`
4. `../10_bootstrap/INIT_ENTRY.md`
5. `../10_bootstrap/BOOTSTRAP_GUIDE.md`
6. `../10_bootstrap/guardian-installation-strategy.md`
7. `../20_runtime/plan-work-verify-runtime.md`
8. `../20_runtime/critique-and-resolution-flow.md`
9. `../40_profiles/global-runtime-profile-policy.md`
10. `../60_templates/required-template-catalog.md`
11. 대상 pack `README.md`를 기준으로 실제 로컬 계층에 연결해요

## bootstrap 진입

- 새 환경이거나 하네스 표준 연결이 아직 없으면 bootstrap으로 들어가요.
- bootstrap일 때는 하네스 경로, guardian 연결, profile 적용 준비까지 확정해요.

## retrofit 진입

- 기존 LLM 전용 자산이 이미 있으면 retrofit으로 들어가요.
- retrofit일 때는 기존 자산 인벤토리, contamination 위험, 사용자 승인 필요 지점을 먼저 정리해요.

## 단일 LLM 모드

- 하나의 LLM이 plan, work, verify를 모두 맡아도 괜찮아요.
- 이 경우에도 전역 프로필을 반드시 읽고 role binding을 명시해요.

## 복수 LLM 모드

- 복수 LLM 구성이 필요하면 `20_runtime/multi-llm-role-policy.md`를 따라요.
- 각 LLM 담당 범위, 순차 또는 병렬 여부, critique 방식, 최종 판단자, 통합 책임자를 사용자에게 먼저 물어요.

## guardian 연결

- guardian 표준 정의는 `.harness/10_bootstrap/guardian-installation-strategy.md`를 따라요.
- 실제 호출 엔트리는 각 LLM 로컬 계층에 둬요.
- 호출 타이밍과 차단 규칙은 runtime 문서 기준으로 연결만 하고, 여기서 새로 정의하지 않아요.

## 전역 프로필 적용

- `40_profiles/global-runtime-profile-policy.md`의 필수 필드를 먼저 확인해요.
- shared workflow는 전역 프로필 없이 시작하지 않아요.
- 단일 LLM이든 복수 LLM이든 `profile_name`, role binding, artifact root를 먼저 확정해요.

## `.ai/` 참조 규칙

- 공유 산출물 경로, handoff, task-order, verification artifact가 필요할 때만 `.ai/`를 참조해요.
- 로컬 설치 구조, 로컬 명령 진입, 로컬 guardian 배치는 해당 LLM 로컬 디렉토리를 우선 봐요.
- `.ai/` 계약을 바꾸는 순간 retrofit 단계로 승격하고 사용자 보고가 필요해요.

## 질문과 보고 규칙

- 애매하거나 충돌 가능성이 있으면 추측하지 말고 질문해요.
- 기존 의미를 바꾸는 수정이면 사전 보고해요.
- unresolved critique가 있으면 다음 단계로 가지 않아요.
