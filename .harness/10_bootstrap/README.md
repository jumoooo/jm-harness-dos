# Bootstrap

## 목적

`10_bootstrap/`은 하네스를 특정 LLM 환경에 주입하거나,
이미 있는 환경을 하네스 기준으로 보강하는 셋업 계층이에요.

이 폴더는 설치와 진입만 다뤄요.
설치 후 실제 운영 규칙은 아직 `20_runtime/`에서 분리해 정의할 예정이에요.

## 포함 문서

- `INIT_ENTRY.md`
- `BOOTSTRAP_GUIDE.md`
- `bootstrap-architecture.md`
- `retrofit-architecture.md`
- `guardian-installation-strategy.md`
- `contamination-analysis-policy.md`
- `setup-flow-checklist.md`
- `SETUP.md`
- `harness-sync-policy.md`

## 읽기 순서

1. `../00_foundation/README.md`
2. `INIT_ENTRY.md`
3. `BOOTSTRAP_GUIDE.md`
4. `bootstrap-architecture.md`
5. `retrofit-architecture.md`
6. `guardian-installation-strategy.md`
7. `contamination-analysis-policy.md`
8. `setup-flow-checklist.md`
9. `SETUP.md`
10. `harness-sync-policy.md`

## 이 폴더가 답하는 질문

| 질문 | 문서 |
|---|---|
| 어떤 요청이 init 진입 신호인가 | `INIT_ENTRY.md` |
| bootstrap 전체 흐름은 무엇인가 | `BOOTSTRAP_GUIDE.md`, `bootstrap-architecture.md` |
| 기존 환경 보강은 어떻게 구분하는가 | `retrofit-architecture.md` |
| guardian은 어디에 어떤 방식으로 심는가 | `guardian-installation-strategy.md` |
| 오염 가능성은 어떻게 판단하고 보고하는가 | `contamination-analysis-policy.md` |
| 셋업 전 확인 항목은 무엇인가 | `setup-flow-checklist.md` |
| 어떤 축을 먼저 결정해야 하는가 | `SETUP.md` |
| 마스터/파생본 동기화 규칙은 무엇인가 | `harness-sync-policy.md` |

## 금지

- runtime 운영 규칙 상세 구현 금지
- 실제 repo retrofit 착수 금지
- 기존 `.claude/`, `.ai/` 자산 직접 수정 금지

## 메모

> 🧭 이 단계는 "어떻게 심을지"만 정의해요.
> "심은 뒤 어떻게 일할지"는 다음 phase에서 다뤄요.
