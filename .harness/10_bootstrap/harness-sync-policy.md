# Harness Sync Policy

## 목적

이 문서는 하네스 마스터와 런타임 파생본을
어떤 방향으로 유지할지 고정해요.

## harness-first 원칙

- `.harness/60_templates/`는 마스터예요.
- 런타임 파일은 파생본이에요.
- 예시 파생본:
  - `.ai/templates/`
  - `.ai/handoffs/`
  - `.claude/`, `.codex/`의 런타임 자산 일부
- 파생본만 먼저 고치지 않아요.
- 먼저 마스터를 고치고, 그 다음 파생본을 동기화해요.

## CORE / ENV 경계

### CORE 마커

`<!-- CORE:START --> ... <!-- CORE:END -->`

- 자동 동기화 영역이에요.
- 수동 수정 금지예요.
- 마스터 변경 시 파생본에 덮어써도 되는 영역이에요.

### ENV 마커

`<!-- ENV:START --> ... <!-- ENV:END -->`

- 환경별 커스터마이즈 허용 영역이에요.
- 저장소마다 다른 값을 둘 수 있어요.
- 동기화 시 기존 값을 최대한 보존해요.

## sync 트리거

- 마스터 파일의 `template_version` 숫자가 올라가면
  파생본 동기화가 필요해요.
- 동기화 기본 원칙:
  - CORE 영역: 마스터 기준으로 덮어써요.
  - ENV 영역: 기존 환경 값을 보존해요.

## env-configurable 허용 필드

아래 5개 필드만 ENV 영역에서 바꿔요.

| 필드 | 의미 |
|---|---|
| `model` | 기본 모델 식별자 |
| `project_root` | 프로젝트 루트 절대 경로 |
| `artifact_root` | 공유 산출물 루트 경로 |
| `command_prefix` | 명령 접두어 |
| `llm_role_binding` | Plan / Work / Verify / Final Check 담당 LLM 매핑 |

## 위반 금지

- 파생본의 CORE 영역을 직접 수정하지 않아요.
- 마스터 없이 파생본만 생성하지 않아요.
- ENV 영역 밖에서 `env-configurable` 필드를 정의하지 않아요.
- 파생본을 기준으로 마스터를 역추적하지 않아요.

## 운영 메모

- sync 자동화는 이번 범위가 아니에요.
- 지금 단계에서는 마스터/파생본 경계와 수정 규칙만 분명히 해요.
