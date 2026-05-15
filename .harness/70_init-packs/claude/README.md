# Claude Init Pack

## 목적

이 pack은 Claude 환경에 하네스를 init할 때 읽는 진입 README예요.

## Claude 진입 표준 순서

전체 표준 순서는 아래예요.

1. `{project_root}/CLAUDE.md`
2. 현재 문서: `Claude Init Pack`
3. `{project_root}/.harness/70_init-packs/COMMON_INIT_SEQUENCE.md`
4. `{project_root}/.claude/README.md`

## 이 문서의 역할

- 이 문서는 Claude 전용 init 기준 문서예요.
- 시작 문서는 `CLAUDE.md`예요.
- 다음 읽기 문서는 `COMMON_INIT_SEQUENCE.md`예요.

## 추가 참조 문서

다음 문서들은 필요할 때 이어서 참조해요.

1. `../../10_bootstrap/INIT_ENTRY.md`
2. `../../10_bootstrap/guardian-installation-strategy.md`
3. `../../40_profiles/global-runtime-profile-policy.md`
4. `../../20_runtime/multi-llm-role-policy.md`

## 이 pack의 기본 전제

- Claude는 이미 `.claude/`와 `CLAUDE.md` 같은 로컬 자산이 있을 가능성이 높아요.
- 그래서 새 빈 환경이 아니면 기본적으로 retrofit 가능성을 먼저 점검해요.
- 다만 실제 retrofit 실행은 이 pack이 아니라 다음 단계에서 해요.

## bootstrap 진입

- Claude 로컬 자산이 없거나 하네스 연결이 전혀 없으면 bootstrap으로 들어가요.
- 이때는 하네스 문서 읽기, guardian 연결 준비, 전역 프로필 확정, runtime 인계 준비까지 진행해요.

## retrofit 진입

- `.claude/` 또는 `CLAUDE.md`가 이미 있으면 retrofit으로 들어가요.
- 기존 명령 의미, 에이전트 구조, `.ai/` 참조 계약, contamination 지점을 먼저 정리해요.
- 의미 충돌이 보이면 사용자에게 먼저 보고해요.

## 단일 LLM 모드

- Claude 하나가 plan, work, verify를 모두 맡을 수 있어요.
- 이 경우에도 전역 프로필에서 세 역할 binding을 명시하고 넘어가요.

## 복수 LLM 모드

- Claude를 plan만 맡기거나 verify만 맡기는 식의 분리가 가능해요.
- 이 경우 사용자 확인 없이 역할 분리를 확정하지 않아요.

## guardian 연결

- guardian 표준은 `.harness/10_bootstrap/guardian-installation-strategy.md`를 따라요.
- Claude 로컬 계층에서 실제 호출 가능한 엔트리를 둬야 해요.
- 호출 규칙은 runtime 문서에 연결만 하고, 여기서 새로 만들지 않아요.

## 전역 프로필 적용

- `profile_name`
- `default_plan_llm`
- `default_work_llm`
- `default_verify_llm`
- `artifact_roots`

위 필드를 먼저 확인하고 Claude 역할을 binding해요.

## `.ai/` 참조 규칙

- task-order, handoff, verification artifact가 필요할 때 `.ai/`를 참조해요.
- Claude 로컬 명령, 로컬 agent, guardian 엔트리 판단은 `.claude/`를 우선 봐요.
- `.ai/` 계약을 바꾸는 순간 바로 보고해요.

## 질문과 보고

- 기존 `CLAUDE.md` 의미를 바꾸면 보고해요.
- 기존 `.claude/` 구조를 덮어쓸 수 있으면 보고해요.
- 애매하면 질문하고, unresolved critique가 있으면 중단해요.
