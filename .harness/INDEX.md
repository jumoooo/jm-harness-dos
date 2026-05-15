# Harness Index

## 최종 번호 매핑

| 번호 | 경로 | 역할 | 상태 |
|---|---|---|---|
| `00` | `00_foundation/` | 하네스 철학, 우선순위, 불변 규칙 | 구현됨 |
| `10` | `10_bootstrap/` | bootstrap, retrofit, guardian 설치 전략 | 구현됨 |
| `20` | `20_runtime/` | plan, work, verify 런타임 규칙 | 구현됨 |
| `40` | `40_profiles/` | 전역 runtime 프로필 정책 | 구현됨 |
| `50` | `50_scoring/` | 점수제와 채택 기준 | 구현됨 |
| `60` | `60_templates/` | 템플릿 카탈로그와 예외 정책 | 구현됨 |
| `70` | `70_init-packs/` | LLM별 init pack 진입 구조 | 구현됨 |

## 읽기 순서

1. `README.md`
2. `00_foundation/README.md`
3. `10_bootstrap/README.md`
4. `20_runtime/README.md`
5. `40_profiles/README.md`
6. `50_scoring/README.md`
7. `60_templates/README.md`
8. `70_init-packs/README.md`
9. `CHECKLIST.md`
12. `context-summary.md`

## 번호 체계 메모

- `70_init-packs/`는 실제 init 진입점 역할을 가지므로 유지해요.
- 이미 만들어진 번호 체계를 흔들지 않는 것을 우선해요.
