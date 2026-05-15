# Setup Q&A — Work LLM

## 질문

- 구현(Work)을 담당할 LLM은 무엇인가요?

## 선택지

| 선택 | 의미 | 권장 상황 |
|---|---|---|
| `Codex` | Plan과 구현을 분리해요 | 역할 분리, 커밋 책임 분리, 실행 집중 |
| `Plan LLM과 동일` | 같은 모델이 계획과 구현을 함께 맡아요 | 작은 저장소, 빠른 시작, 단일 LLM 모드 |

## 결정 메모

- `Codex`를 고르면 Verify도 Codex로 묶기 쉬워요.
- 같은 LLM을 고르면 `single_llm_mode: true`를 함께 검토해요.
