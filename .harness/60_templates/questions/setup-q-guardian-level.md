# Setup Q&A — Guardian Level

## 질문

- guardian 강도를 어느 수준으로 둘까요?

## 선택지

| 선택 | 구성 | 권장 상황 |
|---|---|---|
| `Full` | guardian 4개 전체 | 새 팀, 새 저장소, 엄격한 운영 |
| `Minimal` | `harness-guardian` + `policy-critic` | 기본 안전장치만 유지하고 싶을 때 |
| `None` | guardian 없음 | 실험용, 로컬 샌드박스, 수동 점검 |

## 결정 메모

- Full은 범위·정책·handoff 품질을 가장 안정적으로 지켜줘요.
- Minimal은 비용 대비 효과가 좋아 기본값으로 쓰기 좋아요.
