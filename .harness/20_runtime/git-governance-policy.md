# Git Governance Policy

이 문서는 `{project}-root-ops` 저장소(ops 전용)의 브랜치 생성·운영·종료에 관한
기준을 정의한다. `.harness/` 우선 원칙에 따라 이 정책이 다른 모든 git 관련 지침보다
우선한다.

## 저장소 특성

- **용도**: ops 전용 (.ai/, .harness/, .claude/, .codex/, docs/ 만 추적)
- **비추적 대상**: frontend/, backend/ (별도 코드 저장소)
- **협업 모델**: Claude(Plan) + Codex(Work) 멀티-LLM

---

## 1. 절대 금지 사항 (Non-Negotiables)

아래 항목은 어떤 사유로도 예외를 허용하지 않는다.
위반 시 즉시 revert 처리하고 incident 기록을 남긴다.

| # | 금지 행위 | 이유 |
|---|-----------|------|
| N-1 | `main` 브랜치에 직접 push | 검토 없는 변경으로 harness SoT가 오염됨 |
| N-2 | ops repo에 `feat/`, `fix/`, `codex/feat/`, `codex/fix/` 브랜치 생성 | 코드 구현 브랜치는 코드 저장소 전용 접두사임 |
| N-3 | frontend/ 또는 backend/ 소스 코드 변경 커밋 | ops repo 추적 범위 외 파일 |
| N-4 | 동일 커밋에 harness 구조 변경 + plan 산출물 혼합 | Atomic commit 원칙 위반 |
| N-5 | 브랜치 수명 초과 상태에서 PR 없이 지속 커밋 | 이종 커밋 누적 → 리뷰 불가 상태 |
| N-6 | 시크릿·실제 API 키 커밋 | 보안 사고 |
| N-7 | `git push --force` (main 대상) | 이력 파괴 |
| N-8 | merge 후 브랜치 미삭제 | 좀비 브랜치 누적으로 탐색 비용 증가 |

---

## 2. 브랜치 타입 매트릭스

### 허용 브랜치 접두사

| 타입 | 네이밍 패턴 | 최대 수명 | merge 정책 | 종료 조건 |
|------|------------|-----------|-----------|-----------|
| `ops` | `ops/YYYY-MM_slug` | 48h | Squash Merge | ops 설정 변경 PR merge + 브랜치 삭제 |
| `plan` | `plan/issue-N-slug` | 24h | Squash Merge | work-order MD+JSON 동쌍 merge + 브랜치 삭제 |
| `docs` | `docs/YYYY-MM_slug` | 4h | Squash Merge | 단일 주제 문서 merge + 브랜치 삭제 |

> **slug 규칙**: 영문 소문자 + 하이픈. 띄어쓰기 없음. 최대 40자.
> 예시: `ops/2026-05_harness-runtime-policy`, `plan/issue-7-server-list-api`, `docs/2026-05_git-governance`

### 브랜치 타입 선택 가이드

```
변경 대상이 .harness/, .ai/gates/, .claude/, .codex/ 구조 또는 워크플로우?
  → ops/YYYY-MM_slug

GitHub Issue에 연결된 계획 산출물 (.ai/plans, .ai/handoffs, .ai/tasks)?
  → plan/issue-N-slug

docs/ 폴더의 단일 주제 문서?
  → docs/YYYY-MM_slug
```

### 금지 접두사 (ops repo에서)

| 금지 패턴 | 이유 |
|-----------|------|
| `feat/*` | 코드 저장소 전용 |
| `fix/*` | 코드 저장소 전용 |
| `codex/feat/*` | 코드 저장소 전용 |
| `codex/fix/*` | 코드 저장소 전용 |
| `hotfix/*` | ops repo에 코드 수정 없음 |
| `release/*` | ops repo에 배포 이력 없음 |

---

## 3. 브랜치 생명주기 상태 머신

```
CREATED → IN_PROGRESS → REVIEW_REQUESTED → MERGED → DELETED
   │              │               │
   │ (수명 초과)  │ (수명 초과)    │ (24h 방치)
   ↓              ↓               ↓
EXPIRED        EXPIRED         STALE_PR
```

### 상태별 EXIT CRITERIA

#### CREATED

진입 조건:
- 올바른 접두사 + 네이밍 패턴 충족
- 작업 범위가 단일 타입(ops OR plan OR docs)

EXIT CRITERIA (→ IN_PROGRESS):
- 첫 번째 Atomic 커밋 push 완료
- 수명 타이머 시작

#### IN_PROGRESS

진입 조건: CREATED에서 첫 커밋 이후

EXIT CRITERIA (→ REVIEW_REQUESTED):
- [ ] 모든 커밋이 Atomic Commit 기준 충족
- [ ] Conventional Commits 형식 사용
- [ ] 브랜치 수명 이내
- [ ] PR 생성 완료 (해당 브랜치 타입의 PR 템플릿 사용)

EXIT CRITERIA (→ EXPIRED):
- 브랜치 수명 초과 + PR 미생성

#### REVIEW_REQUESTED

진입 조건: PR 생성 완료

EXIT CRITERIA (→ MERGED):
- [ ] PR 체크리스트 항목 전부 체크
- [ ] branch-governance 워크플로우 PASS
- [ ] CI 검사 PASS (해당하는 경우)
- [ ] 리뷰어 승인 (또는 단독 작업 시 자가 확인)

EXIT CRITERIA (→ STALE_PR):
- PR 생성 후 24h 내 merge 미완료

#### MERGED

진입 조건: PR Squash Merge 완료

EXIT CRITERIA (→ DELETED):
- [ ] 브랜치 삭제 완료 (GitHub "Delete branch" 또는 `git push origin --delete`)
- [ ] 로컬 브랜치도 삭제 (`git branch -d`)

#### EXPIRED / STALE_PR

대응 절차 (아래 §5 참조):
1. 브랜치 강제 종료 또는 수명 재협의
2. incident 기록

---

## 4. 브랜치별 Atomic Commit 기준

Atomic Commit = 하나의 커밋은 하나의 논리 단위만 담는다.
"논리 단위"는 독립적으로 revert 가능하고, 커밋 메시지 한 줄로 설명 가능한 변경을 뜻한다.

### ops/ 브랜치

```
허용 커밋 유형:
  chore(harness): ...    — .harness/ 구조·정책 파일 변경
  chore(claude): ...     — .claude/ 명령·에이전트·프로필 변경
  chore(codex): ...      — .codex/ 규칙·에이전트 변경
  chore(github): ...     — .github/ 워크플로우·템플릿 변경
  chore(root): ...       — 루트 설정 파일 (.gitignore, docker-compose 등)

금지:
  - docs/ 변경을 ops/ 브랜치 커밋에 혼합
  - plan 산출물(.ai/plans, .ai/handoffs)을 ops/ 브랜치 커밋에 혼합
```

### plan/ 브랜치

```
허용 커밋 유형:
  docs(plan): ...        — .ai/plans/drafts/ 계획 초안
  docs(handoff): ...     — .ai/handoffs/ work-order MD+JSON 동쌍
  docs(task): ...        — .ai/tasks/<id>/ todo, evidence

금지:
  - harness 구조 변경을 plan/ 브랜치 커밋에 혼합
  - 코드 파일 포함
```

### docs/ 브랜치

```
허용 커밋 유형:
  docs(<scope>): ...     — docs/ 하위 단일 주제

금지:
  - 두 개 이상의 독립 주제 혼합
  - .harness/, .ai/, .claude/ 파일 변경 혼합
```

---

## 5. 위반 시 대응 절차

### 위반 감지 경로

1. `branch-governance.yml` 워크플로우 자동 감지 (PR 생성 시)
2. main push 시도 (branch protection rule 거부)
3. 수동 검토 중 발견

### 대응 단계

```
위반 발견
  │
  ├─ N-1 (main 직접 push 성공한 경우)
  │    → 즉시 git revert + 강제 push (main 관리자만)
  │    → .ai/harness/safety-exception-log.md 기록
  │
  ├─ N-2 (금지 브랜치 생성)
  │    → 브랜치 삭제 후 올바른 접두사로 재생성
  │    → 재생성 시 수명 타이머 초기화
  │
  ├─ N-4 (Atomic 위반 커밋)
  │    → PR 단계에서 발견: 커밋 분리 요청 후 PR 닫기
  │    → merge 후 발견: revert + 재분리 + 새 PR
  │
  └─ N-8 (merge 후 브랜치 미삭제)
       → 7일 경과 시 자동 삭제 대상으로 등록
       → .ai/harness/safety-exception-log.md 기록
```

### 수명 초과 처리

| 브랜치 타입 | 수명 초과 임계 | 처리 |
|------------|--------------|------|
| docs/ | 4h + 2h grace | 작업 중단 → 즉시 PR 또는 브랜치 삭제 |
| plan/ | 24h + 4h grace | 작업 중단 → 즉시 PR 또는 브랜치 삭제 |
| ops/ | 48h + 8h grace | 수명 재협의 필요 (`.ai/harness/safety-exception-log.md`에 사유 기록) |

### N-5 브랜치 수명 경고 hook 등록 절차

1. `.claude/hooks/branch-lifetime-check.ps1`를 추가한다.
2. `.claude/settings.json`의 `PreToolUse`에 경고 hook으로 등록한다.
3. 초기 운영은 경고 모드(`Write-Warning` + `exit 0`)로 시작한다.
4. 실제 차단 전환은 운영 안정화 후 별도 합의로 진행한다.

---

## 6. Squash Merge 정책

ops repo는 모든 PR에 **Squash Merge**만 허용한다.

이유:
- 브랜치 개발 중 WIP 커밋, 오타 수정 커밋이 main 이력에 노출되지 않도록 한다.
- main 이력 = 논리 단위 변경 이력 = 브랜치 하나 = 커밋 하나 구조를 유지한다.

Squash Merge 커밋 메시지 형식:
```
<type>(<scope>): <요약> (#PR번호)
```

예시:
```
chore(harness): git governance policy 추가 (#42)
docs(plan): issue-7 server-list-api work order 생성 (#43)
```

### N-6 시크릿 차단 hook 등록 절차

1. `.claude/hooks/secret-detection.ps1`를 추가한다.
2. `.claude/settings.json`의 `PreToolUse`에 차단 hook으로 등록한다.
3. 탐지 대상 패턴은 OpenAI, Google, AWS, GitHub, Slack 등 대표 토큰 형식을 포함한다.
4. 탐지 시 즉시 실패(`exit 1`)하고 사용자에게 차단 이유를 보고한다.

---

## 구현 위임 (Implementation Delegation)

이 문서는 "What" 레이어 — 정책 의도와 규칙을 정의합니다.
실제 기계적 강제는 "How" 레이어(런타임)에 위임됩니다.

| 규칙 | 위임 파일 | 강도 | 상태 |
|-----|---------|-----|-----|
| N-2 금지 브랜치 접두사 | `.claude/hooks/branch-naming-check.ps1` | 🔒 exit 1 차단 | ✅ 활성 |
| N-5 브랜치 수명 경고 | `.claude/hooks/branch-lifetime-check.ps1` | ⚠️ 경고 | ✅ 활성 (§5 경고 모드 허용) |
| N-6 시크릿 차단 | `.claude/hooks/secret-detection.ps1` | 🔒 exit 2 차단 | ✅ 활성 |
| N-7 force push 차단 | 미구현 | — | ❌ 훅 레벨 커버 어려움 |
| N-8 merge 후 브랜치 삭제 | 미구현 | — | ❌ 수동 프로세스 |
| Work 범위 강제 (Codex) | `.codex/hooks/pre_tool_policy.js` | 🔒 차단 | ✅ 활성 |

**원칙**: 위임 파일을 먼저 수정하고, 이 문서는 의도만 반영합니다.
새 규칙 추가 시 이 테이블을 반드시 갱신합니다.

## 참조 문서

- Codex ops 플레이북: `.codex/rules/git-ops-playbook.md`
- Safety 예외 로그: `.ai/harness/safety-exception-log.md`
- branch-governance 워크플로우: `.github/workflows/branch-governance.yml`
- harness 런타임 정책: `.harness/20_runtime/plan-work-verify-runtime.md`
