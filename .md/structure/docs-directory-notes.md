# docs Directory Notes

## 목적

`docs/` 폴더가 어떤 성격의 콘텐츠를 담고 있는지와 `.md/guides`와의 역할 경계를 정리한다.

## 범위

- 포함: Jekyll 사이트 구조, 주요 문서 카테고리, 네비게이션 데이터
- 제외: 각 문서의 상세 튜토리얼 내용 재작성

## 역할 요약

- `docs/`는 RFSoC-PYNQ 공개 문서 사이트 소스다.
- 사용자/교육/지원 중심 콘텐츠를 담으며, 빌드/검증 절차의 운영 문서는 `.md/guides`에서 관리한다.

## 주요 구성

- `_config.yml`, `_layouts/`, `_sass/`, `assets/`: 사이트 렌더링 설정/테마
- `_data/navigation.yml`: 상단 메뉴 구조 정의
- `index.md`, `getting_started.md`, `overlays.md`, `support.md`, `faqs.md` 등: 공개 안내 문서
- `rfsoc_4x2_base_overlay.md`, `rfsoc_4x2_getting_started.md`: 보드/오버레이 중심 설명 문서

## guides와의 경계

- `docs/`
  - 제품/보드 소개, 학습 자료, 공개 FAQ/지원 링크 중심
- `.md/guides/`
  - 저장소 클론 이후 내부 실행 절차(bring-up, 빌드, 반영, 테스트, 트러블슈팅) 중심

## 중복 방지 원칙

- `docs/`의 튜토리얼 내용을 `guides/`에 복사하지 않는다.
- `guides/`에서는 필요한 문서 경로만 링크하고 운영 절차에 집중한다.

## 관련 문서

- `repository-overview.md`
- `../guides/README.md`
