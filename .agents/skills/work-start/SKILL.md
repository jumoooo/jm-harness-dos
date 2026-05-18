# work-start 스킬

## 목적
Plan LLM이 생성한 핸드오프(work-order.md + work-order.json)를 수신하고
7단계 프로토콜에 따라 Work 단계를 시작한다.

## 트리거
입력: 핸드오프 파일 경로 (work-order.md)
형식: "/work-start 핸드오프: {project}/.ai/handoffs/{slug}/work-order.md"

## 실행 프로토콜 (7단계)

### Step 1: 핸드오프 파일 파싱
- work-order.md + work-order.json 동쌍 존재 확인
- schema_version 확인
- batches, acceptance_criteria, verification_commands 필드 존재 확인
- 누락 필드 있으면 BLOCK + 보고

### Step 2: git_state 검증 및 브랜치 준비
- expected_branch 확인
- pre_work_steps 순서대로 실행
- git status --short 로 clean state 확인
- 예상 브랜치와 다르면 BLOCK + 사유 보고

### Step 3: Scope 선언
- batches[].files 에서 허용 경로 목록 추출
- {llm_local}/state/active-handoff.json 에 기록:
  { "handoff_id": "...", "allowed_paths": [...], "started_at": "..." }
- 이 파일이 있는 동안 scope enforcement 적용

### Step 4: Batches 순서대로 실행
batches 배열을 index 0부터 순서대로:
  4-1. 해당 batch의 files 목록만 수정 (allowed_paths 외 수정 금지)
  4-2. 변경 전 git diff --staged 확인
  4-3. frontend/, backend/ 경로 포함 여부 재확인 (ops repo 시)
  4-4. 커밋 메시지 형식 확인 (Conventional Commits)
  4-5. git add {files} → git commit -m "{message}"

### Step 5: 각 Batch Evidence 기록
각 커밋 완료 후:
  evidence.push({ command: "git commit", exit_code: 0, summary: "커밋 내용 1줄" })

### Step 6: Acceptance Criteria 검증
acceptance_criteria 전 항목:
  verification_commands 실행 → exit_code 확인
  각 항목 PASS/FAIL 기록

### Step 7: Verify 핸드오프 생성
파일: .ai/handoffs/YYYY-MM-DD_work-to-verify/work-order.md + .json
포함 내용:
  - 완료된 커밋 목록 (git log 기반)
  - Evidence 배열
  - acceptance_criteria 항목별 결과
  - rework_count 현재값

## BLOCK 조건
다음 중 하나라도 해당하면 즉시 STOP + 사유 보고:
  - work-order.json 필수 필드 누락
  - 현재 브랜치 ≠ expected_branch
  - frontend/ 또는 backend/ 경로 파일 포함 (ops repo 시)
  - acceptance_criteria 미충족 상태로 Step 7 진행 불가

## rework 처리
  check-gate FAIL → rework_count++
  rework_count >= 3 → "재작업 3회 초과 — 사용자 에스컬레이션" STOP
  외부 환경 원인 → count 제외, rework.md 사유 기록 후 재시도

## 참조
- 핸드오프 형식: .harness/20_runtime/handoff-contract.md
- 채점 기준: .harness/50_scoring/scoring-framework.md
- 재작업 정책: .harness/20_runtime/plan-work-verify-runtime.md
