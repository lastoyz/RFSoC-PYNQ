# Repository Overview

## 목적

저장소의 핵심 축인 `Makefile`, `boards/`, `docs/`가 어떻게 연결되어 동작하는지 상위 관점에서 정리한다.

## 범위

- 포함: 디렉터리 역할, 빌드 연결 관계, 문서 경계
- 제외: 단계별 실행 절차 상세 (해당 내용은 `guides/` 문서에서 관리)

## 핵심 구조

- `Makefile`: 루트 빌드 오케스트레이터
- `boards/`: 보드별 설계/패키지/BSP/노트북 자산
- `docs/`: 공개 문서 사이트(Jekyll) 소스
- `pynq/`: submodule로 연결된 상위 PYNQ 빌드 시스템(`sdbuild`) 소스

## 빌드 흐름 관점 연결

1. 사용자가 루트에서 `make BOARD=<BOARD>` 실행
2. 루트 `Makefile`이 환경/BSP를 확인
3. 필요 시 `boards/<BOARD>/base`를 호출해 base overlay 산출물 생성
4. `pynq/sdbuild`를 호출해 SD 이미지 생성
5. 결과로 `<BOARD>-<VERSION>.img` 산출

## 문서 경계

- 절차/운영: `guides/` (bring-up, PL/FW/SW, troubleshooting)
- 구조/코드 맥락: `structure/` (이 문서 포함)
- 프로젝트 배경: `project-purpose.md`

## 관련 문서

- `makefile-notes.md`
- `boards-directory-notes.md`
- `docs-directory-notes.md`
