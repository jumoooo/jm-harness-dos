# INTEGRATION_GATE

## 목적
Work 완료 후 Verify 단계에서 이 파일 기준으로 PASS/FAIL 판정을 해요.

## 기본 게이트 기준
- [ ] acceptance_criteria 전 항목 충족
- [ ] verification_commands 전 항목 exit_code 0
- [ ] 새로 생성한 파일이 work-order.md `batches[].files` 목록과 일치
- [ ] 커밋 메시지 Conventional Commits 형식 준수
- [ ] 신규 파일에 프로젝트 고유 하드코딩 없음 (템플릿 변수 `{placeholder}` 사용)

## 판정 기준
- PASS: 전 항목이 모두 충족돼요.
- FAIL: 1개 이상 미충족이고, 사유를 함께 기록해요.

## Evidence 형식
`{ command, exit_code, summary }[]`
