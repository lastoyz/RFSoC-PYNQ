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

## `pynq/`의 정의 (중요)

- 이 저장소는 `RFSoC-PYNQ`이며, 보드별 확장 자산(`boards/`)을 중심으로 관리한다.
- `pynq/`는 이 저장소 내부 코드가 아니라, 외부 `Xilinx/PYNQ` 저장소를 특정 커밋으로 고정한 submodule이다.
- 즉 `pynq/`는 "복사본 폴더"가 아니라 "외부 소스 포인터"이며, SD image 빌드 시 필요한 공통 빌드 시스템(`sdbuild`)을 제공한다.
- 결과적으로 RFSoC-PYNQ는 보드 커스터마이징을, PYNQ submodule은 공통 런타임/이미지 빌드 기반을 담당한다.

## PYNQ 프레임워크 기능 관점 정리

- PYNQ는 "Python으로 FPGA/SoC 하드웨어를 제어"하는 프레임워크다.
- 핵심 사용 패턴은 overlay(bitstream) 로딩, MMIO/GPIO/interrupt 접근, Jupyter 기반 실험/검증이다.
- RFSoC-PYNQ는 이 PYNQ 프레임워크를 그대로 활용하면서, RFSoC 보드에 필요한 자산(overlay, board package, BSP, notebooks)을 얹는 확장 레이어다.

## fork 관계 관점 정리

- 현재 저장소가 fork라 하더라도 `pynq/`의 실제 소스 기준은 `.gitmodules`에 정의된 upstream URL/commit이다.
- 따라서 fork 저장소에서 작업해도 SD image 빌드 경로는 `pynq` submodule 버전에 의존한다.
- `pynq`를 업데이트하면 "외부 소스 버전"이 바뀌는 것이며, RFSoC-PYNQ 자체 코드 변경과 성격이 다르다.

## "그대로 가져오나?"에 대한 답

- 기본 프레임워크(`pynq/`)는 submodule로 가져와 재사용한다.
- 다만 RFSoC-PYNQ는 다음 영역에서 보드 특화 변경/확장을 가진다.
  - `boards/<BOARD>/base`: 보드별 base overlay 설계/빌드 Tcl
  - `boards/<BOARD>/petalinux_bsp`: u-boot/kernel/device-tree 커스터마이징
  - `boards/<BOARD>/packages`: `rfsystem`, `tics`, `rfsoc4x2_oled` 등 보드 전용 패키지
  - 일부 패키지는 patch 형태로 동작 수정 (예: `xrfclk.patch`)
- 따라서 "PYNQ 엔진 + RFSoC 보드 확장" 구조로 이해하는 것이 정확하다.

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
