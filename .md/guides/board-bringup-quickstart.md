# Board Bring-up Quickstart

## 목적

새 보드를 처음 켜는 시점부터 PYNQ 접속 확인까지, 초기 bring-up 절차를 빠르게 점검한다.

## 대상

- `RFSoC4x2`
- `ZCU208`
- `ZCU111`

## 준비물

- 전원 어댑터, USB/UART 케이블, Ethernet 케이블
- SD 카드 (검증된 PYNQ 이미지)
- 호스트 PC (브라우저 + SSH 클라이언트)

## 빠른 절차

### 1) SD 카드/케이블 점검

- SD 카드 이미지 버전 확인
- SD 카드 삽입 후 DIP/부트 모드 점검
- 전원, USB/UART, Ethernet 연결

### 2) 부팅 확인

- 전원 인가 후 LED 상태/시리얼 로그 확인
- 커널 부팅 완료까지 대기 (RFSoC4x2 기준 약 40초 내외)
- 네트워크 링크 업 여부 확인
- RFSoC4x2 기준 정상 신호 예:
  - `DONE`/`INIT` LED 점등
  - 사용자 LED(LED0-LED3) 점등
  - OLED에 네트워크/IP 표시

### 3) 보드 접속 확인

- 브라우저에서 Jupyter 접속 확인
- SSH 접속 확인
- 기본 시스템 정보 확인 (호스트명, IP, 저장공간)
- 연결 방식별 기본 접속:
  - USB Ethernet Gadget 사용 시: `http://192.168.3.1/lab`
  - Ethernet 사용 시: OLED/네트워크에서 확인한 보드 IP로 접속
- 기본 로그인 비밀번호: `xilinx` (이미지/설정에 따라 다를 수 있음)

### 4) 기본 동작 스모크 체크

- 간단한 Python 셀 실행
- Overlay 로딩 가능 여부 확인
- 오류 발생 시 `common-troubleshooting.md` 참조

## 완료 기준

- 보드가 안정적으로 부팅됨
- Jupyter/SSH 접속 가능
- 기본 실행 환경이 정상 동작

## 다음 단계

- Overlay 빌드: `fpga-board-vivado-setup.md`
- SD 이미지 재빌드(필요 시): `sd-image-build.md`
- FW 반영/로딩: `fw-setup-deep-dive.md`
- SW 기능 테스트: `sw-setup-deep-dive.md`
