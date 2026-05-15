# Harness Root

> **빠른 참조**: [QUICKSTART.md](./QUICKSTART.md) - 어떤 파일을 먼저 읽어야 하는지 빠르게 확인해요.

## 목적

`.harness/`는 특정 LLM에 종속되지 않는 하네스 표준 루트예요.

이 폴더는 하네스 규약, 셋업, 런타임 규칙, 점수제, 템플릿, init pack을 번호 체계로 분리해 관리해요.

## 현재 상태

- 현재 구현된 계층은 아래와 같아요.
  - `00_foundation/`
  - `10_bootstrap/`
  - `20_runtime/`
  - `40_profiles/`
  - `50_scoring/`
  - `60_templates/`
  - `70_init-packs/`

- 위 계층에는 각 번호별 `README.md`와 세부 규칙 문서가 함께 있어요.
- 실제 repo 자산 retrofit은 LLM별로 단계적으로 진행해요.
- 현재 Codex는 safe retrofit 기준의 init bridge 문서화를 시작했고, 기존 로컬 설정 의미 보존을 우선해요.
- Claude 쪽 retrofit 적용은 아직 별도 단계예요.

## 번호 체계 표준

이 저장소의 하네스 표준 번호 체계는 아래 방향을 기본으로 사용해요.

| 경로 | 역할 | 상태 |
|---|---|---|
| `00_foundation/` | 철학, 우선순위, 불변 규칙, 질문 정책, 토큰 효율 원칙 | 구현됨 |
| `10_bootstrap/` | 하네스를 새 환경에 주입하거나 기존 환경을 보강하는 셋업 진입 계층 | 구현됨 |
| `20_runtime/` | 하네스 적용 후 실제 작업 흐름 규칙 | 구현됨 |
| `40_profiles/` | 전역 프로필과 역할 모델 정의 | 구현됨 |
| `50_scoring/` | 카테고리별 점수제와 채택 기준 | 구현됨 |
| `60_templates/` | 기본 산출물 템플릿과 강제 규칙 | 구현됨 |
| `70_init-packs/` | LLM별 init pack 진입 구조 | 구현됨 |

## 읽기 순서

1. `00_foundation/README.md`
2. `10_bootstrap/README.md`
3. `00_foundation/philosophy.md`
4. `00_foundation/priorities.md`
5. `00_foundation/non-negotiables.md`
6. `00_foundation/question-and-escalation-policy.md`
7. `00_foundation/token-efficiency-principles.md`
8. `10_bootstrap/INIT_ENTRY.md`
9. `10_bootstrap/BOOTSTRAP_GUIDE.md`
10. `20_runtime/README.md`
11. `40_profiles/README.md`
12. `50_scoring/README.md`
13. `60_templates/README.md`
14. `70_init-packs/README.md`
15. `CHECKLIST.md`
18. `context-summary.md`

## 경계

- `00_foundation/`에는 bootstrap 절차를 넣지 않아요.
- `00_foundation/`에는 runtime 절차를 넣지 않아요.
- `10_bootstrap/`에는 runtime 운영 세부 규칙을 넣지 않아요.
- `20_runtime/`에는 셋업 절차를 넣지 않아요.
- `.harness/`와 `.ai/`의 역할을 섞지 않아요.

## 메모

> ⚠️ 이후 phase에서도 번호 체계를 유지해요.
> 다만 세부 폴더명은 해당 phase 문서와 충돌하지 않도록 순서대로 확정해요.

## `.ai/` 자유 영역 메모

- `.ai/project-analysis/`는 프로젝트 방향성·제약·상태 분석 문서예요.
- `.ai/issues/`는 GitHub 이슈 로컬 스테이징 영역이에요.
- 두 폴더는 하네스가 직접 스키마를 강제하지 않는 자유 영역이고, 상세 관리 기준은 `.ai/README.md`를 따라요.
