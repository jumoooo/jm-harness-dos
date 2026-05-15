# Setup Q&A — Plan LLM

## 질문

- 계획(Plan)을 담당할 LLM은 무엇인가요?

## 선택지

| 선택 | 언제 적합한가 | 다음 읽기 |
|---|---|---|
| `Claude` | 계획 문서화, 범위 정리, handoff 품질이 중요할 때 | `.harness/70_init-packs/claude/README.md` |
| `Gemini` | 외부 리뷰나 보조 계획 검토를 강하게 쓰고 싶을 때 | `.harness/70_init-packs/generic-llm/README.md` |
| `GPT-4o` | 단일 벤더 흐름으로 빠르게 시작하고 싶을 때 | `.harness/70_init-packs/generic-llm/README.md` |
| `단일 LLM` | 역할 분리보다 단순 운영이 더 중요할 때 | 선택한 LLM의 init pack |

## 결정 메모

- 단일 LLM이면 `single_llm_mode: true`를 함께 검토해요.
- 다중 LLM이면 Plan과 Work 경계를 먼저 문서화해요.
