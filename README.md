# jm-harness-dos

> 멀티-LLM 프로젝트 거버넌스 하네스 킷 — Claude + Codex 협업 환경을 위한 운영 표준 모음

## 왜 만들었는가?

AI 도구(Claude, Codex 등)로 개발할 때 가장 자주 생기는 문제가 있어요.

- LLM이 맥락 없이 파일을 마음대로 수정한다
- 계획 단계와 구현 단계가 섞여서 무엇이 완료됐는지 모른다
- 여러 LLM이 같은 코드베이스를 건드릴 때 역할이 불분명하다
- 작업이 끝났는지 판단하는 기준이 없다

**jm-harness-dos**는 이런 문제들을 해결하기 위한 **거버넌스 문서 킷**이에요.

코드가 아니라 문서 묶음이에요. `.harness/` 폴더를 프로젝트 루트에 넣고, LLM 새 세션을 시작할 때 이 문서를 읽게 하면 — LLM이 어떤 역할을 맡고, 어떤 순서로 작업하고, 어디서 멈춰야 하는지를 스스로 파악하게 됩니다.

---

## 핵심 개념

### Plan → Work → Verify 3단계 구조

모든 작업은 이 3단계를 거쳐요.

| 단계 | 하는 일 | 담당 예시 |
|------|---------|---------|
| **Plan** | 무엇을 어떻게 만들지 계획하고 핸드오프 문서 작성 | Claude |
| **Work** | 실제 구현, 커밋 | Codex |
| **Verify** | 완료 기준 충족 여부 검증, 최종 판정 | Claude or Codex |

단계별로 LLM 역할을 나눌 수 있고, 하나의 LLM이 전부 맡아도 돼요.

### 핸드오프 (Handoff)

Plan 단계가 끝나면 LLM이 "무엇을 만들어야 하는지"를 담은 문서를 `.ai/handoffs/` 폴더에 생성해요. 이 문서를 다른 LLM(또는 다음 세션)에 전달하면 맥락 없이도 작업을 이어받을 수 있어요.

### Guardian

작업 도중 정책 위반을 감시하는 에이전트 계층이에요. 예를 들어 "계획도 없이 파일 수정하려 하면 차단"하거나, "핸드오프 문서 형식이 맞는지 검사"하는 역할이에요.

### Harness-first 원칙

> 정책 문서를 먼저 수정하고, 구현체는 나중에 동기화한다.

코드나 설정 파일을 먼저 바꾸면 나중에 문서와 충돌이 생겨요. 반드시 `.harness/` 정책 문서를 먼저 고치고, 그 다음에 실제 파일을 동기화하는 방식을 따릅니다.

---

## 폴더 구조

```
.harness/
│
├── 00_foundation/         하네스의 철학과 불변 원칙
│   ├── philosophy.md      왜 이런 구조를 쓰는가
│   ├── priorities.md      충돌 시 우선순위 기준
│   └── non-negotiables.md 절대 위반 금지 규칙
│
├── 10_bootstrap/          새 프로젝트에 하네스를 처음 설치할 때
│   ├── INIT_ENTRY.md      진입점 — 여기서 시작
│   ├── BOOTSTRAP_GUIDE.md 단계별 설치 가이드
│   └── guardian-installation-strategy.md  Guardian 연결 방법
│
├── 20_runtime/            실제 작업 중 따라야 하는 운영 규칙
│   ├── plan-work-verify-runtime.md  3단계 흐름 상세 규칙
│   ├── handoff-contract.md          핸드오프 문서 형식 계약
│   ├── git-governance-policy.md     브랜치/커밋/PR 정책
│   └── multi-llm-role-policy.md     멀티-LLM 역할 분리 기준
│
├── 40_profiles/           LLM 역할 프로필 정책
│   └── global-runtime-profile-policy.md
│
├── 50_scoring/            작업 품질 점수 체계
│   └── scoring-framework.md
│
├── 60_templates/          핸드오프, 태스크, 재작업 등 산출물 템플릿
│   ├── agents/            에이전트 베이스 템플릿
│   └── artifacts/         handoff.md, task-todo.md, rework.md 등
│
├── 70_init-packs/         LLM별 세션 시작 가이드
│   ├── COMMON_INIT_SEQUENCE.md  모든 LLM 공통 순서
│   ├── claude/README.md         Claude 전용 진입 순서
│   └── codex/README.md          Codex 전용 진입 순서
│
├── ACTIVE_FILES.md        전체 파일 분류 인벤토리
├── CHECKLIST.md           설치 완료 체크리스트
├── INDEX.md               번호 체계 전체 매핑
└── QUICKSTART.md          빠른 참조 가이드
```

---

## 사용 방법

### Step 1 — 프로젝트에 복사

```bash
# .harness/ 폴더를 프로젝트 루트에 복사
cp -r .harness/ {your-project-root}/
```

### Step 2 — 설치 경로 확인

`.harness/10_bootstrap/INIT_ENTRY.md`를 열어서 두 가지 경로 중 하나를 선택해요.

- **bootstrap** — 새 프로젝트, `.claude/`나 `.codex/` 같은 로컬 자산이 없는 경우
- **retrofit** — 기존 프로젝트, 이미 로컬 설정이 있고 하네스를 덧붙이는 경우

### Step 3 — LLM 세션 시작 시 읽게 하기

**Claude** 새 세션을 시작할 때:

```
1. {project_root}/CLAUDE.md
2. .harness/70_init-packs/claude/README.md
3. .harness/70_init-packs/COMMON_INIT_SEQUENCE.md
4. .claude/README.md  (Claude 로컬 브리지 문서)
```

**Codex** 새 세션을 시작할 때:

```
1. .harness/70_init-packs/codex/README.md
2. .harness/70_init-packs/COMMON_INIT_SEQUENCE.md
3. .codex/README.md
```

### Step 4 — 작업 시작

세션 시작 후 LLM이 자동으로 하네스 기준을 따라 작업하게 돼요.

- 계획이 필요하면 `.harness/20_runtime/plan-work-verify-runtime.md` 기준으로 Plan 단계 진행
- 구현 전에 핸드오프 문서(`.harness/60_templates/artifacts/handoff.md`) 생성
- 작업 완료 후 Verify 단계에서 완료 기준 충족 여부 판정

---

## 멀티-LLM 구성 예시

```
Claude  ──→  Plan 단계    (계획 수립, 핸드오프 문서 생성)
                │
                ▼  핸드오프 전달
Codex   ──→  Work 단계    (구현, 커밋, 검증)
                │
                ▼  결과 전달
Claude  ──→  Final Check  (완료 판정, PASS/FAIL)
```

단일 LLM 구성도 지원해요. `40_profiles/global-runtime-profile-policy.md`에서 `single_llm_mode: true`로 설정하면 한 LLM이 모든 단계를 담당합니다.

---

## 어떤 프로젝트에 맞나요?

- Claude Code + Codex를 함께 쓰는 프로젝트
- frontend + backend monorepo 구조 프로젝트
- AI 작업 흐름을 체계화하고 싶은 모든 프로젝트

단순한 스크립트 프로젝트나 혼자서 단순 작업만 할 때는 오버스펙일 수 있어요.

---

## 참고

이 킷은 `server-pulse` 프로젝트에서 실제 운영하며 만들어진 거버넌스 체계를 범용 킷으로 추출한 것입니다.
