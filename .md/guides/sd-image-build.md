# SD Image Build Guide

## 목적

보드용 SD 카드 이미지를 재빌드할 때 필요한 준비사항과 실행 절차, 산출물 범위를 정리한다.

## 범위

- 대상: `RFSoC4x2`, `ZCU208`, `ZCU111` (루트 `README.md` 기준)
- 포함: Linux 호스트 준비, BSP 배치, `make BOARD=<BOARD>` 실행
- 제외: base overlay 내부 로직 수정 방법, 노트북 기능 테스트 상세

## 핵심 개념 (PL/FW/SW 관점)

- SD image 빌드는 **FW(PS) 중심** 절차다.
- 다만 이 저장소 `Makefile`은 이미지 빌드 전에 board별 `base.bit` 빌드를 먼저 수행하도록 연결되어 있어, 결과적으로 **PL 산출물도 함께 패키징 경로에 포함**된다.
- 따라서 SD image 빌드는 "FW only"라기보다, **PS 부팅 스택 + rootfs + (board 기본 PL 오버레이 산출물)**을 묶는 통합 빌드에 가깝다.

## 선행 조건

- Linux 호스트 (Ubuntu 22.04 권장)
- Vivado 2024.1, PetaLinux 2024.1 설치
- 저장소 clone (`--recursive`)
- 보드별 BSP 파일 확보

## 실행 도구 안내 (`make`)

- SD image 절차의 실행 진입점은 루트 `Makefile`이며, 명령은 `make BOARD=<BOARD>`를 사용한다.
- `make`는 저장소에 포함된 도구가 아니라 시스템에 별도 설치되어 있어야 한다.
- Windows PowerShell 기본 환경에는 `make`가 없을 수 있으므로, 필요 시 Linux/WSL 환경에서 실행한다.
- `make`는 현재 디렉터리의 `Makefile`을 기본 참조하므로, 반드시 저장소 루트에서 실행한다.

## 절차

### 1) 저장소 준비

```bash
git clone --recursive https://github.com/Xilinx/RFSoC-PYNQ.git
cd RFSoC-PYNQ
```

### 2) BSP 배치

```bash
cp <local-path-to-bsp> boards/<BOARD>/<BOARD>.bsp
```

### 3) SD image 빌드

```bash
make BOARD=<BOARD>
```

완료 시 루트 경로에 `<BOARD>-<VERSION>.img`가 생성된다.

## 결과물에 대한 실무 해석

SD image에는 일반적으로 다음 계층이 포함된다.

- PS 부팅/런타임에 필요한 이미지 구성 요소 (부트 관련 산출물, Linux 커널/디바이스 트리, rootfs)
- PYNQ 사용자 공간 패키지/환경
- 보드 기본 오버레이 운용에 필요한 파일(이 저장소 빌드 흐름상 `base.bit/.hwh` 포함 경로)

정확한 내부 파티션/파일 구성은 `pynq/sdbuild` 설정과 보드 spec에 따라 달라질 수 있다.

## 다른 가이드와의 경계

- PL(base overlay) 설계/재빌드 상세: `fpga-board-vivado-setup.md`
- 보드 반영/로딩 안정화: `fw-setup-deep-dive.md`
- 기능 검증(노트북): `sw-setup-deep-dive.md`
