# Setup Q&A — Package Structure

## 질문

- 프로젝트 패키지 구조는 무엇인가요?

## 선택지

| 선택 | 구조 설명 | 운영 포인트 |
|---|---|---|
| `Virtual Monorepo A` | 하나의 저장소 안에 여러 패키지 루트가 있어요 | 수동 handoff와 package 경계가 중요해요 |
| `Submodule A'` | 패키지별로 submodule을 써요 | Git 경계와 참조 경로를 엄격히 관리해요 |
| `Single package` | 단일 패키지예요 | 가장 단순하게 시작할 수 있어요 |

## 결정 메모

- 멀티 패키지 구조면 orchestrator가 경계를 먼저 선언해야 해요.
- 단일 패키지면 PKG_LOCAL 흐름으로 단순화할 수 있어요.
