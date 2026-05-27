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

## PYNQ 원본 대비 보드 확장 포인트

- `<BOARD>.spec`에서 stage4 패키지 조합을 보드별로 정의한다.
  - 예: `rfsystem`, `tics`, `rfsoc4x2_oled`, `rfsoc_sam`, `zcu111_sensors`
- `petalinux_bsp/`의 `u-boot`/`kernel` 레시피 append로 PS 부팅 환경을 보드별로 조정한다.
- `packages/`에는 보드 동작에 필요한 Python/유틸 패키지와 pre/post 스크립트가 포함된다.
- 일부 동작은 patch로 보정된다.
  - 예: `boards/RFSoC4x2/packages/xrfclk-patch/xrfclk-patch/xrfclk.patch`

## `packages/`와 `notebooks/`의 개념 (Python 관점)

### `packages/`는 "기능 구현 코드"

- `packages/`는 보드 기능을 실제로 구현하는 Python 패키지/유틸 계층이다.
- 일반적으로 `setup.py`, `package/<pkg_name>/...` 구조를 가지며, SD 이미지 빌드(stage4)에서 설치 대상으로 사용된다.
- Python 관점에서는 표준 패키징(installable module) 모델을 따르며, 런타임에서 `import` 가능한 라이브러리를 제공한다.
- 예:
  - RFDC 분석/제어 로직 (`rfsystem`)
  - 보드 주변장치 제어 (`rfsoc4x2_oled`)
  - 클럭/센서/유틸 계열 패키지

### `notebooks/`는 "사용/검증 인터페이스"

- `notebooks/`는 Jupyter Notebook(`.ipynb`) 기반의 사용 예제/검증 시나리오다.
- Python 관점에서는 라이브러리 기능을 호출하는 상위 레이어(실행 스크립트 + 문서) 역할이다.
- 즉 보통 노트북은 `packages/`의 API와 PYNQ 런타임 API를 조합해 실험/데모/셀프테스트를 수행한다.
- `common/`은 보드 공통 또는 PL 비의존 기능, `selftest/`/`base/notebooks/`는 보드/오버레이 의존 검증을 담당한다.

### 관계 요약

- `packages/` = "재사용 가능한 구현체(코어 Python 코드)"
- `notebooks/` = "그 구현체를 실행/검증/설명하는 사용자 진입점"
- 두 영역이 함께 PYNQ 프레임워크 위 보드 확장을 구성한다.

## 패키지 재활용성 메모 (향후 과제)

- 논의 결론:
  - 보드 의존 코드(PYNQ/overlay/MMIO/디바이스 접근)와 순수 Python 로직(신호처리/데이터 처리)을 분리하면 재활용성이 높아진다.
  - 이렇게 분리하면 Jupyter 외 일반 Python GUI/서비스 코드에서도 일부 모듈 재사용이 가능해진다.
- 현재 상태:
  - 이 저장소는 보드 동작 중심 구조이며, 재활용성 분리를 위한 대규모 리팩터링은 아직 수행하지 않았다.
- 향후 시도 방향(필요 시):
  - `core`(하드웨어 비의존) / `hw-adapter`(PYNQ 의존) 계층 분리
  - notebook은 상위 실행/검증 레이어로 유지
  - 단계적으로 기존 모듈을 이동해 호환성 확인

### 구현 범위 원칙 (RFSoC-PYNQ vs PYNQ)

- 1차 원칙:
  - `core/hw` 분리는 RFSoC-PYNQ 보드 패키지 내부(`boards/<BOARD>/packages/...`)에서 우선 수행한다.
- PYNQ 자체 수정은 기본적으로 제외:
  - 기존 PYNQ API(Overlay, DefaultIP, MMIO 등)로 추상화가 가능하면 submodule 수정 없이 진행한다.
- 예외적으로 PYNQ 수정이 필요한 경우:
  - 프레임워크 레벨의 공통 기능이 필요하거나,
  - PYNQ 제약/버그로 인해 보드 패키지 레벨에서 해결이 어려운 경우,
  - 여러 보드에서 재사용 가능한 공통 개선을 upstream으로 반영할 가치가 있는 경우

## 주의 포인트

- 보드 간 산출물(`.bit/.hwh/.bsp`) 혼용 금지
- `<BOARD>.spec`의 패키지 조합은 이미지 기능/크기에 직접 영향
- 보드별 차이는 `../guides/board-variants-notes.md`에서 운영 관점으로 확인

## 관련 문서

- `makefile-notes.md`
- `../guides/fpga-board-vivado-setup.md`
- `../guides/board-variants-notes.md`
