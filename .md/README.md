# 코드 분석 문서 폴더

이 폴더는 프로젝트 코드를 분석한 Markdown 문서를 모아두는 공간입니다.

## 기본 구조

- `INDEX.md`: 전체 문서 인덱스
- `modules/`: 모듈/디렉터리 단위 분석
- `features/`: 기능 단위 분석
- `guides/`: 보드/FW/SW/빌드 환경 설정 가이드
- `issues/`: 이슈/디버깅 기록
- `templates/`: 문서 템플릿

## 분류 기준

- `modules/`: 코드 구조 중심 분석 (예: `boards/`, `docs/`, `sdbuild/`)
- `features/`: 사용자 관점 기능 분석 (예: base overlay, spectrum analyzer)
- `guides/`: 실행 절차/환경 구축 (예: 보드 셋업, Vivado 설치/빌드, FW/SW 설정)

## 작성 규칙 (간단 버전)

- 파일명은 소문자-kebab-case 사용 (예: `auth-flow.md`)
- 문서 첫 부분에 목적/범위/관련 파일 경로를 짧게 기록
- 중복 내용은 `INDEX.md`에서 링크로 연결
