# guides

보드 셋업, FPGA/Vivado 빌드, FW/SW 환경 구성 등 "실행 절차형 문서"를 저장한다.

문서 역할 분리:

- `board-bringup-quickstart.md`: 보드 초기 bring-up/접속 확인 전용
- `fpga-board-vivado-setup.md`: Vivado 프로젝트 생성 + `.bit/.hwh` 빌드 전용
- `sd-image-build.md`: SD 이미지(PS 중심 통합 이미지) 빌드 전용
- `fw-setup-deep-dive.md`: 보드 반영 + overlay 로딩 안정화 전용
- `sw-setup-deep-dive.md`: Python/Notebook 기반 기능 테스트 전용
- `common-troubleshooting.md`: 공통 장애 대응 전용
- `board-variants-notes.md`: 보드별 차이/적용 주의사항 전용

권장 진행 순서:

1. `board-bringup-quickstart.md`
2. `fpga-board-vivado-setup.md` (PL 산출물 준비)
3. `sd-image-build.md` (필요 시, 전체 이미지 재빌드)
4. `fw-setup-deep-dive.md`
5. `sw-setup-deep-dive.md`

빠른 의사결정 트리:

- 보드에서 기본 이미지가 정상 부팅되고, PL만 수정했는가?
  - 예: `fpga-board-vivado-setup.md` -> `fw-setup-deep-dive.md` -> `sw-setup-deep-dive.md`
  - 아니오: SD 이미지까지 갱신 필요 여부 확인
- 커널/드라이버/rootfs/PYNQ 패키지 변경이 필요한가?
  - 예: `sd-image-build.md` 수행 후 FW/SW 검증 진행
  - 아니오: overlay 경로만 진행

Go/No-Go 게이트:

- [ ] Vivado 프로젝트 생성 성공 (`source ./base.tcl`)
- [ ] Bitstream/HWH 생성 성공 (`source ./build_bitstream.tcl` 또는 `make`)
- [ ] Overlay 로딩 성공
- [ ] 기본 기능 테스트 통과
- [ ] RFDC 테스트 통과(또는 이슈화)
- [ ] 결과/이슈 기록 완료

기존 프로젝트 문서 대비 추가 후보:

- (현재 없음)
