# Critique And Resolution Flow

> Plan 단계 구현체: `/plan-reconcile` (.claude/commands/plan-reconcile.md)
> 적용 맥락: Plan 내 Gemini 리뷰 수렴에 사용.
> Work↔Plan 간 재개 시에도 이 문서의 흐름을 따른다.

## 원칙

- 긍정만 하는 협업은 금지해요.
- 지적할 것이 있으면 즉시 지적해요.
- 지적 결과는 산출물로 남겨요.
- unresolved critique가 있으면 다음 단계로 못 가요.

## 상태

- `open`
- `resolved`
- `escalated`
- `accepted-risk`

## 다음 단계 진입 허용 조건

- `resolved`
- 사용자 승인된 `accepted-risk`
- 상위 책임자가 승인한 `escalated`

---

## Critique 실행 방법

### 현재 표준 도구

**Gemini CLI** — `/gemini-assist-plan` 커맨드로 실행.
다른 LLM 또는 사람 리뷰어도 동일한 Reconciliation 형식으로 결과 정리.

### Gemini CLI 폴백 체인

실행 전략을 순서대로 시도한다. 이전 전략 실패(quota/timeout/파싱 오류) 시 다음으로 폴백.

```
1. json-stdin (기본)
   전체 계획을 stdin으로 전달 + --output-format json
   결과: JSON response 필드 파싱

2. json-stdin-plan (fallback)
   동일하나 --approval-mode plan 추가 (tool call 억제)
   결과: 동일 JSON 파싱

3. text-stdin-plan (최후 수단)
   짧은 요약 프롬프트만 전달 + --output-format text
   결과: "제한 모드 요약"으로 기록, 제한 사항 명시
```

폴백 후에도 실패 시: 시도한 전략, 주요 오류, exit code를 계획 파일 "외부 리뷰 → 제한 사항"에 기록.

---

## Reconciliation 표 형식 (필수)

critique 결과는 반드시 아래 형식으로 정리한다. 이유 없는 행 금지.

| 항목 | 외부 의견 요지 | 결정 | 이유 |
|-----|-------------|------|-----|
| (critique 항목명) | (핵심 내용 1~2줄) | 채택/불채택/부분채택 | (프로젝트 맥락 기반 이유 필수) |

**결정 기준**:
- `채택`: 프로젝트 목표·제약과 일치, 구현 비용 낮음
- `부분채택`: 방향은 옳으나 현재 scope 초과 또는 단계적 적용 필요
- `불채택`: 비목표와 충돌, 이미 구현됨, 근거 불충분

**원칙**: 불채택도 이유 필수. "이유 없음" 불채택은 unresolved로 간주.

---

## Phase C 핸드오프 블록 형식

Reconciliation 완료 후 계획 문서 하단에 아래 블록을 추가한다.
블록 없이 /cm_run 실행 전환 금지.

```markdown
## 실행 핸드오프 — Phase C
- **계획 파일**: (경로)
- **목표**: (1~3줄)
- **비목표**: (1~3줄)
- **수용 기준**: (체크 가능 문장 3~7개)
- **불변/위험**: (침범 금지 사항)
- **다음 액션**: planner 선행 / 또는 바로 feature
```

---

## 구현 위임

| 항목 | 파일 |
|-----|-----|
| 실행 커맨드 | `.claude/commands/gemini-assist-plan.md` |
| 계획 저장 위치 | `.ai/plans/drafts/YYYY-MM-DD_topic.md` |
| Reconciliation 후 핸드오프 생성 | `.claude/commands/cm_run.md` Step 4a |
| 임시 파일 | `.ai/tmp/gemini_*` (실행 후 자동 정리) |
