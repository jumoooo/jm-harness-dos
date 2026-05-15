# jm-harness-dos

> **김준모 하네스 킷** — frontend + backend root 단 멀티-LLM 프로젝트 거버넌스 하네스

## 이게 뭐야?

`.harness/`는 Claude, Codex 등 멀티-LLM 환경에서 프로젝트를 운영할 때 필요한
**정책 / 셋업 / 런타임 규칙 / 템플릿 / init pack**을 하나의 폴더에 묶은 거버넌스 킷이에요.

특정 LLM에 종속되지 않아요. 어떤 프로젝트 루트에 `.harness/` 폴더째로 복사하면
그대로 사용할 수 있어요.

---

## 빠른 시작

### 1. 새 프로젝트에 드롭인

```bash
cp -r .harness/ {your-project-root}/
```

### 2. 셋업 순서대로 읽기

```
.harness/10_bootstrap/INIT_ENTRY.md      ← 여기서 시작
.harness/70_init-packs/COMMON_INIT_SEQUENCE.md
.harness/70_init-packs/claude/README.md  ← Claude 환경
.harness/70_init-packs/codex/README.md   ← Codex 환경
```

### 3. LLM에 붙여넣기

Claude 또는 Codex 새 세션에서 아래 순서로 읽게 하면 끝이에요:

```
1. {project_root}/CLAUDE.md (또는 AGENTS.md)
2. .harness/70_init-packs/{llm}/README.md
3. .harness/70_init-packs/COMMON_INIT_SEQUENCE.md
4. .{llm}/README.md  (로컬 런타임 브리지)
```

---

## 폴더 구조

```
.harness/
├── 00_foundation/     철학, 우선순위, 불변 규칙
├── 10_bootstrap/      새 환경 주입 / 기존 환경 보강 셋업
├── 20_runtime/        Plan → Work → Verify 런타임 규칙
├── 40_profiles/       전역 런타임 프로필 정책
├── 50_scoring/        카테고리별 점수제와 채택 기준
├── 60_templates/      산출물 템플릿 (handoff, task, rework 등)
├── 70_init-packs/     LLM별 진입 init pack
│   ├── claude/
│   ├── codex/
│   ├── generic-llm/
│   └── COMMON_INIT_SEQUENCE.md
├── ACTIVE_FILES.md    파일 분류 인벤토리
├── CHECKLIST.md       셋업 체크리스트
├── INDEX.md           번호 체계 전체 매핑
├── QUICKSTART.md      빠른 참조 가이드
└── README.md          ← 현재 문서
```

---

## 핵심 개념

| 개념 | 설명 |
|------|------|
| **Plan / Work / Verify** | 작업 3단계. LLM별로 담당 단계를 분리할 수 있어요 |
| **Guardian** | 정책 준수를 자동 감시하는 에이전트 계층 |
| **Handoff** | Plan → Work 전환 시 생성하는 MD+JSON 산출물 쌍 |
| **Harness-first** | 정책 문서(마스터)를 먼저 수정한 뒤 구현체를 동기화 |
| **score_breakdown** | 작업 품질을 카테고리별로 수치화하는 점수 구조 |

---

## 멀티-LLM 역할 예시

```
Claude  →  Plan (계획, 핸드오프 생성)
Codex   →  Work (구현, 커밋, 검증)
Claude  →  Final Check (게이트 판정)
```

단일 LLM으로도 운영 가능해요. `40_profiles/global-runtime-profile-policy.md` 참조.

---

## 주요 문서 경로

| 목적 | 파일 |
|------|------|
| 진입점 | `.harness/10_bootstrap/INIT_ENTRY.md` |
| 공통 init 순서 | `.harness/70_init-packs/COMMON_INIT_SEQUENCE.md` |
| Guardian 설치 | `.harness/10_bootstrap/guardian-installation-strategy.md` |
| 핸드오프 계약 | `.harness/20_runtime/handoff-contract.md` |
| 점수제 | `.harness/50_scoring/scoring-framework.md` |
| 템플릿 목록 | `.harness/60_templates/required-template-catalog.md` |

---

## 라이선스

개인 프로젝트용 거버넌스 킷이에요. 필요에 맞게 자유롭게 수정해서 사용하세요.
