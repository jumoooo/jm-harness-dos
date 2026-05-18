# .ai 공유 작업면

## 역할
`.ai/`는 AI-agnostic 공유 작업면이에요.
LLM 종류에 관계없이 공유 산출물을 관리하는 곳이에요.

## 하위 폴더

| 경로 | 역할 |
|---|---|
| `harness/` | 런 기록, KPI, 운영 가이드 |
| `gates/` | 통합 게이트 기준 |
| `handoffs/` | Work Order MD+JSON 동쌍 |
| `plans/drafts/` | 계획 초안 |
| `tasks/` | 태스크 SoT |
| `templates/` | 공유 템플릿 |

## 네이밍 규칙
- 핸드오프 폴더: `YYYY-MM-DD_slug/`
- 계획 문서: `YYYY-MM-DD_topic.md`
- 태스크 폴더: `.ai/tasks/<task_id>/`

## 운영 원칙
- Multi-LLM 환경에서 `.ai/`는 중립 지점이에요.
- 사람과 여러 LLM이 같은 산출물을 함께 읽고 이어받는 기준면이에요.
- 정책 서술은 `.harness/`를 따르고, 실행 산출물은 `.ai/`에 쌓아요.
