# Handoff Notes (Neon Pop Sweeper)

## 1) 프로젝트 개요

- 프로젝트명: `Neon Pop Sweeper (Dynamic Vision)`
- 목표: MD 문서(`docs/01~05`)만으로 AI가 실행 가능한 `NeonPopSweeper.html` 단일 파일 프로토타입을 재현 가능하게 유지
- 저장소: `https://github.com/seokjinida/seokjingit.git`
- 배포 URL: `https://seokjinida.github.io/seokjingit/`
- 참고: `HANDOFF.md`는 내부 작업 이어받기용이며, 채용 담당자 전달 문서 범위는 `docs/01~05` 기준

## 2) 현재 문서 구조

- `docs/00_INDEX.md`: 문서 인덱스
- `docs/01_GAME_SPEC.md`: 게임 스펙/수치/플로우 기준 문서
- `docs/02_IMPLEMENTATION_RULES.md`: 구현 고정 룰
- `docs/03_AGENT_EXECUTION_PROMPT.md`: 에이전트 실행 프롬프트(한글)
- `docs/04_FUN_HYPOTHESES.md`: 핵심 재미 가설 및 검증 질문
- `docs/05_VALIDATION_CRITERIA.md`: 프로토타입 검증 기능 체크리스트/판정 규칙

## 3) 현재 구현 상태 (핵심)

- 루트 `NeonPopSweeper.html`: **플레이 가능한 단일 파일** 프로토타입(Canvas 2D, 외부 라이브러리·네트워크 없음).
- 최신 파일은 블라인드 재생성/후처리 과정을 거친 상태이며, 문서 기준 준수 여부는 추가 점검이 필요함.
- 불일치 발생 시 원칙:
  - 문서가 최신 의도라면 코드를 문서에 맞춤
  - 의도가 코드 기준으로 확정된 경우 문서를 선반영 후 코드 재동기화

## 4) 최근 주요 요청 반영 히스토리 요약

1. 문서 고도화(드리프트 방지 강화)
   - `docs/01_GAME_SPEC.md` `v1.17`
   - `docs/02_IMPLEMENTATION_RULES.md` `v2.6`
   - `docs/03_AGENT_EXECUTION_PROMPT.md` `v2.9`
   - `docs/05_VALIDATION_CRITERIA.md` `v2.9`
2. 고정값/고정문구/고정수식 명시 강화
   - 시작 설명/조작 문구 고정
   - 시작 카드-전체화면 간격 `8px`
   - `effectiveZoom = cameraZoom * view.scale` 체인 고정
   - 파티클 티어 절대값 고정
3. 최근 드리프트 이슈 대응 규칙 추가
   - 랭킹 패널 치수는 `measureText + padding` 강제
   - AI 조인 시작점은 게임뷰 외곽 기준 강제
   - UI 폰트 토큰(start title/desc/hint/button) 고정
   - 왕관-닉네임 최소 간격 `8px` 고정
4. 미준수 재발 방지 규칙 추가(블라인드 절차 문구 제외)
   - 조인 슬라이드 인은 상태 업데이트가 아니라 실제 가시 렌더 보장(`joining || joined`)
   - 닷 타입 스키마 5종 고정(`normal/bonus/fever/chaser/hazard`), 문서 외 타입(`refund` 등) 금지
5. 블라인드 재생성 실행 이력
   - 재생성 결과물에 HTML 이중 붙임 이슈(문서 2개 연속)가 발생했고 후처리로 정리 완료
   - 현재 파일은 단일 HTML 문서 구조로 복구됨

## 5) Git 상태 / 커밋

- 현재 워킹트리는 변경사항이 남아 있는 상태(문서 + HTML 수정 포함)일 수 있으므로 `git status` 우선 확인
- 권장 커밋 단위:
  1) 문서 동기화 커밋(`docs/*`)
  2) 실행파일 수정 커밋(`NeonPopSweeper.html`)
- 커밋 전 반드시 `docs/05` 기준 PASS/FAIL 기록

## 6) 재시작 시 권장 작업 순서

1. `docs/01~05` 버전/고정값 섹션 확인 (`01 v1.17`, `02 v2.6`, `03 v2.9`, `05 v2.9`)
2. `NeonPopSweeper.html`에서 아래 회귀 위험 2개를 최우선 점검
   - 조인 슬라이드 인 가시 렌더(`joining || joined`)
   - 문서 외 닷 타입 추가 여부(`refund` 등)
3. 기능 수정 시 문서-코드 동기화 순서 유지
   - 문서 확정 -> 코드 반영 -> `docs/05` PASS/FAIL 점검
4. 커밋은 문서/코드를 분리해 관리 권장
5. 필요 시 배포 반영 및 Pages 확인

## 7) 주의사항

- `file://` 직접 실행 시 브라우저 보안 경고가 나타날 수 있음(게임 로직 에러와 구분 필요)
- 웹 검증은 가능한 Pages URL 또는 로컬 서버(`http://localhost`) 권장
- 문서와 구현 수치가 달라지지 않도록 항상 동기화 유지
- 조인 연출은 “좌표 업데이트”만으로 PASS 처리하지 말고 “화면 가시성”을 반드시 확인
- 문서에 없는 타입/문구/레이아웃을 추가한 경우, 먼저 문서 반영 여부를 결정한 뒤 유지/삭제를 판단
