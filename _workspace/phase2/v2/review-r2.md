# Design Review Report R2 — Design Spec v2

> **📝 샘플 문서** — 이 파일은 Multi-agent Producer-Reviewer 파이프라인의 실제 산출물 예시로 공개된 샘플입니다. 원본에 있던 사업 구체 내용(목표 수치, 수익 모델, 매체명, 경쟁사 포지셔닝, 퍼소나 시나리오 등)은 플레이스홀더로 일반화되거나 생략되어 있습니다.

> 리뷰: Design Red Team · 라운드 2 · 2026-04-13 · 대상 `_workspace/phase2/design.md` v2
> 기준: `_workspace/phase2/review-r1.md`(Critical 1·Major 4·Minor 6), PRD Grade A `handoff/prd-final.md`, 참조 가이드
> 이전 리뷰 파일 덮어쓰기.

---

## 종합 등급

**A (Grade A)** — Critical 0, Major 1(잔존 Advisory성), Minor 2. R1 지적 11건 중 11건 실질 반영(100%), PRD FR 22/22 완전 커버(100%). 신규 도입된 5건(라벨 컴포넌트·경고 컴포넌트·Dashboard·resolver·OG 정책) 모두 Next.js 15 App Router + 기존 헥사고날 아키텍처에서 구현 가능한 범위. **개발 핸드오프 가능**.

잔존 Major 1건은 "PRD 데이터 계약 마이그레이션"이 디자인 스펙 내 선언으로만 기록되고 PRD·DB 마이그레이션 파일 실 업데이트는 Phase 3 개발 트랙으로 넘어가는 **트랙 경계 이슈**이며, 설계 결함이 아니다. 라운드 3 불필요.

---

## R1 지적 반영 매트릭스

| R1 지적 | 등급 | 반영 | 위치 | 평가 |
|---|---|---|---|---|
| **C1. `[AiLabelComponent]` 분리 라벨 컴포넌트 미명세** | Critical | 반영 | §3.1 IA 행 신설 · §5.1.5 신규 컴포넌트 · §12 E2E · §14 변경 델타 | 5개 필수 요소(카피·위치·시각·variant 불변·aria) 전부 반영. 자동 검사 마커 + 5 variant Storybook 스냅샷 규정까지 추가. **양호**. |
| M1. FR-A0 3축 검증 UI 단일 흡수 | Major | 반영 | §5.13.1 `[TranslationWarningComponent]` 신규 · §11.2 중앙 패널 · §12 FR-A0 검증 격상 | 3-row 그리드로 축 독립. `LeadEchoEditor`는 단일 축 전용 유지. "이 축만 재검증" 버튼으로 부분 재계산도 명시. **양호**. |
| M2. `cc-nd-linked` locale 판정 로직 미명시 | Major | 반영 | §5.2 resolver 알고리즘 박스 · §6.4 locale 전환 플로우 · §10·§14 반영 · §15 운영 체크리스트 | 서버 사이드 결정 순서 코드 박스로 명시. `source_language` null → 안전 강등 주석. 캐시 키 확장 + 단위 테스트 수용 기준. **양호**. |
| M3. Compliance Dashboard 화면 부재 | Major | 반영 | §4 행 신설 · §11.3 `/cms/dashboard` 신규 섹션 · §12 격상 · §14 신규 파일 | KPI 카드 + Audit 필터 테이블(결함 노출) + 경고 피드 + 정정 큐 사이드 + 증빙 다운로드 + e-서명 hash까지 완전 명세. 권한 가드 · Storybook · Playwright E2E 수용 기준. **양호**. |
| M4. OG 정책 미종결 | Major | 반영 | §5.10 OG 사양 · §6.2 OG 계약 자동화 테스트 · §15 열린 이슈 종결 | R1 Red Team 권고 그대로 반영 — 차별화 요소 1줄 + [PRODUCT] 배지, 매체 로고·이름 렌더 금지, `X-Robots-Tag`, `Cache-Control`. 자동화 테스트 3종 수용 기준 명시. **양호**. |
| m1. 누적 카운터 UI 부재 | Minor | 반영 | §5.12 · §11.2 | source별 누적 진행 바, 3단계 색 전환. **양호**. |
| m2. DMCA 503 가드 주석 누락 | Minor | 반영 | §4 비고 · §15 운영 체크리스트 | 환경변수 게이트 + 503 정적 페이지 폴백 명시. **양호**. |
| m3. `warn-*` CMS 네임스페이스 분리 | Minor | 반영 | §10 토큰 표 prefix · §10 (m3) 블록 | 디렉토리 외 임포트 시 빌드 실패. ESLint/Tailwind safelist 격리. **양호**. |
| m4. sticky CTA 임계치 A/B 미명시 | Minor | 반영 | §6.1 W0 A/B 3-cell | interim UI 트랙에 흡수. 라운드 누수 없음. **양호**. |
| m5. 차별화 섹션 DOM 마커 부재 | Minor | 반영 | §5.7 `data-gv-content="angle"` 본문만 | 상단 라벨 제외 범위 주석. 비율 쿼리 가능. **양호**. |
| m6. 매체명 대체 인터랙션 미정의 | Minor | 반영 | §5.1 인터랙션 절 재작성 + §14 `SourceInfoDialog.tsx` 신규 | 다이얼로그 인터랙션. OutlinkCTA 동선 분리. **양호**. |

**반영률: 11/11 = 100%**. 피상적 반영 없음 — 모든 지적이 컴포넌트 신설·알고리즘 박스·수용 기준 격상 등 **실체 있는 수정**으로 이어졌다.

---

## FR 커버리지 검증 (PRD 22개 FR 전수)

| FR | 매핑 | 평가 |
|---|---|---|
| FR-A0 번역 금지 (3축 자동 검증) | §5.13.1 + §5.13 | 3축 독립 표시, 축별 재검증 버튼 |
| FR-A1 상한 2축 | §11.2 + §5.12 진행 바 | 2축 모두 UI 매핑 |
| FR-A2 감지 차단 | §5.13 | 반영 |
| FR-A3 단독소스 강등 | §5.2 resolver → `tier2-short` | 알고리즘 박스에 명시 |
| FR-A4 content_type 분류 | §5.2 + §11.2 수동 오버라이드 | 반영 |
| FR-A5 유사도 검사 | §5.8 3 severity | 반영 |
| FR-A5.5 매체-팩트 귀속 (상표권) | §5.1.5 `[AiLabelComponent]`(강제 슬롯) + §5.12 + §5.7 분리 + OG 매체명 금지까지 일관 | C1 해소, **공개 뷰·공유 경로까지 일관된 방어선** |
| FR-A6 CC-ND 차단 | §5.2 `cc-nd-linked` + resolver + §6.4 locale 재평가 플로우 + 캐시 키 확장 | M2 해소 |
| FR-A7 감사 로그 | §11.3 Audit 테이블 | M3 해소 |
| FR-B1 Tier 분기 | §5.2 `NewsCard` 5 variant | 반영 |
| FR-B2 Outlink 프라이머리 | §5.3 + §6.1 sticky-mobile + A/B 임계치 | 반영 |
| FR-B3 LicenseBadge + SourceMeta | §5.1 + §5.4 + §5.1.5 짝 | 반영 |
| FR-B4 SimilarityWarning/AttributionWarning | §5.8 + §5.13.1 CMS 전용 | 반영 |
| FR-B5 공유 재설계 | §5.10 + §6.2 OG 계약 | M4 종결 |
| FR-B6 투명성 footer | §5.6 + §5.11 3칼럼 | 반영 |
| FR-B7 차별화 요소 전면화 | §3.1 IA + §5.7 + DOM 마커 | 반영 |
| FR-C1 opt-out 3단계 | §5.9 + 포털 | 반영 |
| FR-C2 correction SLA | §5.9 카운트다운 | 반영 |
| FR-C3 DMCA SLA | §5.9 + 대리인 등록 503 가드(m2) | 반영 |
| FR-C4 감사 | §11.3 Audit 테이블 | M3 해소 |
| FR-C5 도메인 차단 | §6.3 플로우 후속 | 반영 |
| FR-C6 Compliance Dashboard | §11.3 KPI+테이블+피드+정정 큐 | M3 해소 |

**커버리지율: 22/22 = 100%**. v1 대비 완전 13 → 22 (+9). Grade A 기준("PRD FR 완전 커버") 충족.

---

## 잔존 이슈

### Critical (목표 0) — **0건**

### Major (목표 ≤2) — **1건 (Advisory, 트랙 경계)**

#### M1-r2. `sources.language` 컬럼·캐시 키 마이그레이션의 PRD·DB 실체 업데이트 트랙 위임

- **위치**: §5.2 "PRD 데이터 계약 후속 업데이트 필요", §15 운영 체크리스트
- **문제**: Design Spec v2는 resolver가 `sourceLanguage`에 의존하고 캐시 키를 locale 포함으로 확장한다고 선언하지만, 이는 **PRD·스키마·캐시 구현 파일의 실 업데이트를 전제**로 한다. 현재 `prd-final.md` §NFR은 아직 해당 컬럼을 명시하지 않았을 가능성이 높고, 기존 캐시 키가 locale 단일 차원이면 마이그레이션 없이 배포 시 교차 오염이 발생한다.
- **성격**: 설계 결함이 아니라 **트랙 경계 이슈**. Design 단계 산출물은 올바르게 선언했고, Phase 3 개발 트랙이 이 선언을 PRD·DB 마이그레이션·캐시 네임스페이스 변경·백필 스크립트로 실체화해야 한다.
- **권고**: §15에 있는 운영 체크리스트를 Phase 3 킥오프 게이트로 승격. 개발 핸드오프 메시지 항목 1로 이관.

### Minor — **2건**

#### n1. resolver 알고리즘의 `content_type` 분기 순서가 FR-A3과 FR-A4를 하나로 병합

- **위치**: §5.2 알고리즘 박스
- **문제**: PRD FR-A3은 "단독소스 강등", FR-A4는 "content_type 분류"로 독립 근거 문서. OR 단일 branch는 **강등 사유가 무엇인지 로그·테스트에서 역추적 불가**.
- **권고**: 반환 타입을 `{ variant, downgradeReason }`로 확장하고 DB 컬럼에 저장. Dashboard Audit 필터에 노출. 단위 테스트 판별력도 상승. 1줄 수정.

#### n2. `[AiLabelComponent]`의 `cc-nd-linked` variant 카피 불일치 가능성

- **위치**: §5.1.5 vs §5.2
- **문제**: 기본 분기 카피가 `cc-nd-linked`에 부적절 — 본질적으로 "AI 재서술"이 아니라 **"원문 링크 전용"**에 가깝다. 현재 카피대로면 독자가 오해할 수 있어 브랜드 인식까지 위반할 여지.
- **권고**: §5.1.5에 `cc-nd-linked` 전용 카피 분기 추가. 5 variant × 2 locale = 10개 스냅샷으로 Storybook 확장.

---

## 신규 이슈 (v2 수정 중 생긴 문제)

**없음**. v2의 5개 대규모 수정 모두 §10 토큰·§14 변경 델타·§12 수용 기준과 교차 정합. v1에서 유지 예정이던 강점 전부 보존. §13 트레이드오프는 컴포넌트 도입으로 인한 영향까지 선제 기록했다.

---

## 구현 가능성 검증 (Next.js 15 + 헥사고날)

| 신규 요소 | Next.js 15 / 헥사고날 적합성 | 비고 |
|---|---|---|
| `[AiLabelComponent]` Server Component | 적합 | `'use client'` 불필요. 툴팁은 Radix Tooltip(Client) 하위로만. |
| `[TranslationWarningComponent]` | 적합 | 평가 결과 props는 Server에서 계산, 재검증 버튼만 Client. |
| resolver 함수 | 적합 | 순수 함수. 포트·어댑터 영향 없음. 테스트 단순. |
| 캐시 키 locale 확장 | 적합 | Upstash 기반이면 key prefix만 변경 + 기존 키 invalidate. 단, **기존 PRD/NFR 업데이트 선행**(M1-r2). |
| Dashboard Server Component + 필터 Client | 적합 | URL search param 쿼리 반영 정상. 권한 가드는 middleware. |
| OG HTTP 헤더 | 적합 | route handler의 응답 헤더 설정 정상. Vercel Functions 기본 Node runtime 적합. |
| 환경변수 503 가드 | 적합 | middleware 또는 page-level 조건부 렌더 가능. |

모두 구현 가능. **Next.js 15 App Router + Server Component 중심 + Radix UI + Vercel OG 스택** 기존 선택과 정합.

---

## 강점 (v2 신규 포함, 유지 대상)

- **FR-A5.5 방어선의 일관성** — `[AiLabelComponent]`(공개 카드) + `[AngleBlockComponent]` 매체 로고·이름 금지(섹션) + OG 매체명 렌더 금지(공유 경로)까지 **3 계층 일관 적용**. 상표권 방어선이 공개→공유 전 경로를 커버.
- **resolver 알고리즘 박스** — 코드 스니펫으로 박아 모호 제거. 개발자가 그대로 구현 가능. 안전 우선 정책이 FR-A6 보수 해석과 일치.
- **`--cms-*` 네임스페이스 분리** — ESLint import 제약·Tailwind safelist 격리 2단 가드. 공개 뷰 누수 사고 선제 차단. 실무적으로 성숙한 설계.
- **Dashboard의 "데이터 결함 노출"** — null 필터를 Audit 테이블에 노출해 안전 강등이 영구 은폐되지 않도록 가시화. 단순 대시보드가 아닌 **데이터 품질 피드백 루프**.
- **interim UI A/B × sticky 임계치 A/B 합산** — 디자인 결정의 불확실성 2건을 같은 W0 트랙에서 소진. 라운드 누수 없음.

---

## 최종 판정

- [x] **Grade A — 개발 핸드오프 가능**
- [ ] Grade B — 라운드 3 필요
- [ ] Grade C/D — 에스컬레이션

근거: Critical 0, Major 1(Advisory/트랙 경계), Minor 2(핸드오프 메시지로 전달 가능). R1 반영률 100%, FR 커버리지 100%. 구현 스택 적합. 라운드 3 불필요.

---

## 개발 핸드오프 메시지 (Grade A 확정)

1. **[선행 마이그레이션 게이트]** Phase 3 킥오프 전에 (a) PRD §NFR·§데이터 계약에 `sources.language` 컬럼을 추가하고, (b) 스키마 마이그레이션 + 백필, (c) 캐시 키를 locale 포함으로 일괄 전환하고 기존 키를 invalidate하라. 이 3개가 완료되기 전 Dashboard Audit null 필터로 데이터 품질을 가시화하되, 공개 뷰 배포는 보류. (M1-r2)

2. **[`downgradeReason` 필드 확장]** resolver 반환을 `{ variant, downgradeReason }`로 바꾸고 DB 컬럼 신설. Dashboard 필터에 노출. 단위 테스트는 variant 단독이 아닌 (variant, reason) 쌍으로 단언. (Minor n1)

3. **[`[AiLabelComponent]` 카피 `cc-nd-linked` 전용 분기]** §5.1.5 카피 분기에 `variant === 'cc-nd-linked'` 케이스 추가. Storybook 스냅샷을 5 variant × 2 locale = 10개로 확장. (Minor n2)

4. **[운영 체크리스트 동시 착수]** 언어 백필 완료 + 캐시 키 전환 목표. DMCA 지정 대리인 등록 완료 목표(미완료 시 503 가드 활성). 둘 다 환경 플래그로 점진 롤아웃 가능하도록 feature flag 설정.

5. **[자동화 테스트 4종 선 세팅]** Playwright E2E 세팅 시 최우선: (a) `[AiLabelComponent]` 5 variant DOM 순서 검증, (b) OG 응답에서 본문·매체명·매체 로고 미포함 + 헤더 존재, (c) CC-ND 원문 × locale 변화 시 variant 결정, (d) Dashboard KPI 카드 클릭 시 테이블 자동 필터 연동. 이 4종이 통과하면 Design Spec v2의 법적 방어선 실효성 자동 검증된다.
