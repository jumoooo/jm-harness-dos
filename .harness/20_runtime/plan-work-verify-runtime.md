# Plan Work Verify Runtime

## 역할 정의

- Plan
  - 깊은 판단, 범위 확정, 리스크 식별, 질문
- Work
  - 결정된 범위를 집행
- Verify
  - 증빙 기반 검증과 PASS 또는 FAIL 판정

## 핵심 규칙

- Plan은 decision-complete 상태를 목표로 해요.
- Work는 큰 설계 판단 없이 집행 가능한 상태를 전제로 해요.
- Verify는 근거 없는 PASS를 금지해요.
- Work 단계에서 고위험 애매성이 생기면 즉시 중단하고 상위에 보고해요.

## 재작업 정책

- Final Check FAIL → 즉시 Codex 재작업 진입. 기회/허용 개념 없음. FAIL이면 바로 돌려보낸다.
- `rework_count`는 누적 카운터다. FAIL마다 1씩 올라간다. "3회 허용"이 아니라 "3회 누적 시 에스컬레이션"이다.
- `rework_count`가 3이 되면 즉시 자동 중단 후 사용자에게 에스컬레이션한다.
  - 에스컬레이션 메시지: "재작업 3회 누적 — 구조적 문제일 수 있습니다. 사용자 확인이 필요합니다."
- `check-gate`에서 기능 또는 로직 FAIL이 나면 재작업 1회를 소진한다.
- 외부 요인(인프라, 환경변수 미설정)으로 인한 재작업은 카운트에서 제외하고 `rework.md`에 사유를 기록한다.
- 환경 또는 형식 오류(Lint, Build 환경, 파일 누락 등)는 외부 요인과 동일하게 카운트에서 제외한다.
- 카운트 제외 항목은 `rework.md`에 `external_cause: true`와 함께 사유를 기록한다.
- 재작업 카운트는 `closeout.md`의 `rework_count` 필드로 추적한다.

## Work 단계 guardian 개입 기준

guardian 에이전트 4종은 Work 단계에서 아래 조건이 감지되면 개입한다.

| guardian | 개입 조건 | 처리 방식 |
|---------|---------|---------|
| `scope-referee` | detail-plan에 없는 파일·모듈 수정 시도 | 즉각 중단 → 사용자 보고 |
| `policy-critic` | non-negotiable 위반 (ESM `.js` 누락, `respond.ok<T>` 제네릭 누락, `any` 타입 사용, Context 서비스 전달 등) | 즉각 중단 → 구체적 위반 항목 명시 |
| `harness-guardian` | Work 단계에서 Plan·Verify 범위 작업 시도 (단계 오염) | 즉각 중단 → 단계 이탈 경고 |
| `handoff-auditor` | closeout.md 필수 섹션 누락 (evidence 링크, rework_count, 검증 결과) | 경고 → closeout 보완 후 계속 |

즉각 중단 시:
- 현재 작업을 중단하고 사용자에게 경고 메시지를 제공한다.
- 어떤 guardian이, 어떤 파일에서, 어떤 규칙을 위반했는지 명시한다.
- 사용자 승인 없이 재개하지 않는다.
