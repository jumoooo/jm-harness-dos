# Bootstrap Architecture

## 목적

새로운 LLM 환경에 하네스를 처음 주입하는 절차를 정의해요.

## 진입 신호

- `.harness/... init 해줘`
- `.harness/BOOTSTRAP_GUIDE.md 기준으로 셋업해줘`
- 특정 init pack 경로를 태그해서 init 요청

## 기본 흐름

1. foundation 문서 읽기
2. 현재 LLM 특성 파악
3. 전역 프로필 확인 또는 생성
4. 단일/복수 역할 구성 판정
5. 필요한 로컬 디렉토리와 guardian 계층 설계
6. 템플릿과 init 가이드 주입
7. bootstrap 검수

## 핵심 불변 조건

- 하네스 방향성은 bootstrap 편의보다 우선해요.
- 단일 LLM 모드가 항상 가능해야 해요.
- bootstrap은 runtime 규칙과 혼합되지 않아야 해요.
