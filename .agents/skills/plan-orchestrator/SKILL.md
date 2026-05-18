# plan-orchestrator 스킬

## 목적
사용자 요청(또는 계획 파일)을 받아 Plan 단계 전체를 실행하고
Work LLM에게 전달할 핸드오프 + 프롬프트를 생성한다.

## 트리거
- /cm_run 또는 동등한 Plan 진입 커맨드
- 입력: 계획 파일 경로 OR 자연어 설명

## 실행 프로토콜 (8단계)

### Step 1: 요청 파싱
- 계획 파일 경로 제공 시: 파일 읽기 → 목표/비목표/수용기준 추출
- 자연어 제공 시: 의도 분류 (SIMPLE/MEDIUM/COMPLEX)
- 범위 불명확 시: Step 2로

### Step 2: scope-referee 호출 (범위 불명확 시)
- 에이전트: .claude/agents/scope-referee.md (또는 LLM 등가)
- 범위 명확해질 때까지 반복
- 범위 확정 후 Step 3

### Step 3: 계획 수립
- 작업 단계 분해 (batches 배열)
- 각 batch: 파일 목록, 커밋 메시지, 변경 유형
- acceptance_criteria 도출
- verification_commands 작성

### Step 4: policy-critic 호출 (Plan 종료 직전)
- 에이전트: .claude/agents/policy-critic.md
- 검토 항목: non-negotiable 위반 여부, 프로젝트 규칙 충돌
- FAIL 시: 계획 수정 후 재호출

### Step 5: harness-guardian 호출 (Plan 완료 직전)
- 에이전트: .claude/agents/harness-guardian.md
- 검토 항목: 하네스 단계 오염 여부, 역할 경계 위반
- FAIL 시: 계획 수정 후 재호출

### Step 6: root-handoff-packager 호출
- 스킬: .agents/skills/root-handoff-packager/SKILL.md
- 출력: .ai/handoffs/YYYY-MM-DD_slug/work-order.md + work-order.json

### Step 7: handoff-auditor 호출 (PASS 필수)
- 에이전트: .claude/agents/handoff-auditor.md
- 검증: schema_version, git_state, batches[].files, acceptance_criteria, verification_commands
- FAIL 시: 누락 필드 보완 후 재검증
- PASS 없이 Step 8 진행 절대 금지

### Step 8: Work LLM 전달 프롬프트 출력
형식 (~~~fence):
~~~
/work-start
핸드오프: {project}/.ai/handoffs/{slug}/work-order.md
참고: {1줄 작업 요약}
~~~
출력 후 STOP (Work 단계 진입 금지)

## 참조
- 핸드오프 전달: .harness/20_runtime/handoff-delivery-protocol.md
- Guardian 설치: .harness/10_bootstrap/guardian-installation-strategy.md
