# FPGA Board Vivado Setup

## 목적

RFSoC 보드의 base overlay 기준으로 Vivado 프로젝트를 생성하고 `.bit/.hwh`를 빌드하는 절차만 다룬다.

## 범위

- 대상: `RFSoC4x2`, `ZCU208` (저장소 내 base Tcl 존재 확인)
- 기준 Vivado 버전: `2024.1` (문서 기준)
- 포함: 프로젝트 생성, bitstream/hwh 생성
- 제외: 보드 반영, overlay 로딩, 기능 테스트 (별도 문서)

## 관련 경로

- `boards/RFSoC4x2/base/base.tcl`
- `boards/RFSoC4x2/base/build_bitstream.tcl`
- `boards/ZCU208/base/base.tcl`
- `boards/ZCU208/base/build_bitstream.tcl`

## 문서 경계

- 보드 반영/overlay 로딩: `fw-setup-deep-dive.md`
- 노트북 기반 기능 검증: `sw-setup-deep-dive.md`
- 전체 진행 순서/게이트: `guides/README.md`

## 빠른 결론

- 저장소에는 `.xpr`가 커밋되어 있지 않다.
- `base.tcl`로 Vivado 프로젝트를 생성하고, `build_bitstream.tcl` 또는 `make`로 bitstream을 빌드하는 구조다.
- 보드 기본 이미지에는 prebuilt base overlay가 포함되어 있으므로, 설계 수정/분석 목적이 아니면 재빌드는 필수가 아니다.

## 절차

### 1) 사전 준비

- Vivado `2024.1` 설치
- RFSoC 디바이스 패키지 포함 여부 확인
- 저장소 clone 후 대상 보드 `base` 디렉터리로 이동

예시:

```bash
cd RFSoC-PYNQ/boards/RFSoC4x2/base
```

### 2) Vivado 프로젝트 생성

Vivado Tcl console에서 실행:

```tcl
source ./base.tcl
```

이 단계에서 project 생성 및 block design 구성이 진행된다.

### 3) Bitstream 재빌드

Vivado Tcl console에서 실행:

```tcl
close_project
source ./build_bitstream.tcl
```

또는 Linux 환경에서는:

```bash
make
```

### 4) 산출물 확인 (이 문서의 종료점)

- bitstream (`.bit`)
- 하드웨어 핸드오프 파일 (`.hwh`)

여기까지 완료하면 빌드 단계 종료이며, 이후 절차는 FW/SW 문서로 진행한다.

## 참고

- 상세 설명 문서: `docs/rfsoc_4x2_base_overlay.md`
- 상위 개요: `README.md`
