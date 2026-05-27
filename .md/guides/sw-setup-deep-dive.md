# SW Setup Deep Dive

## 목적

보드에서 overlay 기능 검증을 수행하기 위한 소프트웨어 환경 준비와 테스트 실행 절차를 정리한다.

## 범위

- Python/Notebook 실행 환경 확인
- 의존성 점검
- 기능 테스트 시나리오 실행
- 제외: Vivado 프로젝트 생성/bitstream 빌드, overlay 파일 배치 및 로딩

## 선행 문서

- 빌드 절차: `fpga-board-vivado-setup.md`
- overlay 반영/로딩: `fw-setup-deep-dive.md`
- 전체 단계: `guides/README.md`

## 단계

### 1) 실행 환경 점검

- 보드에서 Python/Jupyter 접근 확인
- 작업 디렉터리 및 노트북 위치 확인
- 네트워크/스토리지 상태 점검

### 2) 의존성 확인

- 노트북에서 사용하는 패키지 import 확인
- 보드/이미지 버전과 예제 호환성 확인
- 필요 시 사용자 패키지 설치 및 버전 고정

### 3) 기본 기능 테스트

- GPIO/레지스터 관련 노트북 실행
- 각 노트북의 핵심 셀 기준 pass/fail 기록
- 실패 시 즉시 로그/스크린샷 확보

권장:

- `buttons_leds_switches.ipynb`
- `register_map_intro.ipynb`
- `pl_ddr4.ipynb`
- `syzygy_loopback.ipynb`

### 4) RFDC 기능 테스트

- RFDC 노트북 순차 실행
- 캡처 데이터, 스펙트럼, sweep 결과 확인
- 재실행 시 동일 결과 재현 여부 점검

권장 순서:

1. `01_rf_dataconverter_introduction.ipynb`
2. `02_rf_spectrum_analysis.ipynb`
3. `03_rf_spectrum_sweep.ipynb`

### 5) 결과 정리

- 테스트 항목별 pass/fail 테이블 작성
- 실패 항목 재현 절차와 원인 가설 기록
- FW/HW 영향 여부를 분리해 이슈 등록

## 완료 기준

- 기본 기능 + RFDC 핵심 시나리오 통과
- 실패 항목은 모두 재현/로그/다음 액션이 기록됨

## 체크리스트

- [ ] 실행 환경/의존성 확인
- [ ] 기본 기능 노트북 테스트 완료
- [ ] RFDC 노트북 테스트 완료
- [ ] 결과 및 이슈 기록 완료
