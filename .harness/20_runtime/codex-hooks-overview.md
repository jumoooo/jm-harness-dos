# Codex Hooks 생태계 개요 (Codex Hooks Overview)

> 작성일: 2026-05-15
> 분류: 런타임 정책
> 구현 위치: `.codex/hooks/`

## 목적

`.codex/hooks/`에 구현된 Codex 런타임 훅 시스템의 구조와 역할을
하네스 관점에서 문서화한다.

## 훅 파일 목록

| 훅 파일 | 역할 | 하네스 정책 연결 |
|--------|-----|--------------|
| `pre_tool_policy.js` | 3계층 방어 (파괴적 명령 / 디자인 토큰 / 범위 강제) | N-7, 디자인 토큰 규칙, plan-work-verify-runtime |
| `permission_request_review.js` | 권한 요청 검토 | — |
| `post_tool_review.js` | 도구 실행 후 사후 감사 | — |
| `session_start_context.js` | 세션 시작 시 컨텍스트 주입 | — |
| `stop_continue.js` | 중단/계속 판단 | — |
| `user_prompt_submit.js` | 사용자 프롬프트 제출 전 전처리 | — |

## pre_tool_policy.js 3계층 방어 구조

```
Layer 1: 파괴적 명령 차단
  - git push --force, git reset --hard, git merge main 등
  - 감지 즉시 차단, 사용자 보고

Layer 2: 디자인 토큰 직접 사용 차단
  - bg-[#...], text-[#...] 형식의 직접 색상 값
  - 디자인 토큰 전용 클래스(bg-bg-*, text-fg-*) 사용 강제

Layer 3: Work 단계 범위 이탈 차단 (active-handoff.json 연동)
  - .codex/state/active-handoff.json 읽기
  - allowed_paths 목록과 편집 대상 비교
  - 범위 이탈 시 SCOPE BLOCK + 사용자 보고
```

## work-start → SCOPE BLOCK 연결 구조

```
Codex Work 세션 시작
    ↓
work-start 스킬 실행 (config.toml에 등록돼야 Codex가 발견 가능)
    ↓
.codex/state/active-handoff.json 생성 (allowed_paths 포함)
    ↓
pre_tool_policy.js가 active-handoff.json 읽어 범위 이탈 차단
```

## 패키지별 work-start 등록 현황

| 패키지 | config.toml 등록 | 경로 |
|--------|-----------------|------|
| Root | ✅ 등록 완료 | `.agents/skills/work-start` |
| Frontend | ✅ 등록 완료 | `../.agents/skills/work-start` |
| Backend | ✅ 등록 완료 | `../.agents/skills/work-start` |

## 훅 추가 규칙

새 Codex 훅 추가 시:
1. `.codex/hooks/`에 파일 생성
2. Codex hooks 설정에 등록
3. 이 문서 훅 파일 목록 갱신
4. 관련 하네스 정책 문서 "구현 위임" 섹션 갱신

## 구현 위임

| 항목 | 파일 |
|-----|-----|
| 핵심 범위 강제 훅 | `.codex/hooks/pre_tool_policy.js` |
| 범위 상태 파일 | `.codex/state/active-handoff.json` (세션마다 생성) |
| work-start 스킬 | `.agents/skills/work-start/SKILL.md` |
| Claude 측 대응 훅 | `.claude/hooks/` (Claude Code 전용) |

## 하네스 정책 근거

- `.harness/20_runtime/plan-work-verify-runtime.md` — Work 단계 경계 규칙
- `.harness/20_runtime/handoff-contract.md` — active-handoff.json 스키마
- `.harness/20_runtime/git-governance-policy.md` — N-7 force push 차단
