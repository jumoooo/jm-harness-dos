# Harness Setup Wizard

## 진입 전 읽기

- `.harness/00_foundation/README.md`
- `.harness/10_bootstrap/INIT_ENTRY.md`
- `.harness/10_bootstrap/harness-sync-policy.md`

## 설치 축 4개

이 문서는 새 환경에 하네스를 심기 전에
무엇을 먼저 결정해야 하는지 빠르게 정리해요.

### Axis 1. Plan LLM

질문:
- 계획(Plan)을 담당할 LLM은 무엇인가요?

선택지:
- `(a) Claude`
- `(b) Gemini`
- `(c) GPT-4o`
- `(d) 단일 LLM`

해석:
- `(d)`를 고르면 `single_llm_mode: true`를 기본값으로 잡아요.
- `(a)`, `(b)`, `(c)`를 고르면 다중 LLM 운영을 기본값으로 잡아요.

다음 읽기:
- Claude 중심 시작: `.harness/70_init-packs/claude/README.md`
- Codex 중심 시작: `.harness/70_init-packs/codex/README.md`
- 범용 LLM 시작: `.harness/70_init-packs/generic-llm/README.md`

### Axis 2. Work LLM

질문:
- 구현(Work)을 담당할 LLM은 무엇인가요?

선택지:
- `(a) Codex`
- `(b) Plan LLM과 동일`

해석:
- `(a)`는 역할 분리 모드예요.
- `(b)`는 `single_llm_mode: true` 초기값과 가장 잘 맞아요.

추가 메모:
- `global-runtime-profile.md`에서는 기본적으로
  Plan/Final Check와 Work/Verify를 분리해 둘 수 있어요.

### Axis 3. 저장소 상태

질문:
- 현재 저장소는 어떤 상태인가요?

선택지:
- `(a) 새 환경 (bootstrap)`
- `(b) 기존 환경 (retrofit)`

해석:
- `bootstrap`은 하네스 자산이 아직 없는 상태예요.
- `retrofit`은 기존 `.claude/`, `.codex/`, `.ai/` 자산이 이미 있는 상태예요.

다음 읽기:
- 새 환경: `.harness/70_init-packs/COMMON_INIT_SEQUENCE.md`
- 기존 환경: `.harness/10_bootstrap/retrofit-architecture.md`

### Axis 4. 패키지 구조

질문:
- 프로젝트 패키지 구조는 무엇인가요?

선택지:
- `(a) Virtual Monorepo A`
- `(b) Submodule A'`
- `(c) Single package`

해석:
- `Virtual Monorepo A`는 `frontend/`, `backend/` 같은 멀티 루트 구조예요.
- `Submodule A'`는 패키지별 Git submodule 구조예요.
- `Single package`는 단일 앱 또는 단일 패키지 구조예요.

운영 메모:
- `(a)`와 `(b)`는 orchestrator가 패키지 경계를 더 엄격히 다뤄야 해요.
- `(c)`는 단일 orchestrator 흐름으로 단순하게 시작할 수 있어요.

## Guardian 레벨

질문:
- Guardian 강도를 어느 수준으로 둘까요?

선택지:
- `Full` : 4개 guardian 모두 사용
- `Minimal` : `harness-guardian` + `policy-critic`
- `None` : guardian 없이 수동 점검

권장:
- 새 팀 / 새 저장소: `Full`
- 이미 운영 기준이 안정된 저장소: `Minimal`
- 실험용 샌드박스: `None`

## global-runtime-profile 초기값 힌트

### 단일 LLM 모드

- `single_llm_mode: true`
- Plan / Work / Verify / Final Check를 같은 LLM으로 잡아요.

### 다중 LLM 모드

- `single_llm_mode: false`
- 예시:
  - Plan: Claude
  - Work: Codex
  - Verify: Codex
  - Final Check: Claude

## 선택 결과별 빠른 경로

| 상황 | 먼저 읽을 문서 |
|---|---|
| Claude로 계획부터 시작 | `.harness/70_init-packs/claude/README.md` |
| Codex 중심 실행 환경 준비 | `.harness/70_init-packs/codex/README.md` |
| 특정 벤더에 묶이지 않는 범용 시작 | `.harness/70_init-packs/generic-llm/README.md` |
| 기존 환경 보강이 먼저 필요 | `.harness/10_bootstrap/retrofit-architecture.md` |

## 완료 후 다음 액션

1. `.harness/10_bootstrap/harness-sync-policy.md`를 읽어요.
2. 위 4축과 Guardian 레벨을 결정해요.
3. 결정 결과에 맞는 `70_init-packs/<llm>/README.md`로 이동해요.
