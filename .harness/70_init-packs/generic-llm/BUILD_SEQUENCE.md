# Generic LLM BUILD_SEQUENCE

## 전제
- Phase 1~3 완료 상태예요.
- Generic LLM은 `.md` 파일만 지원한다고 가정해요.

## 생성 순서

### Step 1 — 디렉터리 구조 생성
```text
.{llm_name}/
├── agents/       (.md 파일만)
├── commands/     (.md 파일만)
└── profile.md    (담당 역할, 단계 기록)
```

### Step 2 — llm-format-rules.md의 Generic 섹션 기준으로 파일 생성
- 모든 에이전트와 커맨드는 순수 Markdown으로 작성해요.
- YAML front matter 없이 `##` 섹션으로만 구분해요.

### Step 3 — 담당 역할별 문서 복사
- Plan 담당이면 `setup-guide-plan.md` 기준을 적용해요.
- Work 담당이면 `setup-guide-work.md` 기준을 적용해요.
- Verify 담당이면 `setup-guide-verify.md` 기준을 적용해요.

### Step 4 — 검증
- [ ] `.{llm_name}/` 디렉터리 존재
- [ ] `agents/`, `commands/` 하위 파일 존재
- [ ] `profile.md`에 담당 단계 명시

## 참조
- `.harness/10_bootstrap/llm-format-rules.md`
