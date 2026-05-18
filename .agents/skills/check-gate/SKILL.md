# check-gate 스킬

## 목적
Work LLM의 결과 핸드오프를 받아 검증을 실행하고
acceptance_criteria 충족 여부를 판정한다.

## 트리거
입력: Verify 핸드오프 파일 경로 (work-to-verify work-order.md)

## 실행 프로토콜 (7단계)

### Step 1: Verify 핸드오프 파싱
- 커밋 목록, Evidence 배열, acceptance_criteria 추출

### Step 2: verification_commands 전체 실행
- 각 커맨드 순서대로 실행
- { command, exit_code, output_summary } 형식으로 기록

### Step 3: acceptance_criteria 항목별 판정
각 항목:
  - 검증 커맨드 결과와 대조
  - PASS / FAIL 판정
  - FAIL 시 구체적 실패 근거 기록

### Step 4: score_breakdown 계산
.harness/50_scoring/scoring-framework.md 기준:
  {
    "correctness": N,
    "completeness": N,
    "quality": N,
    "process_compliance": N
  }

### Step 5: harness-run-recorder 호출
스킬: .agents/skills/harness-run-recorder/SKILL.md
기록 대상: runs.jsonl

### Step 6: 종합 판정
PASS 조건: acceptance_criteria 전 항목 PASS
FAIL 조건: 1개 이상 FAIL
REWORK 조건: FAIL + rework_count < 3

### Step 7: 결과 핸드오프 생성
PASS 시:
  - PASS 판정 + Evidence 요약 출력
  - 커밋 프롬프트 출력 (Plan LLM용)
FAIL 시:
  - FAIL 항목 목록 + 재작업 범위 명시
  - rework_count++ 후 work-start 재진입 프롬프트 출력

## Evidence 형식
{ "command": "명령어", "exit_code": 0, "summary": "결과 1줄 요약" }

## 참조
- 채점 기준: .harness/50_scoring/scoring-framework.md
- 재작업 정책: .harness/20_runtime/plan-work-verify-runtime.md
