# BUILD_SEQUENCE

## 목적
이 파일은 jm-harness-dos를 새 프로젝트에 설치할 때 전체 순서를 안내해요.

## 전제 조건
- git 초기화 완료
- Node.js 설치 완료
- 대상 LLM CLI 설치 완료

## 전체 순서

### Phase 1 — .harness/ 복사 (기초 구조)
- `jm-harness-dos/.harness/`를 새 프로젝트 루트에 복사해요.
- 프로젝트 고유 변수(`{project_root}`, `{project_name}`)를 일괄 교체해요.
- 확인:
  - PowerShell: `Get-ChildItem .harness | Measure-Object`
  - bash: `find .harness -maxdepth 1 -type d | wc -l`
  - 기대 결과: `.harness/` 아래 9개 폴더 존재

### Phase 2 — .ai/ 워크스페이스 생성
- `jm-harness-dos/.ai/` 디렉터리 구조를 복사해요.
- `runs.jsonl`은 초기 상태로 유지해요.
- 확인:
  - PowerShell: `Test-Path .ai/gates/INTEGRATION_GATE.md`
  - bash: `test -f .ai/gates/INTEGRATION_GATE.md && echo OK`

### Phase 3 — .agents/skills/ 공유 스킬 복사
- `jm-harness-dos/.agents/skills/`를 복사해요.
- LLM 공통 스킬만 먼저 배치해요.
- 확인:
  - PowerShell: `Get-ChildItem .agents/skills -Recurse -Filter SKILL.md`
  - bash: `find .agents/skills -name SKILL.md`
  - 기대 결과: 5개 `SKILL.md` 존재

### Phase 4 — LLM별 로컬 실행 계층 생성
- Claude는 `70_init-packs/claude/BUILD_SEQUENCE.md`를 따라가요.
- Codex는 `70_init-packs/codex/BUILD_SEQUENCE.md`를 따라가요.
- Generic은 `70_init-packs/generic-llm/BUILD_SEQUENCE.md`를 따라가요.
- 확인:
  - PowerShell: `Get-ChildItem -Force .claude,.codex -ErrorAction SilentlyContinue`
  - bash: `ls -a .claude .codex 2>/dev/null`

### Phase 5 — 설정 파일 초기화
- `role-flow.md`와 `role-flow.json`을 생성해요.
- `global-runtime-profile.md`를 각 LLM 전용 로컬 레이어 위치에 생성해요.
- `single_llm_mode`를 결정하고 기록해요.
- 확인:
  - PowerShell: `Get-ChildItem .ai\templates\role-flow.* -ErrorAction SilentlyContinue`
  - bash: `ls .ai/templates/role-flow.* 2>/dev/null`

### Phase 6 — Integration Gate 검증
- `.ai/gates/INTEGRATION_GATE.md` 체크리스트를 실행해요.
- PASS면 초기 `runs.jsonl` 런을 기록해요. (`run_id: RUN-000`, `phase: bootstrap`)
- 확인:
  - PowerShell: `Get-Content .ai/harness/runs.jsonl`
  - bash: `cat .ai/harness/runs.jsonl`

## 문제 발생 시 참조
- `.harness/10_bootstrap/INIT_ENTRY.md`
- `.harness/10_bootstrap/BOOTSTRAP_GUIDE.md`
- `.harness/10_bootstrap/setup-guide-plan.md`
- `.harness/10_bootstrap/setup-guide-work.md`
- `.harness/10_bootstrap/setup-guide-verify.md`
## single_llm_mode: true - 단일 LLM 실행 경로

Plan · Work · Verify를 하나의 LLM이 담당하는 경우,
역할 경계를 **논리적으로 유지하면서** 아래 순서로 진행한다.

### 필수 선언 (각 단계 전)

단계 전환 시 반드시 선언:
```text
현재 역할: [Plan / Work / Verify]
```

### 건너뛸 수 있는 단계

| 단계 | multi_llm 모드 | single_llm 모드 |
|------|---------------|-----------------|
| 핸드오프 MD+JSON 생성 | 필수 | 간소화 가능 (`work-order.json`은 유지) |
| Work LLM 전달 프롬프트 출력 | 필수 | 건너뜀 (바로 Work 단계 진입) |
| Verify LLM 전달 프롬프트 출력 | 필수 | 건너뜀 (바로 Verify 단계 진입) |
| FinalCheck 프롬프트 출력 | 필수 | 건너뜀 (바로 FinalCheck 진입) |

### 유지해야 하는 단계 (single_llm에서도 필수)

- `work-order.json` 작성 (`batches`, `acceptance_criteria`, `verification_commands`)
- scope enforcement: `batches[].files` 외 수정 금지
- `verification_commands` 실행 + exit_code 기록
- `score_breakdown` 계산 + `runs.jsonl` 기록

### 역할별 체크리스트

-> `.harness/70_init-packs/ROLE_GUIDE.md` 참조
