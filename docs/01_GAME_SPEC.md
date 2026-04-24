# Neon Pop Sweeper (Dynamic Vision) - Game Spec v1.1 (Current Build)

이 문서는 `index.html` 최신 구현 상태를 기준으로 동기화한 스냅샷입니다.

## 1. 게임 개요

- Title: Neon Pop Sweeper (Dynamic Vision)
- Genre: Hyper-Casual Collection & Competition
- Session Time: 정확히 70초 (`70000ms`)
- Platform: HTML5 Canvas (Desktop + Mobile Web)
- 승리 조건: 제한 시간 종료 시 최고 점수

## 2. 현재 Core Loop

1. 시작 패널에서 조작 안내 확인 후 `Start Game`
2. 조작(드래그 조이스틱 또는 WASD/방향키)으로 이동
3. 네온 스퀘어 닷 수집 -> 점수/콤보 상승
4. 캐릭터 크기 성장 + 카메라 동적 줌 적용
5. AI 3인과 실시간 점수 랭킹 경쟁
6. 70초 종료 시 결과 패널 + 재시작

## 3. 월드/엔티티 스펙

### 맵
- 크기: `2000 x 2000`
- 카메라 클램프로 맵 밖 미노출 보장

### 플레이어/봇 공통
- 기본 이동속도: `220 units/s`
- 반지름: `(15 + combo * 1.0) * 2.0`
- 콤보 최대: `50`
- 공격/충돌 페널티 없음(겹침 허용)

### 플레이어/봇 차등 밸런스
- 플레이어 속도 배수: `1.0`
- 봇 속도 배수: `0.88`
- 봇 조향: 목표 방향 즉시 전환(응답 패널티 없음)
- 플레이어 획득 배수: `1.82`
- 봇 획득 배수: `0.9`
- 획득 범위는 캐릭터 스케일과 함께 커지되, 캐릭터 외곽과 획득 경계 간격은 고정 오프셋 방식

### 닷(Dot)
- 형태: 네온 사각형
- 초기 생성 수: `220`
- 유지 범위: `220~280`
- 리스폰 검사 주기: `0.25s`
- 스폰 제한: 모든 캐릭터 기준 반경 `120` 이내 금지

| Type | Spawn Ratio | Base Score | Combo Gain | Extra |
|---|---:|---:|---:|---|
| normal | 85% | +10 | +1 | - |
| bonus | 12% | +25 | +2 | - |
| fever | 3% | +40 | +4 | 피버 2.5초 |

## 4. 점수/콤보/연출 규칙

### 점수
- 수집 시: `score += baseScore + combo * 2`

### 콤보
- 수집 시: `combo = min(50, combo + comboGain)`
- 콤보 감쇠: 마지막 수집 후 `1.2초` 지나면 초당 `5` 감소
- 체인 연출 유지시간: `0.6초`
  - 0.6초 내 연속 수집 시 체인 증가
  - 0.6초 초과 시 체인 수 즉시 초기화

### 콤보 텍스트(중앙 상단 UI)
- 위치: 화면 중앙 상단 (`y=170`)
- 컬러 단계(15단위):
  - `1~15` 흰색
  - `16~30` 파랑
  - `31~45` 노랑
  - `46~60` 빨강
  - `61+` 보라
- 색 단계가 올라갈수록 크기 10%씩 증가
- 팝 스케일 연출 강화 적용

### 피버(2.5초)
- 수집 반경 `+35%`
- 이동속도 `+10%`
- 파티클 강화

## 5. 카메라 스펙 (현재 튜닝값)

- `targetZoomRaw = (1 / (1 + combo * 0.007)) * 1.3`
- 보간: `currentZoom = lerp(currentZoom, targetZoom, 0.1)`
- 줌 범위: `0.2 ~ 3.0`
- 시작 줌: `3.0` (초기 카메라를 더 가깝게)
- 월드 클램프:
  - `halfW = canvas.width / (2 * zoom)`
  - `halfH = canvas.height / (2 * zoom)`
  - `camX = clamp(player.x, halfW, MAP_W - halfW)`
  - `camY = clamp(player.y, halfH, MAP_H - halfH)`

## 6. 입력 스펙

- Pointer: `pointerdown`, `pointermove`, `pointerup`, `pointercancel`
- Keyboard: `WASD` + `Arrow Keys`
- 우선순위: 키보드 입력이 있으면 키보드 벡터 사용, 없으면 조이스틱 사용
- 조이스틱 최대 반경: `80px`

## 7. AI 봇 규칙

- 타깃 재선정 주기: `0.4초`
- 평가식:
  - `valueScore = dotBaseScore * 1.0`
  - `distScore = -distance * 0.08`
  - `catchupBonus = (botRank >= 3) ? 8 : 0`
  - `total = valueScore + distScore + catchupBonus`
- 선택된 목표로 조향 이동(관성 보간)

## 8. 렌더링/아트

- 배경: `#0b0b1a`
- 네온 연출: `globalCompositeOperation = "lighter"`, `shadowBlur 16` (저사양 10)
- 파티클: 최대 `140`, 수명 `0.35~0.6s`
- 1등 캐릭터 상단 금색 왕관
- 획득 시 화면 흔들림: 제거됨(현재 미사용)

## 9. UI/플로우

### 시작 전
- 시작 안내 패널 표시
- 조작법/라운드 안내 후 `Start Game`으로 시작

### 플레이 중
- 상단 중앙: 남은 시간(소수점 1자리)
- 우측 상단: 랭킹 보드(1~4위)
- 좌측 하단: 콤보/줌/점수

### 종료
- 조건: `elapsedMs >= 70000`
- 결과 패널: 최종 순위/점수, 플레이어 강조, `Restart`
- 동점: 먼저 해당 점수 도달한 참가자 우선

## 10. KPI 체크(현재 빌드 기준)

- [ ] 시작 패널 -> Start Game -> 정상 라운드 진입
- [ ] 조이스틱 + WASD/방향키 이동 모두 동작
- [ ] 네온 사각형 닷 수집/리스폰 정상 동작
- [ ] 콤보 체인 0.6초 규칙(초과 시 체인 초기화) 확인
- [ ] 콤보 색상/크기 단계 연출(15단위, 10% 증분) 확인
- [ ] 70초 정확 종료 및 결과 패널 표시
- [ ] Restart 후 초기화/재플레이 정상 동작
