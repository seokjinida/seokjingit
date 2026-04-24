# Neon Pop Sweeper - 작업 문서 인덱스

현재 문서 기준 버전: **v1.3 (hypotheses synced)**

이 프로젝트는 **마스터 1개 + 보조 4개** 구조로 관리합니다.

## 문서 구성

1. `01_GAME_SPEC.md`
   - 게임 기획/수치/UX/검증 기준의 단일 기준 문서(SoT, Source of Truth)

2. `02_IMPLEMENTATION_RULES.md`
   - AI 에이전트가 임의 해석하지 못하도록 막는 고정 룰
   - 금지사항, 예외 처리, 성능 하한선

3. `03_AGENT_EXECUTION_PROMPT.md`
   - 실제 코딩 에이전트에 바로 전달할 실행 프롬프트
   - 산출물 형식, 단계별 구현 순서, 완료 조건 포함

4. `04_FUN_HYPOTHESES.md`
   - 핵심 재미 가설(H1~H6)과 검증 질문

5. `05_VALIDATION_CRITERIA.md`
   - 프로토타입 완성 여부 판단용 기능 체크리스트/판정 규칙

## 권장 사용 순서

1) `01_GAME_SPEC.md`로 게임 이해  
2) `04_FUN_HYPOTHESES.md`로 재미 가설 확인  
3) `05_VALIDATION_CRITERIA.md`로 검증/판정 기준 확인  
4) `02_IMPLEMENTATION_RULES.md`로 고정 규칙 확인  
5) `03_AGENT_EXECUTION_PROMPT.md`를 그대로 전달해 구현 시작

## 변경 관리 원칙

- 수치/룰 변경은 반드시 `01_GAME_SPEC.md` 먼저 수정
- 재미 가설 변경은 `04_FUN_HYPOTHESES.md` 먼저 수정
- 검증 기준 변경은 `05_VALIDATION_CRITERIA.md` 먼저 수정
- `02`, `03`은 `01/04/05`를 따라 동기화
- 구현 중 임의 변경 금지 (변경 필요 시 문서 선수정)
