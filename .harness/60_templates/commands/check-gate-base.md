# check-gate-base - 통합 게이트 검증 커맨드 베이스

## 역할
Verify/FinalCheck 단계에서 `INTEGRATION_GATE.md` 기준으로 PASS/FAIL을 판정해요.

## 판정 로직
1. `.ai/gates/INTEGRATION_GATE.md` 체크리스트 로드
2. `work-order.json`의 `acceptance_criteria` 로드
3. `verification_commands` 실행 -> `exit_code` 기록
4. 전 항목 ✅이면 PASS / 1개 이상 ❌이면 FAIL

## score_breakdown (PASS 시 필수 계산)
- `correctness`: 최대 30점 (기능 동작 여부)
- `completeness`: 최대 25점 (acceptance_criteria 충족률)
- `compliance`: 최대 25점 (하네스 규칙 준수)
- `efficiency`: 최대 20점 (rework_count 기반 감점)
- `total`: 합계

## Evidence 출력 형식 (필수)
`{ "command": "...", "exit_code": 0, "summary": "..." }`

## FAIL 처리
- 기능/로직 FAIL -> `rework_count++`
- 환경/형식 오류 -> `rework_count` 유지, `rework.md`에 사유 기록
- `rework_count >= 3` -> 자동 중단 + 사용자 에스컬레이션

## 구현 참조
- 게이트 기준: `.ai/gates/INTEGRATION_GATE.md`
- 스코어링: `.harness/50_scoring/scoring-framework.md`
- 스킬: `.agents/skills/check-gate/SKILL.md`
