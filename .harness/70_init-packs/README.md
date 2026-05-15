# Init Packs

## 목적

`70_init-packs/`는 특정 LLM이 하네스를 실제로 받아들이기 위한 진입 계층이에요.

각 pack은 무엇을 먼저 읽고,
어떤 순서로 하네스를 녹이고,
어떤 규칙을 반드시 지켜야 하는지 고정해요.

## 포함 구조

- `COMMON_INIT_SEQUENCE.md`
- `claude/README.md`
- `codex/README.md`
- `generic-llm/README.md`

## 공통 원칙

- init-pack은 bootstrap과 retrofit 둘 다 진입할 수 있어야 해요.
- 단일 LLM 모드를 항상 정상 경로로 유지해요.
- 복수 LLM 모드는 지원하지만 사용자 확인 없이 강제하지 않아요.
- guardian 설치는 `10_bootstrap/guardian-installation-strategy.md`를 기준으로 연결해요.
- 전역 프로필은 `40_profiles/global-runtime-profile-policy.md`를 기본으로 읽어요.
- `.ai/`는 공유 산출물 계약이 필요할 때만 참조하고, 로컬 설치 판단은 각 로컬 디렉토리를 우선 봐요.

## 읽기 순서

1. `COMMON_INIT_SEQUENCE.md`
2. 대상 pack의 `README.md`

## 메모

> 🧩 init-pack 구현은 완료돼도, 실제 repo retrofit은 별도 단계에서 진행해요.
