# Generic LLM Init Pack

## 목적

이 pack은 특정 도구명이 아직 고정되지 않은 일반 LLM 환경에 하네스를 init할 때 쓰는 기준 pack이에요.

## 먼저 읽을 문서

1. `../COMMON_INIT_SEQUENCE.md`
2. `../../10_bootstrap/INIT_ENTRY.md`
3. `../../10_bootstrap/guardian-installation-strategy.md`
4. `../../40_profiles/global-runtime-profile-policy.md`
5. `../../20_runtime/multi-llm-role-policy.md`

## 이 pack의 기본 전제

- 로컬 디렉토리 이름, 명령 체계, agent 구조가 아직 표준화되지 않았을 수 있어요.
- 그래서 bootstrap과 retrofit 판정을 먼저 하고, 로컬 계층 실체를 파악한 뒤에만 연결해요.

## bootstrap 진입

- 아직 로컬 자산이 거의 없으면 bootstrap으로 들어가요.
- 하네스 문서, guardian 연결 준비, 전역 프로필, runtime 연결 준비를 우선해요.

## retrofit 진입

- 기존 명령, 프롬프트, 메모리, agent 구조가 있으면 retrofit으로 들어가요.
- 덮어쓰기, 이동, 의미 변경 가능성을 먼저 점검해요.
- 충돌이 애매하면 사용자에게 바로 질문해요.

## 단일 LLM 모드

- 하나의 일반 LLM이 모든 역할을 맡아도 괜찮아요.
- 단일 모드에서도 profile binding과 artifact root는 명시해야 해요.

## 복수 LLM 모드

- 여러 LLM을 섞을 수 있지만 기본값은 아니에요.
- 담당 범위, critique 방식, 최종 판단자를 사용자에게 먼저 물어요.

## guardian 연결

- guardian 표준은 `.harness/10_bootstrap/guardian-installation-strategy.md`를 따라요.
- 일반 LLM 로컬 계층에서 호출 가능한 엔트리를 확보해야 해요.
- 호출 타이밍은 runtime에 위임하고, 여기서는 연결 준비만 해요.

## 전역 프로필 적용

- 전역 프로필은 항상 먼저 읽어요.
- 로컬 환경에 맞는 role binding과 artifact root를 확정해요.
- 복수 LLM이면 override 기록을 남겨요.

## `.ai/` 참조 규칙

- 공유 산출물 계약이 필요할 때만 `.ai/`를 참조해요.
- 로컬 설치 구조와 로컬 엔트리는 해당 일반 LLM의 로컬 계층을 우선 봐요.
- `.ai/` 경로 계약이 바뀌면 retrofit 이슈로 올려요.

## 질문과 보고

- 로컬 자산 의미를 바꾸는 순간 보고해요.
- 애매한 로컬 구조는 추측하지 말고 질문해요.
- unresolved critique가 있으면 다음 단계로 가지 않아요.
> **아카이브**: 이 문서는 초기 설계 참고용입니다. 현재 워크플로우와 무관하며 업데이트되지 않습니다.
