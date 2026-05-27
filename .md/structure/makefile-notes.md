# Makefile Notes

## 목적

루트 `Makefile`이 SD image 빌드 과정에서 수행하는 역할과 입력/출력을 구조적으로 설명한다.

## 범위

- 포함: 주요 변수, 타깃 의존성, 산출물 흐름
- 제외: Linux/WSL 설치 가이드, 보드 기능 테스트 상세

## 역할 요약

- 루트 `Makefile`은 SD image 빌드의 실행 진입점이다.
- 한 줄 명령(`make BOARD=<BOARD>`)으로 check -> overlay build -> image build를 오케스트레이션한다.

## 주요 입력

- `BOARD`: 타깃 보드 식별자 (`RFSoC4x2`, `ZCU208`, `ZCU111`)
- `boards/<BOARD>/<BOARD>.bsp`: 보드 BSP 파일
- `pynq/` submodule 및 `pynq/sdbuild` 스크립트
- prebuilt rootfs/sdist 다운로드 URL

## 주요 타깃

- `checkenv_rfsocpynq`: `BOARD`/BSP 존재 확인
- `checkenv_pynq`: `pynq/sdbuild/scripts/check_env.sh` 실행
- `${BASE_OVERLAY}`: `boards/<BOARD>/base`에서 `make` 호출
- `${IMAGE}`: `pynq/sdbuild` 호출 후 최종 이미지 파일 이동

## 산출물

- 최종: `<BOARD>-<VERSION>.img`
- 중간: board base overlay 산출물(`base.bit` 등)

## 실무 메모

- `make`는 현재 디렉터리의 `Makefile`을 기본 참조하므로 루트에서 실행해야 한다.
- 이 Makefile 흐름은 SD image 빌드가 FW 중심이면서도 PL 산출물을 연계하는 통합 파이프라인임을 보여준다.

## 관련 문서

- `../guides/sd-image-build.md`
- `boards-directory-notes.md`
