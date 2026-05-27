# 프로젝트 목적 정리

## 한 줄 요약

이 프로젝트(`RFSoC-PYNQ`)는 AMD Zynq UltraScale+ RFSoC 보드를 대상으로, PYNQ 기반 Python API/오버레이/예제 및 빌드 스크립트를 제공하는 레퍼런스 저장소다.

## 저장소에서 확인되는 목적

- RFSoC 보드용 PYNQ 확장 제공
  - RF data converter, clock, overlay 활용을 위한 Python API/라이브러리/드라이버 제공
- 보드별 베이스 오버레이와 예제 설계 제공
  - RFSoC4x2, ZCU208(README 기준 지원 보드)
- SD 카드 이미지 및 base overlay 재빌드 경로 제공
  - 보드별 `boards/<BOARD>/base` 아래 Vivado Tcl/Makefile 중심 빌드

## 왜 FPGA 로직 셋업이 첫 단계인가

- 오버레이 기반 흐름에서 FPGA bitstream/HWH가 사용자 기능의 기반이기 때문
- 문서상 `base` overlay가 out-of-box 기준점이며, 변경/학습 시 Vivado 프로젝트 재생성이 필요
- 따라서 보드/FW/SW 설명 전에 "기준 하드웨어 설계(base overlay) 확인 및 재빌드 절차"를 먼저 고정하는 접근이 합리적

## 판단

질문하신 방향(프로젝트 목적을 기준으로 먼저 보드 FPGA 로직을 setup)은 맞다.  
권장 순서는 다음과 같다.

1. 보드 전원/부팅/연결 기본 확인
2. base overlay 기준 동작 확인(기본 bitstream)
3. Vivado로 base 프로젝트 생성(`base.tcl`)
4. bitstream 재빌드(`build_bitstream.tcl` 또는 `make`)
5. 이후 FW/SW 환경 및 상위 기능 문서 확장
