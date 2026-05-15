# .harness 활성 파일 목록

> 작성일: 2026-04-30
> 목적: 에이전트가 실제로 읽어야 하는 파일과 보존용 파일을 빠르게 구분해요.

## 분류 기준

- **ACTIVE**: `CLAUDE.md`, `AGENTS.md`, `.claude/`, `.codex/`에서 직접 경로를 참조해요.
- **REFERENCE**: 지금도 정책 또는 템플릿 기준으로 재참조할 수 있어요.
- **ARCHIVE**: 초기 분석, 비교, 회고 성격이 강해서 현재 워크플로우의 기본 읽기 순서에서는 제외해요.

## 루트 파일

| 파일 | 분류 | 비고 |
|------|------|------|
| `README.md` | REFERENCE | 하네스 루트 소개 |
| `INDEX.md` | REFERENCE | 문서 인덱스 |
| `CHECKLIST.md` | REFERENCE | 점검 항목 |
| `context-summary.md` | REFERENCE | 배경 요약 |

## 00_foundation

| 파일 | 분류 | 비고 |
|------|------|------|
| `00_foundation/README.md` | REFERENCE | foundation 진입 문서 |
| `00_foundation/philosophy.md` | REFERENCE | 철학 기준 |
| `00_foundation/priorities.md` | REFERENCE | 우선순위 기준 |
| `00_foundation/non-negotiables.md` | REFERENCE | 불변 규칙 |
| `00_foundation/question-and-escalation-policy.md` | REFERENCE | 질문/에스컬레이션 정책 |
| `00_foundation/token-efficiency-principles.md` | REFERENCE | 토큰 효율 원칙 |

## 10_bootstrap

| 파일 | 분류 | 비고 |
|------|------|------|
| `10_bootstrap/README.md` | REFERENCE | bootstrap 진입 문서 |
| `10_bootstrap/INIT_ENTRY.md` | REFERENCE | 초기 진입 순서 |
| `10_bootstrap/BOOTSTRAP_GUIDE.md` | REFERENCE | bootstrap 상세 가이드 |
| `10_bootstrap/guardian-installation-strategy.md` | ACTIVE | `CLAUDE.md` 직접 참조 |
| `10_bootstrap/bootstrap-architecture.md` | REFERENCE | 구조 설명 |
| `10_bootstrap/contamination-analysis-policy.md` | REFERENCE | 오염 분석 정책 |
| `10_bootstrap/retrofit-architecture.md` | REFERENCE | retrofit 구조 설명 |
| `10_bootstrap/setup-flow-checklist.md` | REFERENCE | 셋업 체크리스트 |

## 20_runtime

| 파일 | 분류 | 비고 |
|------|------|------|
| `20_runtime/README.md` | REFERENCE | runtime 진입 문서 |
| `20_runtime/critique-and-resolution-flow.md` | REFERENCE | critique 흐름 |
| `20_runtime/execution-simplicity-policy.md` | REFERENCE | 실행 단순성 정책 |
| `20_runtime/handoff-contract.md` | **ACTIVE** | handoff 계약 — `git_state` 필드 필수, Work 시작 전 검증 의무 포함 |
| `20_runtime/multi-llm-role-policy.md` | REFERENCE | multi-LLM 역할 정책 |
| `20_runtime/plan-work-verify-runtime.md` | REFERENCE | 단계 런타임 규칙 |

## 40_profiles

| 파일 | 분류 | 비고 |
|------|------|------|
| `40_profiles/README.md` | REFERENCE | profile 진입 문서 |
| `40_profiles/global-runtime-profile-policy.md` | ACTIVE | `CLAUDE.md` 직접 참조 |

## 50_scoring

| 파일 | 분류 | 비고 |
|------|------|------|
| `50_scoring/README.md` | REFERENCE | scoring 진입 문서 |
| `50_scoring/scoring-framework.md` | REFERENCE | scoring 기준 |
| `50_scoring/default-categories.md` | REFERENCE | 기본 카테고리 |
| `50_scoring/decision-thresholds.md` | REFERENCE | 판정 임계값 |
| `50_scoring/bonus-categories-policy.md` | REFERENCE | 보너스 정책 |

## 60_templates

| 파일 | 분류 | 비고 |
|------|------|------|
| `60_templates/README.md` | REFERENCE | template 진입 문서 |
| `60_templates/required-template-catalog.md` | REFERENCE | 필수 템플릿 목록 |
| `60_templates/template-exception-policy.md` | REFERENCE | 예외 정책 |
| `60_templates/template-field-rules.md` | REFERENCE | 필드 규칙 |
| `60_templates/template-governance.md` | REFERENCE | 템플릿 거버넌스 |

## 70_init-packs

| 파일 | 분류 | 비고 |
|------|------|------|
| `70_init-packs/README.md` | REFERENCE | init pack 개요 |
| `70_init-packs/COMMON_INIT_SEQUENCE.md` | ACTIVE | `CLAUDE.md` 직접 참조 |
| `70_init-packs/claude/README.md` | ACTIVE | `CLAUDE.md` 직접 참조 |
| `70_init-packs/codex/README.md` | REFERENCE | Codex 진입 참조 |
| `70_init-packs/generic-llm/README.md` | ARCHIVE | 현재 미사용 범용 init pack |

---

## 교차 시스템 파일 (`.ai/` + `.claude/`) — 하네스 연동 ACTIVE

> 하네스 표준과 직접 연동되는 `.ai/` 및 `.claude/` 파일이에요.
> 핸드오프 계약(`20_runtime/handoff-contract.md`)과 함께 읽어야 해요.

| 파일 | 분류 | 비고 |
|------|------|------|
| `.ai/templates/work-order-schema.md` | **ACTIVE** | `work-order.json` 필드 정의 — `git_state` 스키마 포함 (schema_version: 2026-04-v1) |
| `.ai/templates/codex-handoff-prompt.md` | **ACTIVE** | Codex 핸드오프 프롬프트 템플릿 — Git 상태 검증 섹션 포함 |
| `.claude/hooks/check-dirty-state.ps1` | **ACTIVE** | PreToolUse 훅 — context 전환 전 dirty state 자동 감지 |
| `.claude/settings.json` | **ACTIVE** | Claude Code 프로젝트 훅 등록 파일 |

## 에이전트 템플릿 동기화 체크리스트

새 에이전트 추가 시 반드시 확인:

- [ ] `.harness/60_templates/agents/`에 대응 base 템플릿 존재
- [ ] `.claude/agents/` 파일에 YAML front matter (`extends:` 포함) 존재
- [ ] `.claude/README.md` Agents 목록 업데이트
- [ ] `CLAUDE.md` 관련 섹션 업데이트 (필요 시)
