# Harness Context Summary

## 목적

이 문서는 `.harness/` 설계와 retrofit 진행에서 이미 합의된 핵심 맥락만 짧게 묶어두는 context 문서예요.

## 현재 합의

- `.harness/`는 특정 LLM에 종속되지 않는 표준 루트예요.
- 로컬 실행 계층은 각 LLM 디렉토리에서 유지해요.
- shared 산출물 계약은 `.ai/`에서 보되, 로컬 설정 저장소처럼 쓰지 않아요.
- 단일 LLM 모드는 항상 정상 경로예요.
- 복수 LLM 모드는 사용자 확인 후에만 역할을 고정해요.
- guardian은 설치 규칙과 운영 규칙을 분리해 다뤄요.

## retrofit 원칙

- 기존 운영 중인 로컬 자산은 의미 보존이 우선이에요.
- high-impact runtime 변경은 사용자 승인 전에는 진행하지 않아요.
- 문서형 safe retrofit은 기존 의미를 바꾸지 않는 범위에서 먼저 진행할 수 있어요.
- `.ai/`와 기존 `plans/`, `task/`, `.cursor/docs/` 관계는 보수적으로 다뤄요.

## 현재 상태 요약

- Claude는 bridge 문서와 init pack 흐름이 정리돼 있어요.
- Codex는 `.harness -> codex init pack -> .codex runtime bridge` 흐름이 safe retrofit으로 정리됐어요.
- guardian 실제 엔트리 연결과 전역 프로필의 강한 런타임 연동은 다음 단계 판단으로 남아 있어요.
