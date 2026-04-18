# Design Review Report — R3 (v4.1 — Perspective Level 1/2 Restructure)

> **📝 샘플 문서** — 이 파일은 Multi-agent Producer-Reviewer 파이프라인의 실제 산출물 예시로 공개된 샘플입니다. 원본에 있던 사업 구체 내용(목표 수치, 수익 모델, 매체명, 경쟁사 포지셔닝, 퍼소나 시나리오 등)은 플레이스홀더로 일반화되거나 생략되어 있습니다.

## Meta

- **산출물**: [PRODUCT] Design v4.1 — Screen 2 비교 뷰 강화 (클러스터 요약 스트립 + Level 2 브랜딩), 카드 CONTEXT 기본 열림 정책 반영
- **Blue Team 버전**: v4.1 R3 Round 3 (R1 B → R2 초안 → R3 스코프 조정)
- **리뷰 일시**: 2026-04-11
- **최종 등급**: **A (Pass)**
- **판정**: 개발 핸드오프 진행

---

## Round 3 스코프 조정 (PM 피드백 반영)

### 변경 배경

R3 Round 1에서 카드에 Level 1 클러스터 콘텐츠(요약 스트립, 리스트, 인라인 배너)를 임베드했으나, PM 피드백으로 철회:

> "context 는 열림이 기본이야. 홈화면은 유지하고 다관점 화면만 업데이트해"

### PM 판단의 근거 (Orchestrator 해석)

1. **홈화면 정보 밀도 보호**: 카드는 이미 전체 포맷을 표시한다. 여기에 추가 컴포넌트까지 얹으면 카드 높이가 2배 가까이 늘어나 핵심 사용 시간 성공 지표와 충돌.
2. **Level 1/2 경계의 UX 계층화**: Level 1은 "빠른 스캔 중 가능성 인지" 목적, Level 2는 "의도적 탐색" 목적. 두 맥락을 하나의 카드에 겹치면 사용자 의도가 흐려진다. Level 1은 링크 카운트로 충분한 진입점 역할을 하고, 클러스터 시각화는 Screen 2로 이관.
3. **CONTEXT 기본 열림**: 기존 CTA 의존 구조는 "배경 맥락은 옵션"이라는 신호. v4.1은 포맷의 CONTEXT를 기본 노출해 "배경을 아는 상태로 읽는다"는 제품 약속을 시각적으로 강화.

---

## 변경 범위 요약

### 1. NewsCard — 홈화면 원복 + CONTEXT 기본 열림

- **Round 1 추가 요소 전체 제거**:
  - ClusterSummaryStrip 삭제
  - ClusterList (+ ClusterFooter, blindspotInline, level2Link 포함) 삭제
- **OTHER VIEWS 라벨 원복**
- **CONTEXT 기본 열림** [NEW]:
  - ContextExpandable 내부에 Q&A 본문 추가
  - CTA 텍스트를 열림 상태 반영으로 변경
  - 패딩·gap 가독성 확보

### 2. Screen 2 비교 뷰 — Level 2 화면 강화

- **Level 2 브랜딩 유지**: Level2Tag (다크 pill + 아이콘)
- **클러스터 요약 스트립 신규**: 헤더 상단에 배치
  - 매체 수 (13px, 600 weight)
  - 세로 divider
  - 편향 분포 (9px 원 + 12px 텍스트)
  - 세로 divider
  - 분산도 accent color 메타 텍스트
  - Surface secondary 배경 + 1px 보더로 카드형 강조
- **배너 문구 정합화**: ClusterStrip의 숫자와 정합성 확보

### 3. 부속 요소 제거

Round 1에서 생성한 다음 요소들은 카드 클러스터의 부속물이므로 일괄 삭제:
- Level 1 Cluster Popover
- Mobile Level 1 Cluster
- State · Single Source (Cluster Unavailable)

---

## Iteration History

| Round | 등급 | Critical | Major | Minor | 판정 |
|-------|------|----------|-------|-------|------|
| R1 | A | 0 | 0 | 2 | Pass (초기 Phase 2) |
| R2 | A | 0 | 0 | 2 | Pass (R2 반영) |
| R3 Round 1 | B | 0 | 2 | 4 | Revise (v4.1 — 클러스터 임베드 안) |
| R3 Round 2 | A | 0 | 0 | 0 | Pass — 이슈 해소 완료했으나 PM 스코프 재결정 |
| **R3 Round 3** | **A** | **0** | **0** | **0** | **Pass — 홈화면 유지 + Screen 2 집중 스코프** |

---

## Review Checklist Summary

| 관점 | 상태 | 비고 |
|------|------|------|
| User Flow Integrity | pass | 홈 → 링크 → Screen 2 단일 플로우로 단순화 |
| State Completeness | pass | 카드가 원래 구조로 돌아가 기존 상태 정의 그대로 유효 |
| Design System Consistency | pass | Screen 2 ClusterSummaryStrip이 기존 컴포넌트와 일관된 dot+text 패턴 사용 |
| Accessibility | pass | dot 크기·line-height 유지 |
| Copy & Content | pass | 라벨 원복, 배너 숫자 정합성 확보 |
| Technical Feasibility | pass | Screen 2 한 곳에 클러스터 로직 집중 → 개발 착수 범위 명확 |

---

## Strengths

- **스코프 조정의 명확성**: "홈화면은 깊이 읽기, 비교 뷰는 탐색"이라는 맥락 분리가 사용자 의도와 일치. UI 부담 제거로 카드 렌더링·로딩 최적화 여지 확보.
- **CONTEXT 기본 열림의 메시지**: "배경을 아는 상태로 읽는다"는 포맷의 철학을 시각적으로 강화. Q&A 형식 본문은 기존 CTA보다 더 직접적으로 사용자 학습 동기를 유발.
- **Screen 2 ClusterSummaryStrip의 정보 밀도**: 한 줄에 클러스터 전체 분포 + 분산 메타 데이터를 담는다. 사용자가 3초 내 "이 이슈는 어느 축으로 치우쳤는가"를 파악 가능.
- **배너 정합성**: 클러스터 스트립 숫자와 배너 문구가 동일한 숫자 세트를 참조하므로 개발 구현 시 단일 데이터 소스에서 렌더링 가능.
- **기존 Phase 2 산출물 보존**: 홈화면, 모바일 변형, 상태 프레임, 공유 바텀시트, OG 카드, 온보딩 후속 단계 등 기존 디자인 자산은 전부 변경 없이 유지. 개발 핸드오프 연속성 확보.

---

## Issue Resolution Status

| # | 코드 | 상태 | 해소 방법 |
|---|------|------|----------|
| Round 1 DI-01 | Major | **Superseded** | 카드 클러스터 삭제로 Flow Gap 자체가 소멸. Level 1 → Level 2는 단일 링크로 일원화. |
| Round 1 DI-07 | Major | **Superseded** | 배너 로직은 Screen 2 전용. 개발은 Screen 2 한 곳에만 임계값 조건을 구현. |
| Round 1 DI-02 | Minor | **Superseded** | Single Source 상태도 Screen 2 컨텍스트에서만 발생. 카드 상태 정의 불필요. |
| Round 1 RI-02 | Minor | **Superseded** | Mobile 카드 클러스터 대응 불필요. Screen 2 Mobile 변형은 기존 그대로 유지. |
| Round 1 DI-05 | Minor | **Resolved** | 라벨 원복. |
| Round 1 DI-03 | Minor | **Superseded** | Popover 삭제로 CTA 계층 불일치 이슈 자체 소멸. |

**R3 Round 3 잔여 이슈: 0건.** 모든 이슈가 해소 또는 스코프 조정으로 superseded.

---

## Next Steps (개발 핸드오프)

- [x] Grade A 도달
- [x] design-final.pen 업데이트 완료
- [x] 주요 스크린 PNG 재내보내기
- [x] handoff/design-final-path.md 갱신
