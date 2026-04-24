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

- 루트 `index.html`: **플레이 가능한 단일 파일** 프로토타입(Canvas 2D, 외부 라이브러리·네트워크 없음).
- 동작·수치는 `docs/01_GAME_SPEC.md` 등과 맞출 것; 불일치 시 스펙 문서를 먼저 수정한 뒤 코드 반영.

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
9. 조인/게임시작 다단계 UI, 전체화면 토글, `chaser`/`hazard` 닷, 수집 파티클 티어·거리 튜닝
10. 회귀 방지 보강:
   - 시작 패널 텍스트 중앙 정렬 고정
   - `게임 시작`(카드 내부) / `전체화면`(카드 외부 하단 중앙) 버튼 분리 배치 고정
   - 초기 레이아웃 단계 DOM 참조 누락으로 인한 `undefined.style` 크래시 방지 규칙 및 체크리스트 추가
11. 추가 회귀 방지 보강:
   - 시작 전(조인/시작연출 포함) AI 실제 이동 금지, 합류 슬라이드 인 연출만 허용
   - 랭킹 UI는 `playing`에서만 노출
   - 화면비 축소 시 UI 최소 스케일 고정 없이 비례 축소 유지
   - 콤보 텍스트는 `COMBO xN` 형식만 사용
   - 1등 왕관/닉네임 라벨 겹침 방지, 파티클/환원 분산 거리 가시성 강화
12. 스케일 로직 분리 보강:
   - 콤보는 연출/점수용으로 유지, 캐릭터 스케일과 분리
   - 캐릭터 스케일은 별도 성장값(`scaleLevel`)으로 관리하며 최대 `50` 고정
   - 스케일 증가량은 `comboGain × 0.35`로 축소 적용
   - 콤보는 0.6초 내 다음 닷 미획득 시 즉시 `0`으로 초기화(체인 동시 초기화)
   - 장시간 플레이/고콤보에서도 화면을 덮는 초대형 스케일이 발생하지 않도록 상한 강제
13. AI/닷 추가 튜닝:
   - 붉은 위험 닷(`hazard`) 동시 활성 최대 `3`개 제한
   - AI가 위험 닷 감지 시 일정 확률로 회피 이동 수행
   - 수집 파티클의 크기/확산 거리 추가 상향으로 타격감 강화

## 5) Git 상태 / 커밋

- 로컬 변경 후 `git status`로 확인(문서만 수정 시 커밋 메시지 예: `docs: sync with index.html build`)
- 최근 반영 커밋(요약):
  - `96c2590` Neon Pop Sweeper: docs split, join flow, chaser/hazard, VFX tuning.
  - `82cf240` Tune HUD layout and sync docs for web build.

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
