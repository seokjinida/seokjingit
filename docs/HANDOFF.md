# Handoff Notes (Neon Pop Sweeper)

## 1) 프로젝트 개요

- 프로젝트명: `Neon Pop Sweeper (Dynamic Vision)`
- 목표: MD 문서(`docs/01~05`)만으로 AI가 실행 가능한 `index.html` 단일 파일 프로토타입 재생성 가능해야 함
- 저장소: `https://github.com/seokjinida/seokjingit.git`
- 배포 URL: `https://seokjinida.github.io/seokjingit/`

## 2) 현재 문서 구조

- `docs/00_INDEX.md`: 문서 인덱스
- `docs/01_GAME_SPEC.md`: 게임 스펙/수치/플로우 기준 문서
- `docs/02_IMPLEMENTATION_RULES.md`: 구현 고정 룰
- `docs/03_AGENT_EXECUTION_PROMPT.md`: 에이전트 실행 프롬프트(한글)
- `docs/04_FUN_HYPOTHESES.md`: 핵심 재미 가설 및 검증 질문
- `docs/05_VALIDATION_CRITERIA.md`: 프로토타입 검증 기능 체크리스트/판정 규칙

## 3) 현재 구현 상태 (핵심)

- 파일: `index.html` (단일 실행 파일)
- 렌더링: Canvas 2D, 외부 라이브러리/에셋 미사용
- 입력: 포인터 조이스틱 + `WASD/방향키`
- 세션: 70초 정확 종료(`performance.now()` 기반)
- 월드: 2000x2000
- 카메라:
  - 동적 줌 + 보간
  - `16:9` 고정 게임 뷰(레터박스)
  - 화면 축소 시 월드/UI/조이스틱 동시 스케일링
- 오브젝트:
  - 네온 사각형 닷
  - 타입 비율/점수/콤보 반영
- 콤보 연출:
  - 체인 윈도우 0.6초
  - 중앙 상단 체인 텍스트
  - 15단위 색상 단계 + 단계별 크기 10% 증가
- UI:
  - 시작 패널/결과 패널
  - 우측 상단 랭킹 패널 20% 확대
  - 좌하단 상태 패널 제거
- 안정성:
  - 시작 전 렌더 안전 처리(초기 엔티티 + 방어)
  - pointer cancel/focus blur 예외 처리

## 4) 최근 주요 요청 반영 히스토리 요약

1. 기획/룰/프롬프트 문서를 분리 구성
2. 프로토타입 구현 및 반복 밸런스 조정
3. 조이스틱 + 키보드 입력 동시 지원
4. 닷/이펙트/콤보 연출 다수 튜닝
5. 모바일 시점 이슈 대응:
   - 고정 비율 게임 뷰
   - 전체 스케일 동기화
   - 오버레이(시작/결과) 스케일 동기화
6. 재생성 검증 과정 중 발생한 시작 전 에러 방지 규칙을 MD에 반영
7. HUD 요청 반영:
   - 랭킹 20% 확대
   - 좌하단 상태 UI 제거
8. 문서 구조 개선:
   - 핵심 재미 가설 문서 분리(`04`)
   - 검증 기준(기능 체크리스트/판정 규칙) 문서 분리(`05`)

## 5) Git 상태 / 커밋

- 로컬 상태: clean (`git status` 변경 없음)
- 최근 반영 커밋(요약):
  - `82cf240` Tune HUD layout and sync docs for web build.
  - `7b02790` Update documentation for regenerated build stability.
  - `5d10ec1` Regenerate index.html from current MD spec.
  - `b8c3a22` Add Neon Pop Sweeper playable prototype and synced docs.

## 6) 재시작 시 권장 작업 순서

1. `docs/01~05` 최신값 확인
2. `index.html` 실행 후 체감 검증
3. 변경 발생 시:
   - 코드 수정
   - 문서 동기화(`01`, `02`, `03`, `04`, `05`)
   - 커밋
4. 웹 반영 필요 시 `git push origin master`
5. Pages 반영 확인 (`https://seokjinida.github.io/seokjingit/`)

## 7) 주의사항

- `file://` 직접 실행 시 브라우저 보안 경고가 나타날 수 있음(게임 로직 에러와 구분 필요)
- 웹 검증은 가능한 Pages URL 또는 로컬 서버(`http://localhost`) 권장
- 문서와 구현 수치가 달라지지 않도록 항상 동기화 유지
