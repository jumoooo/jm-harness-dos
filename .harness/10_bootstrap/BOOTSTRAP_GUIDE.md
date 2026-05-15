# Bootstrap Guide

## 목적

새로운 LLM 환경에 하네스를 처음 주입할 때 따를 표준 절차를 정의해요.

## 선행 읽기

1. `../00_foundation/README.md`
2. `../00_foundation/philosophy.md`
3. `../00_foundation/non-negotiables.md`
4. `INIT_ENTRY.md`

## 기본 흐름

1. foundation 문서를 읽어요.
2. 현재 LLM 특성과 기존 자산 유무를 파악해요.
3. bootstrap인지 retrofit인지 판정해요.
4. 전역 프로필 필요 여부를 확인해요.
5. 단일/복수 역할 구성을 판정해요.
6. 필요한 로컬 디렉토리와 guardian 계층을 설계해요.
7. 템플릿과 init 가이드 연결 구조를 정해요.
8. contamination 결과와 승인 필요 여부를 보고해요.
9. 승인 후 셋업을 진행해요.
10. 셋업 완료 후 runtime 단계로 인계해요.

## 핵심 불변 조건

- 하네스 방향성은 bootstrap 편의보다 우선해요.
- 단일 LLM 모드가 항상 가능해야 해요.
- bootstrap은 runtime 규칙과 혼합되지 않아야 해요.
- guardian 설치와 guardian 운영 규칙은 분리돼야 해요.

## runtime 연결 원칙

- 셋업 단계는 `20_runtime/`의 존재를 전제로만 연결해요.
- runtime 상세 규칙은 이 문서에서 정의하지 않아요.
- 셋업 결과는 "runtime 문서를 읽을 준비가 된 상태"까지만 책임져요.

## 메모

> 📦 bootstrap의 완료 기준은 설치 준비와 연결 구조 확정이에요.
> 운영 습관까지 여기서 다루지 않아요.
