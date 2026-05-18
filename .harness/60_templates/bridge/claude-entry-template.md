# {프로젝트명} - Claude Bridge Guide

## 목적
이 문서는 Claude가 {프로젝트명}에 들어올 때 보는 bridge 문서입니다.
하네스 표준은 `.harness/`에, Claude 전용 실행 계층은 `.claude/`에 있습니다.

## Claude 진입 표준 순서
1. 이 문서 (`CLAUDE.md`)
2. `.harness/70_init-packs/claude/README.md`
3. `.harness/70_init-packs/COMMON_INIT_SEQUENCE.md`
4. `.claude/README.md`

## 진입 판정
- `.claude/`와 `CLAUDE.md`가 있으면 -> retrofit 모드
- 없으면 -> bootstrap 모드 (`.harness/70_init-packs/claude/BUILD_SEQUENCE.md` 따름)

## 세션 모드 (시작 시 반드시 선언)
- `ROOT_ORCH`: 통합 계획, 크로스 패키지 핸드오프, 게이트 검증
- `PKG_LOCAL`: 단일 패키지 구현

## 역할 경계 (`single_llm_mode: false` 기준)
| 단계 | Claude | Work LLM |
|---|---|---|
| Plan | ✅ 담당 | - |
| Work | ⛔ 금지 | ✅ 담당 |
| Verify | ⛔ 금지 | ✅ 담당 |
| FinalCheck | ✅ 담당 | - |

## Guardian / Profile 참조
- guardian 설치 기준: `.harness/10_bootstrap/guardian-installation-strategy.md`
- 전역 프로필: `.claude/profiles/global-runtime-profile.md`

## 커스텀 커맨드 (`.claude/commands/`)
| 커맨드 | 설명 |
|---|---|
| `/cm_run` | Plan 진입점 |
| `/check-gate` | 게이트 검증 |
| `{추가 커맨드}` | `{설명}` |

## 구현 전 승인 필수
- 의견/분석 요청은 구현 지시가 아니에요.
- Edit/Write 허용 트리거는 `해줘`, `수정해줘`, `적용해줘` 같은 명시적 지시만 인정해요.
- 의견 제공 후에는 반드시 `적용할까요?`를 확인하고 STOP해요.

## 참조 경로
- 통합 게이트: `.ai/gates/INTEGRATION_GATE.md`
- 태스크 인덱스: `.ai/tasks/index.md`
- 핸드오프 위치: `.ai/handoffs/`
