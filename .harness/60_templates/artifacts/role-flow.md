<!-- harness-template
source: .harness/60_templates/artifacts/role-flow.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
# Multi-LLM Role Flow — {project_name}

schema_version: 2026-04-v1

## 구성 요약

| 단계 | 담당 LLM | 모델 | 진입 자산 |
|------|---------|------|----------|
| Plan | Claude | {plan_llm_model} | `/cm_run` → Plan 단계 |
| Work | Codex | {work_llm_model} | 핸드오프 수신 → `.codex/agents/` |
| Verify | Codex | {work_llm_model} | `.codex/agents/reviewer.toml` + guardian agents |
| Final Check | Claude | {plan_llm_model} | 핸드오프 수신 → `/check-gate` |

## 흐름 다이어그램

```
Claude (Plan)
  └─ Plan 산출물 생성 (.ai/plans/drafts/)
  └─ [root-handoff-packager] → .ai/handoffs/YYYY-MM-DD_plan-to-codex/
       └─ Codex (Work)
            └─ 구현 실행 (.codex/agents/)
            └─ Codex (Verify)
                 └─ reviewer + guardian 검토
                 └─ [root-handoff-packager] → .ai/handoffs/YYYY-MM-DD_codex-to-claude/
                      └─ Claude (Final Check)
                           └─ /check-gate 실행
                           └─ 최종 PASS/FAIL 판정
```

## 순서 정책

- **순차 (sequential)** — 각 단계는 이전 단계 완료 후 시작
- 단계 간 전환: 핸드오프 artifact 생성 + 사용자 명시 또는 자동 트리거
- Plan → Work 전환: Claude가 핸드오프 패키징 후 Codex에 전달
- Verify → Final Check 전환: Codex가 핸드오프 패키징 후 Claude에 전달

## 핸드오프 스킬

핸드오프 artifact는 `root-handoff-packager` 스킬을 사용해요.

- 위치: `.agents/skills/root-handoff-packager/SKILL.md`
- 출력: `.ai/handoffs/YYYY-MM-DD_slug/` (MD + JSON 동쌍)
- 필수 필드: `mode`, `scope`, `handoff`, `verify_log`, `verification`, `evidence`
- schema_version: `2026-04-v1`

## critique 방식

- Plan 완료 전: `policy-critic` (`.claude/agents/policy-critic.md`)
- Work 중 범위 이탈: `scope-referee` (`.codex/agents/scope_referee.toml`)
- Verify 완료 전: `harness-guardian` (`.codex/agents/harness_guardian.toml`)
- Final Check: `harness-guardian` (`.claude/agents/harness-guardian.md`)

## 최종 판단자 / 통합 책임자

- 최종 판단자: **Claude** (Final Check 단계)
- 통합 책임자: **Claude** (`monoOrchestrator` / `cm_run`)

## 충돌 해소 정책

- Work/Verify 단계에서 Codex가 범위 초과 판정 → 핸드오프로 Claude에 에스컬레이션
- Final Check에서 Claude FAIL 판정 → Codex Work 단계 재진입 (최대 2회)
- 2회 연속 FAIL → 사용자 HITL 에스컬레이션

## codex-plugin-cc 사용 정책

`codex-plugin-cc`는 설치 가능하지만 **주 실행면이 아닌 보조 레이어**로만 사용해요.

### 허용 용도 (보조)

- Claude 안에서 즉각적인 2차 코드 리뷰 (`/codex:review`)
- 설계 결정 반박 검토 (`/codex:adversarial-review`)
- Codex 별도 세션으로 넘기기 전 가벼운 탐색 (`/codex:rescue` 단발)
- 작은 버그 조사 위임 (Claude 흐름을 크게 방해하지 않는 범위)

### 금지 용도 (주 실행면 대체 불가)

- Work 단계 전체를 plugin으로 대체
- Verify + evidence 수집을 plugin 결과물로 대체
- `.ai/handoffs/` MD+JSON 핸드오프를 plugin 대화로 대체
- 장기 실행 작업의 SoT를 Claude 대화 상태에 두는 것

### 이유

`codex-plugin-cc`는 독립 실행기가 아닌 **Claude 안에서 Codex를 프록시 호출하는 어댑터**예요.
로컬 Codex CLI와 동일한 설치/인증/config를 공유하지만, 실행면이 Claude 대화에 종속돼요.

별도 Codex CLI/데스크탑 세션은:
- 계획 컨텍스트(Claude)와 실행 컨텍스트(Codex)를 분리
- 실패 원인을 계획 품질 vs 런타임 문제로 명확히 구분
- `.codex/config.toml` sandbox/approval/trust 정책을 온전히 활용
- 하네스 MD+JSON handoff, evidence 수집 흐름과 정합

**메인 실행면: 별도 Codex CLI/데스크탑**
**보조 검토/짧은 위임: codex-plugin-cc**

## 관련 파일

- 이 파일의 JSON 버전: `.ai/role-flow.json`
- Claude 프로필: `.claude/profiles/global-runtime-profile.md`
- Codex 프로필: `.codex/profiles/global-runtime-profile.md`
- 통합 게이트: `.ai/gates/INTEGRATION_GATE.md`
- 핸드오프 스킬: `.agents/skills/root-handoff-packager/SKILL.md`
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->


