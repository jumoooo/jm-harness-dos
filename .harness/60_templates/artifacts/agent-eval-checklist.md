<!-- harness-template
source: .harness/60_templates/artifacts/agent-eval-checklist.md
template_version: 1.0.0
policy: core-locked
-->
<!-- CORE:START -->
---
template: agent-eval-checklist
schema_version: 2026-04-v1
---

# 에이전트 출력 평가 체크리스트

## 사용 방법

Plan 단계 완료 후와 Codex Work 단계 완료 후에 각각 채워요.
평가 결과는 해당 태스크의 `evidence/` 아래에 저장해요.

## [Plan 단계] Claude 출력 평가

| 항목 | 기준 | 결과 |
|------|------|------|
| 목표 명확성 | 무엇을 왜 하는지 한 문장으로 설명 가능해요 | ☐ PASS / ☐ FAIL |
| 범위 경계 | 변경 파일 목록이 명시돼 있어요 | ☐ PASS / ☐ FAIL |
| 수용 기준 | 검증 명령과 기대 결과가 있어요 | ☐ PASS / ☐ FAIL |
| 핸드오프 아티팩트 | MD + JSON 동쌍이 있어요 | ☐ PASS / ☐ FAIL |
| 금지 위반 | Plan 단계에서 제품 코드 수정을 하지 않았어요 | ☐ PASS / ☐ FAIL |

**Plan 평가**: ☐ PASS / ☐ FAIL

## [Work 단계] Codex 출력 평가

| 항목 | 기준 | 결과 |
|------|------|------|
| 계획 준수 | handoff 수용 기준을 모두 구현했어요 | ☐ PASS / ☐ FAIL |
| 타입 안전성 | 필요한 typecheck가 통과해요 | ☐ PASS / ☐ FAIL / ☐ N/A |
| 테스트 통과 | 필요한 test가 통과해요 | ☐ PASS / ☐ FAIL / ☐ N/A |
| 빌드 통과 | 필요한 build가 통과해요 | ☐ PASS / ☐ FAIL / ☐ N/A |
| 범위 이탈 | 계획 외 파일 변경이 없어요 | ☐ PASS / ☐ FAIL |
| 커밋 형식 | Conventional Commits를 지켜요 | ☐ PASS / ☐ FAIL |

**Work 평가**: ☐ PASS / ☐ FAIL

## 평가 메모

```text
날짜:
평가자:
런 ID:
메모:
```
<!-- CORE:END -->
<!-- ENV:START -->
- model: 환경별 기본 모델 식별자를 적어요.
- project_root: 프로젝트 루트 절대 경로를 적어요.
- artifact_root: 공유 산출물 루트 경로를 적어요.
- command_prefix: 커맨드 접두어를 적어요.
- llm_role_binding: Plan/Work/Verify/Final Check 담당 LLM 매핑을 적어요.
<!-- ENV:END -->
