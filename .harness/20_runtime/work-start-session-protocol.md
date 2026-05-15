# Work 단계 세션 시작 프로토콜 (Work-Start Session Protocol)

> 작성일: 2026-05-15
> 분류: 런타임 정책
> 구현 위치: `.agents/skills/work-start/SKILL.md`

## 목적

Codex Work 세션 시작 시 핸드오프 기반 범위를 선언하고,
`pre_tool_policy.js`가 이를 읽어 범위 이탈을 기계적으로 차단하는
3단계 연결 구조를 정의한다.

## 원칙 연결

`plan-work-verify-runtime.md`: "Work 단계에서 고위험 애매성 즉시 중단"
→ 이 프로토콜이 그 원칙의 기계적 구현체.

## 3단계 연결 구조

```
Step 1: work-start 스킬 실행
  .ai/handoffs/ 최신 work-order.json 탐색 → batches[].files 파싱

Step 2: allowed_paths 생성 및 선언
  batches[].files → allowed_paths 목록 생성
  → .codex/state/active-handoff.json 저장

Step 3: 범위 강제 활성화
  pre_tool_policy.js가 active-handoff.json 읽기
  → 편집 대상이 allowed_paths 밖이면 SCOPE BLOCK
```

## 등록 요건

모든 패키지의 `.codex/config.toml`에 work-start 스킬 등록 필수.

| 위치 | 등록 경로 | 상태 |
|-----|---------|-----|
| Root `.codex/config.toml` | `.agents/skills/work-start` | ✅ 등록 완료 |
| Frontend `.codex/config.toml` | `../.agents/skills/work-start` | ✅ 등록 완료 |
| Backend `.codex/config.toml` | `../.agents/skills/work-start` | ✅ 등록 완료 |

## active-handoff.json 최소 필드

```json
{
  "schema_version": "2026-05-v1",
  "handoff_id": "YYYY-MM-DD_slug",
  "allowed_paths": ["경로1", "경로2"],
  "task_summary": "한 줄 작업 요약",
  "created_at": "ISO 8601"
}
```

## 세션 시작 체크리스트

- [ ] work-start 스킬 실행 완료
- [ ] `.codex/state/active-handoff.json` 생성 확인
- [ ] `allowed_paths` 필드에 작업 범위 경로 포함 확인
- [ ] `pre_tool_policy.js` SCOPE BLOCK 활성화 확인

## 주의사항

- `.codex/state/` 디렉토리는 `.gitignore`에 포함 권장 (런타임 상태 파일)
- work-start 미실행 시 active-handoff.json 없음 → SCOPE BLOCK 미작동
- Work 세션 시작 전 반드시 work-start 실행 확인

## 구현 위임

| 항목 | 파일 |
|-----|-----|
| 스킬 정의 | `.agents/skills/work-start/SKILL.md` |
| 범위 강제 훅 | `.codex/hooks/pre_tool_policy.js` |
| 런타임 상태 파일 | `.codex/state/active-handoff.json` |
