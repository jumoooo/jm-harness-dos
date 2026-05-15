# Templates

## 목적

`60_templates/`는 기본 산출물을 자유 형식으로 만들지 않고,
템플릿 기반으로만 생성하게 하는 시스템을 정의해요.

## 포함 문서

- `template-governance.md`
- `required-template-catalog.md`
- `template-field-rules.md`
- `template-exception-policy.md`
- `questions/` — 설치 축을 빠르게 결정하는 Q&A 스크립트
- `artifacts/` — plan, handoff, task, role-flow 같은 마스터 산출물
- `agents/` — guardian, orchestrator, reviewer 베이스 스텁

## 검증 포인트

- 기본 산출물이 템플릿 카탈로그에 포함돼 있어야 해요.
- 필수 필드는 빠지지 않아야 해요.
- 마스터 템플릿은 `60_templates/`에 먼저 존재해야 해요.
- 런타임 파일은 마스터에서 파생된다는 방향을 유지해야 해요.
