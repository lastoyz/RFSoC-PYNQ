# Board Variants Notes

## 목적

지원 보드별 공통점과 차이를 한 페이지로 정리해, 가이드 적용 시 혼선을 줄인다.

## 대상 보드

- `RFSoC4x2`
- `ZCU208`
- `ZCU111`

## 공통점

- base overlay 빌드 흐름은 동일한 형태를 따른다.
  - `boards/<BOARD>/base`에서 `make` 또는 Tcl 기반 빌드
- SD 이미지 재빌드는 루트에서 `make BOARD=<BOARD>`로 수행한다.
- 전체 흐름은 공통 가이드를 그대로 따른다.
  - bring-up -> PL 빌드 -> (필요 시) SD 이미지 빌드 -> FW 반영 -> SW 테스트

## 차이점 (실무 관점)

### 1) BSP 확보 경로

- `RFSoC4x2`: Real Digital 제공 BSP 링크 사용
- `ZCU208`: AMD 제공 BSP 링크 사용
- `ZCU111`: 루트 README 기준 링크가 "coming soon"으로 표시될 수 있어 별도 확보 경로 확인 필요

### 2) 보드별 base 디렉터리/part 설정

- 보드마다 `boards/<BOARD>/base/base.tcl`의 part/board 설정이 다르므로 교차 사용 금지
- 산출물(`.bit/.hwh`)은 반드시 동일 보드 기준으로 사용

### 3) 테스트 중점

- 공통 노트북 흐름은 유사하지만, 보드별 연결/클럭/주변장치 구성에 따라 관찰 포인트가 달라질 수 있다.
- RF 체인 검증 시 보드 액세서리/배선 조건 차이를 먼저 통제해야 한다.

## 적용 규칙

- 문서/명령의 `<BOARD>` 값은 항상 현재 타깃 보드로 치환
- BSP 파일은 `boards/<BOARD>/<BOARD>.bsp` 경로 규칙 준수
- 문제 발생 시 먼저 "보드 불일치(파일/산출물/설정)" 여부 확인

## 관련 가이드

- `board-bringup-quickstart.md`
- `fpga-board-vivado-setup.md`
- `sd-image-build.md`
- `fw-setup-deep-dive.md`
- `sw-setup-deep-dive.md`
- `common-troubleshooting.md`
