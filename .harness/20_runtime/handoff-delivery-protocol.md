# Handoff Delivery Protocol

## 목적
이 문서는 LLM 간 핸드오프를 어떻게 패키징하고 전달하고 수신하는지 정의해요.

## 문서 포지션
- 무엇을 담을 것인가: `handoff-contract.md`
- 어떤 구조인가: `.ai/templates/work-order-schema.md`
- 어떻게 전달하는가: 이 문서 (`handoff-delivery-protocol.md`)

## 전달 절차

### Phase A: Plan -> Work 전달

```text
1. Plan LLM이 work-order.md + work-order.json 동쌍 생성
   - 저장 위치: .ai/handoffs/YYYY-MM-DD_slug/
   - 스키마 기준: .ai/templates/work-order-schema.md

2. handoff-auditor 서브에이전트 검증
   - 필수 필드 확인: schema_version, git_state, batches[].files, acceptance_criteria, verification_commands
   - FAIL -> 누락 필드 보완 후 재검증 (무제한 재시도, rework_count 미소진)
   - PASS -> 다음 단계 진행

3. Work LLM에 전달 프롬프트 생성
   형식:
   /work-start
   핸드오프: {project}/.ai/handoffs/YYYY-MM-DD_slug/work-order.md
   참고: [1줄 작업 요약]

4. Plan LLM STOP - Work 단계 진입 금지
```

### Phase B: Work -> Verify 전달

```text
1. Work LLM이 구현 완료 후 verify-handoff.md 생성
   - 저장 위치: .ai/handoffs/YYYY-MM-DD_slug/verify-handoff.md
   - 포함 내용: 구현 완료 파일 목록, 실행 증빙, verification_commands 결과

2. Verify LLM이 핸드오프 수신
   - work-order.json의 acceptance_criteria 기준으로 검증
   - INTEGRATION_GATE.md 체크리스트 실행

3. gate_result 기록
   - PASS: score_breakdown 채우고 runs.jsonl에 기록 -> FinalCheck 전달
   - FAIL: rework_count++ (기능/로직 FAIL만) -> Work LLM에 재작업 범위 전달
```

### Phase C: Verify -> FinalCheck 전달

```text
1. Verify PASS 후 Verify LLM이 FinalCheck 핸드오프 생성
   - 포함 내용: gate 결과, score_breakdown, Evidence 배열

2. FinalCheck LLM (보통 Plan LLM과 동일)이 수신
   - /check-gate 또는 check-gate 스킬로 최종 판정
   - PASS -> 커밋 프롬프트 생성 후 STOP (커밋 직접 실행 금지)
   - FAIL -> rework_count++ -> 사용자 에스컬레이션 (3회 초과 시)
```

## rework_count 관리 규칙

| FAIL 유형 | rework_count 처리 |
|---|---|
| 기능/로직 FAIL (check-gate) | +1 소진 |
| 형식 오류 (handoff-auditor) | 소진 안 함, 무제한 재시도 |
| 환경/외부 오인 | 소진 안 함, rework.md에 사유 기록 |
| rework_count >= 3 | 자동 중단 + 사용자 에스컬레이션 |

## 핸드오프 디렉터리 구조 예시

```text
.ai/handoffs/2026-05-16_my-task/
├── work-order.md       <- Plan이 생성 (Markdown 사람용)
├── work-order.json     <- Plan이 생성 (기계 읽기용 SSOT)
└── verify-handoff.md   <- Work가 생성 (완료 증빙)
```

## 관련 문서
- 핸드오프 정책: `.harness/20_runtime/handoff-contract.md`
- 스키마 SSOT: `.ai/templates/work-order-schema.md`
- 스킬: `.agents/skills/root-handoff-packager/SKILL.md`
- 게이트 기준: `.ai/gates/INTEGRATION_GATE.md`
