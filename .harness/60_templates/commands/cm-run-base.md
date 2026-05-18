# cm-run-base - Plan 진입점 커맨드 베이스

## 역할
Plan 단계 진입점이에요.
입력을 받아 적절한 orchestrator로 라우팅해요.
Work/Verify 단계 진입은 금지해요. (`single_llm_mode: false` 기준)

## 역할 경계 (구현 시 반드시 강제)
- Plan 단계 허용: 계획 파일 읽기, 핸드오프 생성, `.ai/plans/` 쓰기
- Work/Verify 진입: BLOCK (`single_llm_mode: true` 예외)

## 핵심 단계 (7단계)
1. 입력 파싱 (계획 파일 경로 또는 자연어 설명)
2. 컨텍스트 로드
3. Intent 분류 + 복잡도 평가 (`SIMPLE` / `MEDIUM` / `COMPLEX`)
4. Guardian 호출: `scope-referee`(범위 불명확 시) -> `policy-critic`(Plan 종료 전) -> `harness-guardian`
5. Orchestrator 라우팅
6. 핸드오프 MD+JSON 생성 -> `handoff-auditor` 검증
7. Work LLM 전달 프롬프트 출력 + STOP

## 출력 형식
```text
/work-start
핸드오프: {project}/.ai/handoffs/YYYY-MM-DD_slug/work-order.md
[1줄 작업 요약]
```

## 구현 참조
- 스킬: `.agents/skills/root-handoff-packager/SKILL.md`
- 전달 규칙: `.harness/20_runtime/handoff-delivery-protocol.md`
- 셋업: `.harness/10_bootstrap/setup-guide-plan.md`
