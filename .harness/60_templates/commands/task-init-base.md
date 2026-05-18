# task-init-base - 태스크 스캐폴딩 커맨드 베이스

## 역할
신규 태스크 디렉터리, SoT 파일, 전역 인덱스를 생성해요.

## 생성 파일
```text
.ai/tasks/{task_id}/
├── todo.md        (task-todo.md 템플릿 기반)
└── evidence/      (디렉터리만)
```

## task_id 형식
`TASK-NNN` 형식이에요.
세 자리 순번으로 자동 증가해요.

## 전역 인덱스 갱신
`.ai/tasks/index.md`에 신규 행을 추가해요.

```text
| {task_id} | {제목} | TODO | {담당 LLM} | {핸드오프 ID} | {오늘 날짜} |
```

## 필수 입력
- task 제목
- 담당 LLM (`Claude` / `Codex` / `Generic`)
- 핸드오프 ID (있으면)

## 구현 참조
- 템플릿: `.ai/templates/task-todo.md`
- 인덱스: `.ai/tasks/index.md`
