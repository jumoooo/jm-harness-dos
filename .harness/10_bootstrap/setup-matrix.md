# Setup Matrix - 역할 x LLM 셋업 결정표

## 이 문서의 용도

셋업 시작 시 가장 먼저 읽는 문서예요.
역할과 LLM 종류를 결정하면,
어떤 파일을 만들고 어떤 가이드를 따라야 하는지 즉시 파악할 수 있어요.

## 전제 조건

- `.harness/` 폴더가 프로젝트 루트에 존재해요.
- `.agents/skills/` 폴더가 프로젝트 루트에 존재해요.
- `.ai/` 워크스페이스가 초기화되어 있어요.

## 역할 정의

| 역할 | 책임 |
|---|---|
| Plan | 요청 수신, 계획 수립, 핸드오프 생성, Guardian 호출 |
| Work | 핸드오프 수신, 구현/커밋, Verify 핸드오프 생성 |
| Verify | 검증 실행, 채점, PASS/FAIL 판정, `runs.jsonl` 기록 |
| FinalCheck | Verify 결과 수신, 최종 판정, 커밋 프롬프트 출력 |

## 역할 x LLM 매트릭스

| 역할 | Claude | Codex | Generic LLM |
|---|---|---|---|
| Plan | `.md` commands, `.md` agents, `.ps1` hooks, `settings.json` | `.toml` agents, `.js` hooks, `config.toml` | `.md` only |
|  | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 |
| Work | `.md` agents, `.ps1` scope hooks, `settings.json` | `.toml` agents, `.js` scope hooks, `hooks.json` | `.md` only |
|  | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 |
| Verify | `.md` check-gate, `.ps1` hooks | `.toml` reviewer, `.js` hooks | `.md` only |
|  | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 | 역할별 구체 가이드: `.harness/70_init-packs/ROLE_GUIDE.md` 참조 |

## 셋업 결정 트리

1. 단일 LLM인가, 멀티 LLM인가를 먼저 결정해요.
   - 단일: Plan + Work + Verify를 모두 담당해요. 모든 `setup-guide` 문서를 읽어요.
   - 멀티: 각 LLM은 본인 역할의 `setup-guide`만 읽어요.
2. LLM 종류를 확인해요.
   - Claude Code: `claude/` init-pack을 사용해요.
   - Codex: `codex/` init-pack을 사용해요.
   - 기타: `generic-llm/` init-pack을 사용해요.
3. 해당 셀의 파일 형식을 확인해요.
   - 그다음 역할별 `setup-guide-{role}.md`를 실행해요.

## 단일 LLM 운용 시

전역 프로필에서 `plan_llm`, `work_llm`, `verify_llm`를 동일 LLM으로 설정해요.
`work-start`, `plan-orchestrator`, `check-gate` 같은 핵심 진입점도 단일 환경에 연결해요.

## 참조

- 역할별 상세 가이드: `setup-guide-plan.md`, `setup-guide-work.md`, `setup-guide-verify.md`
- LLM별 파일 형식: `llm-format-rules.md`
- LLM별 진입 가이드: `../70_init-packs/{llm}/README.md`
- 역할 분리 정책: `../20_runtime/multi-llm-role-policy.md`

