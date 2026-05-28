# RFSoC-PYNQ Board Setup and Test Guide (SSOT)

## 문서 목적

`RFSoC-PYNQ` 보드를 실무 관점에서 빠르게 셋업하고, overlay 로딩 및 노트북 기반 기능 테스트까지 완료하는 표준 절차를 제공한다.
이 문서는 `guides` 영역의 실행 순서를 정의하는 단일 기준(SSOT)으로 사용한다.

## 대상 보드

- `RFSoC4x2`
- `ZCU208`
- `ZCU111`

## 전체 흐름

1. Bring-up (전원/부팅/접속 확인)
2. PL 준비 (Vivado base overlay 빌드)
3. FW 반영 (bit/hwh 배치 + overlay 로딩)
4. SW 검증 (기본 + RFDC 노트북 테스트)
5. 필요 시 SD 이미지 재빌드
6. 결과 기록 (pass/fail, 로그, 이슈)

## 1) Bring-up

참조: `board-bringup-quickstart.md`

- SD 카드 이미지 삽입 및 부트 모드 확인
- UART/LED 로그로 정상 부팅 확인
- Jupyter/SSH 접속 확인
- 간단한 Python 셀 실행으로 기본 런타임 점검

완료 기준:

- [ ] 보드 정상 부팅
- [ ] Jupyter 또는 SSH 접속 성공

## 2) PL 준비 (Vivado)

참조: `fpga-board-vivado-setup.md`

권장 환경:

- Vivado `2024.1`

예시:

```bash
cd RFSoC-PYNQ/boards/<BOARD>/base
```

```tcl
source ./base.tcl
close_project
source ./build_bitstream.tcl
```

또는:

```bash
make
```

산출물:

- `.bit`
- `.hwh`

완료 기준:

- [ ] `.bit` 생성 확인
- [ ] `.hwh` 생성 확인

## 3) FW 반영 및 Overlay 로딩

참조: `fw-setup-deep-dive.md`

- 보드 내 overlay 경로에 `.bit/.hwh` 배치
- 동명 파일 교체 시 백업
- 재부팅 후 overlay 로딩
- 로딩 직후 기본 IP 접근 확인

완료 기준:

- [ ] overlay 로딩 성공
- [ ] 재부팅 후 재로딩 재현 가능

## 4) SW 기능 테스트

참조: `sw-setup-deep-dive.md`

기본 기능 테스트:

- `buttons_leds_switches.ipynb`
- `register_map_intro.ipynb`
- `pl_ddr4.ipynb`
- `syzygy_loopback.ipynb`

RFDC 테스트:

1. `01_rf_dataconverter_introduction.ipynb`
2. `02_rf_spectrum_analysis.ipynb`
3. `03_rf_spectrum_sweep.ipynb`

완료 기준:

- [ ] 기본 기능 테스트 통과
- [ ] RFDC 핵심 테스트 통과(또는 이슈 등록)

## 5) SD 이미지 재빌드 (필요 시)

참조: `sd-image-build.md`

다음 조건에서 수행:

- 커널/드라이버/rootfs/PYNQ 패키지까지 변경이 필요할 때
- 단순 overlay 교체가 아닌 보드 이미지 자체 갱신이 필요할 때

실행:

```bash
make BOARD=<BOARD>
```

## 트러블슈팅

공통 이슈 대응은 `common-troubleshooting.md`를 우선 참고한다.

## 보드별 적용 주의사항

보드별 BSP/설정/산출물 차이는 `board-variants-notes.md`를 우선 참고한다.

## 최종 Go/No-Go 게이트

- [ ] 보드 부팅/접속 안정
- [ ] Vivado 빌드 산출물 확보
- [ ] Overlay 로딩 성공
- [ ] 기본 기능 테스트 통과
- [ ] RFDC 테스트 통과 또는 이슈화
- [ ] 결과 및 로그 기록 완료
