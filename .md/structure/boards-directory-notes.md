# boards Directory Notes

## 목적

`boards/` 디렉터리의 구성 규칙과 보드별 자산 배치를 빠르게 이해할 수 있도록 정리한다.

## 범위

- 포함: 공통 디렉터리 패턴, 주요 파일 역할, 빌드 연계 지점
- 제외: 각 노트북 사용법/보드 전기적 세부 사양

## 최상위 구성

- `boards/RFSoC4x2`
- `boards/ZCU208`
- `boards/ZCU111`
- `boards/ip` (보드 공용 커스텀 IP)

## 보드별 공통 패턴

- `<BOARD>.spec`
  - sdbuild용 보드 메타 정보(arch, BSP 이름, stage4 패키지 등)
- `base/`
  - base overlay 설계/빌드 자산 (`base.tcl`, `build_bitstream.tcl`, 제약/노트북 등)
- `petalinux_bsp/`
  - PS 측 BSP 커스터마이징 레이어
- `packages/`
  - 이미지에 포함될 보드 전용 패키지
- `notebooks/`
  - common/selftest 등 사용자 검증 노트북

## 루트 빌드와의 연결

- 루트 `Makefile`은 `boards/<BOARD>/<BOARD>.bsp`를 요구한다.
- 같은 Makefile 흐름에서 `boards/<BOARD>/base`의 `make`를 호출해 overlay 산출물을 만든다.
- 이후 `pynq/sdbuild`가 `boards/` 정보를 읽어 SD image를 생성한다.

## 주의 포인트

- 보드 간 산출물(`.bit/.hwh/.bsp`) 혼용 금지
- `<BOARD>.spec`의 패키지 조합은 이미지 기능/크기에 직접 영향
- 보드별 차이는 `../guides/board-variants-notes.md`에서 운영 관점으로 확인

## 관련 문서

- `makefile-notes.md`
- `../guides/fpga-board-vivado-setup.md`
- `../guides/board-variants-notes.md`
