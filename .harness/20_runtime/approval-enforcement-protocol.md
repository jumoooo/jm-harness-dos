# 승인 강제 프로토콜 (Approval Enforcement Protocol)

> 작성일: 2026-05-15
> 분류: 런타임 정책
> 구현 위치: `.claude/hooks/confirm-before-edit.ps1` v2

## 목적

Claude가 의견·분석 요청을 구현 지시로 오해하는 것을 기계적으로 차단한다.
"의견요청 ≠ 구현지시" 원칙을 훅 레벨에서 강제한다.

## 원칙

- 허용 트리거 없이 Edit / Write / Bash 실행 금지
- "어떻게 생각해?", "맞아?", "확인해봐" → 분석만 제공, 파일 수정 금지
- 명시적 허용 트리거 예시: "해줘", "수정해줘", "적용해줘", "응", "ㅇㅇ", "ok"

## 올바른 흐름

1. 사용자가 의견·분석 요청
2. Claude가 분석 결과 제공
3. Claude가 "적용할까요?" 확인 후 STOP
4. 사용자가 허용 트리거 응답
5. 훅이 transcript 읽어 허용 트리거 확인 → Edit/Write/Bash 실행

## 3계층 아키텍처

| 계층 | 구현 위치 | 강도 |
|-----|---------|-----|
| 1. 문서 규칙 | `CLAUDE.md`, `~/.claude/CLAUDE.md` | 📄 규칙 |
| 2. transcript 훅 | `.claude/hooks/confirm-before-edit.ps1` v2 | 🔒 exit 2 차단 |
| 3. 감사 로그 (선택) | PostToolUse (미구현) | — |

## 훅 동작 방식 (confirm-before-edit.ps1 v2)

```
PreToolUse 실행 (Edit / Write / Bash 감지)
    ↓
stdin JSON에서 transcript_path 추출
    ↓
JSONL 마지막 30줄 읽기 → 사용자 메시지 파싱
(포맷 A: role/user, 포맷 B: type/human, 포맷 C: type/user 지원)
    ↓
"?" 포함 → exit 2 (의문문 = 의견 요청, 즉시 차단)
허용 트리거 매칭 → exit 0 (실행 허용)
매칭 없음 → exit 2 (차단)
파싱 실패 → exit 0 (fail-open, stderr에 실패 이유 기록)
```

## 허용 트리거 패턴

**단독 긍정**: `응`, `ㅇㅇ`, `네`, `그래`, `어`, `ㅇ`, `좋아`, `맞아`, `ok`, `yes`, `yeah`, `yep`

**명시적 지시**: `해줘`, `적용해줘`, `수정해줘`, `추가해줘`, `넣어줘`, `바꿔줘`, `만들어줘`,
`고쳐줘`, `진행해줘`, `작업해줘`, `구현해줘`, `실행해줘`, `변경해줘`, `삭제해줘`, `등록해줘`, `반영해줘`

**직접 지시**: `직접 작업`, `그대로 해`, `내용그대로`, `진행해`, `실행해`, `반영해`

## fail-open 설계 근거

transcript 파싱 실패 시 exit 0 (허용): 정상 작업의 과차단 방지 우선.
파싱 실패 원인은 반드시 stderr에 기록.
완전 fail-closed 전환 시 정상 작업까지 차단될 위험 → 현재 단계에서 채택 안함.

## 구현 위임

| 항목 | 파일 |
|-----|-----|
| 실제 강제 훅 | `.claude/hooks/confirm-before-edit.ps1` v2 |
| 정책 선언 | `CLAUDE.md` §구현 전 승인 필수 |
| 전역 정책 | `~/.claude/CLAUDE.md` |
| 메모리 피드백 | `~/.claude/projects/.../memory/feedback_ask_before_implement.md` |
| 훅 등록 | `.claude/settings.json` (`PreToolUse`, `Edit|Write|Bash` 매처) |
