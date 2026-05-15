# Harness Completion Checklist

## 목적

이 문서는 `.harness/` 패키지 자체가 완료 정의를 충족하는지 빠르게 점검하는 최상위 체크리스트예요.

## 구조 체크

- [x] `README.md`와 `INDEX.md`가 전체 탐색 경로를 안내해요.
- [x] foundation, bootstrap, runtime, profiles, scoring, templates, init packs, retrofit, acceptance 계층이 분리돼 있어요.
- [x] 각 계층에 목적과 세부 규칙 문서가 있어요.

## 운영 체크

- [x] bootstrap 문서와 runtime 문서가 분리돼 있어요.
- [x] guardian 설치 규칙과 runtime 운영 규칙이 분리돼 있어요.
- [x] 단일 LLM 모드가 정상 경로로 유지돼 있어요.
- [x] 복수 LLM 모드는 사용자 확인 없이 강제하지 않아요.

## retrofit 체크

- [x] repo retrofit 계획과 contamination 보고 지점이 따로 정리돼 있어요.
- [x] Codex는 safe retrofit 기준으로 bridge 문서화가 완료됐어요.
- [x] Claude는 `.harness -> init pack -> runtime bridge` 흐름이 문서화돼 있어요.

## acceptance 체크

- [x] acceptance 기준과 verification 시나리오가 있어요.
- [x] operational validation critique log를 LLM별로 남길 수 있어요.
- [x] final review 체크리스트가 있어요.
