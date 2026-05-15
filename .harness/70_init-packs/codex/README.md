# Codex Init Pack

## 목적

이 pack은 Codex 환경에 하네스를 init할 때 읽는 진입 README예요.

## Codex 진입 표준 순서

전체 표준 순서는 아래예요.

1. 현재 문서: `Codex Init Pack`
2. `{project_root}/.harness/70_init-packs/COMMON_INIT_SEQUENCE.md`
3. `{project_root}/.codex/README.md`

## 이 문서의 역할

- 이 문서는 Codex 전용 init 기준 문서예요.
- 시작 문서는 현재 문서예요.
- 다음 읽기 문서는 `COMMON_INIT_SEQUENCE.md`예요.

## 추가 참조 문서

다음 문서들은 필요할 때 이어서 참조해요.

1. `../COMMON_INIT_SEQUENCE.md`
2. `../../10_bootstrap/INIT_ENTRY.md`
3. `../../10_bootstrap/guardian-installation-strategy.md`
4. `../../40_profiles/global-runtime-profile-policy.md`
5. `../../20_runtime/multi-llm-role-policy.md`

## 이 pack의 기본 전제

- Codex는 `.harness/`와 함께 `.codex/`, `.agents/` 같은 로컬 계층을 활용할 수 있어요.
- 따라서 로컬 진입과 guardian 연결은 Codex 로컬 계층을 기준으로 판단해요.
- 실제 Codex 로컬 자산 retrofit은 이 단계에서 실행하지 않아요.

## bootstrap 진입

- Codex 로컬 하네스 연결이 없으면 bootstrap으로 들어가요.
- foundation, bootstrap, guardian, profile, runtime 연결 준비를 순서대로 수행해요.

## retrofit 진입

- 기존 Codex 로컬 자산이 이미 있으면 retrofit으로 들어가요.
- 기존 규칙, skill, local config가 하네스 방향성과 충돌하는지 먼저 봐요.
- high-impact 변경이면 사용자 승인 전에는 진행하지 않아요.
- safe retrofit 단계에서는 `.codex/config.toml`, `hooks.json`, `rules/`, `.agents/skills/`의 운영 의미를 바꾸지 않고, 하네스 브리지와 참조 지점만 먼저 닫아요.

## 단일 LLM 모드

- Codex 하나가 모든 역할을 맡아도 괜찮아요.
- 단일 모드여도 전역 프로필을 생략하지 않아요.

## 복수 LLM 모드

- Codex가 work만 맡고 다른 LLM이 plan이나 verify를 맡을 수 있어요.
- 역할 분리는 사용자 확인 후에만 고정해요.

## guardian 연결

- guardian 표준은 `.harness/10_bootstrap/guardian-installation-strategy.md`를 따라요.
- Codex 로컬 계층에서 guardian 호출 엔트리를 둘 수 있어야 해요.
- critique 결과를 산출물로 남길 수 있어야 해요.
- 현재 Codex는 `.codex/agents/` 아래 guardian 계열 agent를 additive 방식으로 두고, 기존 hook 또는 rule 의미는 유지한 채 검토 흐름에서 위임 가능하게 연결해요.

## 전역 프로필 적용

- shared workflow는 전역 프로필 없이 시작하지 않아요.
- Codex 역할 binding과 artifact root를 먼저 확정해요.
- high-impact override는 별도 기록이 필요해요.
- 실제 전역 프로필은 `.codex/profiles/global-runtime-profile.md`에 두고, 기존 config와 hook 의미를 바꾸지 않는 additive 방식으로 유지해요.

## `.ai/` 참조 규칙

- 공유 산출물과 handoff 계약은 `.ai/`를 참조해요.
- Codex 로컬 규칙과 진입 구조는 `.codex/`, `.agents/` 같은 로컬 계층을 우선 봐요.
- `.ai/`를 로컬 설정 저장소처럼 쓰지 않아요.
- 기존 `plans/`, `task/`, `.cursor/docs/` 운영 자산과 충돌 가능성이 있으면 `.ai/`를 실저장 위치로 승격하지 않고, 우선 계약 참조 레이어로만 다뤄요.

## 질문과 보고

- 기존 Codex 로컬 의미를 바꾸는 수정이면 보고해요.
- `.ai/`와 로컬 계층 역할이 섞일 것 같으면 질문해요.
- unresolved critique가 있으면 중단해요.
