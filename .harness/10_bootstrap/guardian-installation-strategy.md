# Guardian Installation Strategy

## 목적

guardian을 특정 LLM 내부에 어떻게 설치하는지 설명해요.

## 핵심 원칙

- guardian은 하네스 소속이지만, 실제 호출 가능한 형태로 각 LLM 내부에 존재해야 해요.
- 설치는 bootstrap 또는 retrofit 단계에서 수행해요.
- 설치 후 호출 규칙은 runtime 문서에서 따로 관리해요.

## 권장 guardian 묶음

- `harness-guardian`
- `policy-critic`
- `scope-referee`
- `handoff-auditor`

## 설치 방식 원칙

| 항목 | 원칙 |
|---|---|
| 소속 | 표준 정의는 `.harness/`가 가져가요 |
| 배치 | 실제 호출 엔트리는 각 LLM 로컬 계층에 둬요 |
| 참조 | foundation과 이후 runtime 규칙을 읽을 수 있어야 해요 |
| 산출물 | critique 결과를 별도 산출물로 남길 수 있어야 해요 |

## 설치 체크

- 해당 LLM이 guardian을 호출할 수 있는지
- guardian이 foundation과 runtime 정책을 참조할 수 있는지
- guardian이 critique 산출물을 남길 수 있는지
- guardian 에이전트 파일에 `extends: guardian-base` YAML front matter가 있는지

## 경계

- 이 문서는 "설치 전략"만 다뤄요.
- 호출 타이밍과 차단 규칙은 runtime phase로 넘겨요.
