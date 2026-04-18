# Design Review Report R1 — Design Spec v1 ([PRODUCT] v2)

> **📝 샘플 문서** — 이 파일은 Multi-agent Producer-Reviewer 파이프라인의 실제 산출물 예시로 공개된 샘플입니다. 원본에 있던 사업 구체 내용(목표 수치, 수익 모델, 매체명, 경쟁사 포지셔닝, 퍼소나 시나리오 등)은 플레이스홀더로 일반화되거나 생략되어 있습니다.

> 리뷰: Design Red Team · 라운드 1 · 2026-04-13 · 대상 `_workspace/phase2/design.md`
> 기준: PRD Grade A−(`handoff/prd-final.md`), Phase 1 핸드오프 메시지 6항, `docs/<project>/<doc>.md` §8 토큰.
> 이전 테마 파일 덮어쓰기.

---

## 종합 등급

**B+ (Grade B 상단)** — Critical 1, Major 4, Minor 6.

Phase 1 핸드오프 6항 중 5항을 실질 반영했고 PRD FR 22개 중 22개가 UI에 **표면상 매핑**됐으나, (a) FR-A5.5의 **공개 뷰 실효성이 미흡**하고(Critical), (b) `cc-nd-linked` variant의 locale 판정 로직·FR-A0 3축 검증 UI 분리·Compliance Dashboard 미설계·OG 정책 미종결 등 Major가 2건을 초과하여 Grade A 기준(Critical 0, Major ≤ 2) 미충족. **라운드 2 1회**로 Grade A 복귀 가능.

---

## PRD FR 커버리지 매트릭스

| PRD FR | 매핑 컴포넌트/화면 | 평가 |
|--------|-------------------|------|
| FR-A0 번역 금지 | §5.13 `LeadEchoEditor`(인접), §12 수용기준 | 3축 중 1축만 분리, 나머지 2축 1줄로 통합 → Major M1 |
| FR-A1 상한 2축 | §11.2 CMS 중앙 패널 자수 카운터 | 한 축만 명시. 다른 축 누적 시각화 UI 없음 → Minor m1 |
| FR-A2 감지 차단 | §5.13 `LeadEchoEditor` | 반영 |
| FR-A3 단독소스 강등 | §5.2 `tier2-short` variant (`isSoloSource`) | 반영 |
| FR-A4 content_type 분류 | §5.2 variant 규칙 + §11.2 수동 오버라이드 | 반영 |
| FR-A5 유사도 검사 | §5.8 `SimilarityWarning` severity 3단계 | 반영 |
| FR-A5.5 매체-팩트 귀속 | §5.7 `[AngleBlockComponent]`(매체 근접 금지), §5.12 `[AttributionViewComponent]`, §3.1 IA "AI 종합" 라벨 언급 | **공개 뷰에 "AI 종합(매체 원문 아님)" 분리 라벨 컴포넌트 미명세** → Critical C1 |
| FR-A6 CC-ND 차단 | §5.2 `cc-nd-linked` variant | "원문 언어 vs locale 불일치" 판정 로직 미명시 → Major M2 |
| FR-A7 감사 로그 | §12 언급만 | Dashboard 화면 자체 부재 → Major M3 합산 |
| FR-B1 Tier 분기 | §5.2 `LicenseVariant` 5종 | 반영 |
| FR-B2 Outlink 프라이머리 | §5.3 `OutlinkCTA` 3 variant + §6.1 + §7 sticky | 반영 |
| FR-B3 LicenseBadge + SourceMeta | §5.1, §5.4 | 반영 |
| FR-B4 SimilarityWarning | §5.8, §11.1 | 반영 |
| FR-B5 공유 재설계 | §5.10 `ShareSheet v2` + OG 계약 | OG 정책이 §15 열린 이슈로 미결정 → Major M4 |
| FR-B6 투명성 footer | §5.6 `[AttributionLine]` + §5.11 `ComplianceFooter` | 반영 |
| FR-B7 차별화 섹션 전면화 | §3.1 IA + §5.7 `[AngleBlockComponent]` | 핸드오프 §2 "직후 하드 고정" 준수 |
| FR-C1 opt-out (본인확인 3단계) | §5.9 `[CorrectionFormComponent]` + §6.3 | 반영 |
| FR-C2 correction SLA | §5.9 SLA 카운트다운 + §6.3 | 반영 |
| FR-C3 DMCA SLA | §5.9 + §4 화면 #8 | 대리인 등록 선행 타임라인 주석 누락 → Minor m2 |
| FR-C4 감사 | §12 수용 기준만 | Dashboard UI 부재 → Major M3 합산 |
| FR-C5 도메인 차단 | §6.3 플로우 C 후속 처리 | 반영 |
| FR-C6 Compliance Dashboard | §11 에디터 CMS 뷰 | §11.1 / §11.2 만 있고 **Dashboard 뷰 없음** → Major M3 |

**커버리지율**: 완전 13 / 부분 8 / 결함 1 = 22개 중 **완전 커버 59%**, 부분/결함 41%. Grade A("PRD FR 완전 커버") 기준 미달.

---

## 이슈

### Critical

#### C1. FR-A5.5 "매체명↔본문 시각 분리"의 공개 뷰 실효성 결함 — 분리 라벨 컴포넌트 미명세

- **위치**: `design.md` §3.1 IA 표 + §5.1 `SourceMeta` + §5.2 `NewsCard` 공통 시각 스펙, §12 FR-A5.5 수용 기준
- **문제**: PRD §FR-A5.5 구현과 §FR-B3은 "매체명과 본문 사이에 **분리 라벨 삽입**"을 명령형으로 규정한다. Phase 1 핸드오프 §3도 "카드 내에 분리 라벨 영역 확보"를 재확인했다. 그러나 본 Design Spec §5 컴포넌트 카탈로그에 **대응 컴포넌트가 전혀 없다**. IA 표에 문장으로만 언급될 뿐, 컴포넌트·시각 스펙·aria 롤·variant 불변성 명세가 빠져 구현 단계 누락 확률 높음.
- **추가 문제 (더 심각)**: §5.1 `SourceMeta`는 1픽셀 구분선 하나만 갖는다. IA가 "헤드라인 + 즉시 OutlinkCTA"를 같은 블록으로 묶으므로, 실제 렌더 흐름은 매체명 → (1px 선) → 헤드라인 + OutlinkCTA → 본문이 된다. 매체명과 본문 사이에 **브랜드-사실 분리 시그널이 1px 테두리 하나뿐**이며, 이는 FR-A5.5가 차단하려 했던 참조 판례 쟁점(독자가 "매체가 직접 서술한 것"으로 인식 → 상표권)을 그대로 재현할 위험.
- **근거**:
  - PRD FR-A5.5 "매체명과 본문 사이에 '정보원: 자체 분석'으로 명확히 구분"
  - PRD FR-B3 "매체명과 본문 사이에 분리 라벨을 삽입(FR-A5.5와 짝)"
  - Phase 1 핸드오프 R2 §3 "'AI 종합' 라벨 4요소 중 하나로 포함"
- **수정 방향**:
  1. §5에 신규 컴포넌트 `[AiLabelComponent]`(Server Component) 추가. `role="note"`, 배경 `accent-primary-soft` 또는 `license-linked-bg`, 아이콘 + 텍스트 병기(color-only 회피).
  2. 위치: `SourceMeta` row **아래**, `헤드라인 + OutlinkCTA` 블록 **위**. IA 표에 행 삽입.
  3. 4 variant에서 동일 위치·동일 시각 가중치 유지(variant 불변).
  4. `aria-label`·포커스 링·최소 터치 영역 + 툴팁 카피
  5. 수용 기준에 "공개 뷰에서 `SourceMeta` 다음 노드가 `[AiLabelComponent]`이어야 하며 Playwright DOM 순서 E2E로 검증" 추가.

### Major

#### M1. FR-A0 3축 검증 UI가 `LeadEchoEditor` 단일로 흡수 — 3축 개별 피드백 불가

- **위치**: §5.13 `LeadEchoEditor`, §12 수용 기준 FR-A0 행
- **문제**: FR-A0은 3개 축을 각각 자동 검증한다. 디자인에는 1개 축만 반영되고 나머지 2개는 "경고 배너로 표시"라는 한 줄로만 처리. 에디터가 어떤 축이 실패했는지 구분할 수 없어 판단 근거 부재.
- **근거**: PRD FR-A0 — 위반 시 파이프라인 자동 전달. 각 축이 독립 실패 가능하므로 독립 표시 필요.
- **수정 방향**: `SimilarityWarning` 또는 신규 `[TranslationWarningComponent]`에 3축 개별 표시.

#### M2. FR-A6 `cc-nd-linked` variant 진입 조건의 "locale 불일치" 판정 로직 미명시

- **위치**: §5.2 `LicenseVariant` 결정 규칙
- **문제**: 판정 주체(Server vs Client), 전제 필드 존재 여부, locale 전환 시 재렌더 정책 모두 미기재. 1회라도 ND 원문 번역본을 노출하면 FR-A6 위반 → 저작권 침해.
- **근거**: PRD FR-A6. NFR §6 캐시 네임스페이스에 locale 차원 누락 위험.
- **수정 방향**:
  1. `sources.language` 필드를 SSR props에 포함.
  2. 캐시 키를 locale 포함으로 확장.
  3. 필드 null/undefined 시 **안전 우선 강등**을 표 주석에 박을 것.
  4. locale 전환 시 재검증 명시.

#### M3. FR-A7·C4·C6 Compliance Dashboard 화면 부재

- **위치**: §4 화면 목록, §11 에디터 CMS 뷰, §12 FR-A7·C4·C6 행
- **문제**: §12 수용 기준 표가 해당 FR들을 "Dashboard 표"로 집합 매핑하지만, §4 화면 목록에 Dashboard 라우트 없음. §11은 Queue와 검수만 있고 Dashboard **별도 화면 미명세**. KPI·SLA 경고·감사 쿼리 UI 모두 실구현 경로 공백.
- **근거**: PRD 해당 FR들. 운영 가능성은 Dashboard 없이는 달성 불가.
- **수정 방향**:
  - §4 화면 목록에 Dashboard 추가.
  - §11에 신규 섹션 — KPI 카드 + Audit 필터 테이블 + 최근 경고 피드 + 증빙 다운로드. Server Component 위주.
  - §12 검증 방법을 "스크린샷 리뷰"에서 "Storybook + Playwright 단위 테스트"로 승격.

#### M4. FR-B5 OG 정책이 §15 "열린 이슈"로 미종결 — Phase 2 출구 차단

- **위치**: §5.10 `ShareSheet v2`, §13 트레이드오프, §15 열린 이슈
- **문제**: 차별화 요소를 OG 표면에 노출할지 alt text에만 둘지 결정이 Red Team에 떠넘겨진 채로 남아 있다. 공유 플로우의 CTA 기본값·제품 차별화·재배포 리스크 통제의 교차 결정이므로 핸드오프 전에 확정 필요.
- **Red Team 판정(본 리뷰)**:
  - 해당 섹션은 "[PRODUCT] 자체 저작물"이므로 OG 표면 1줄 노출에 **법적 리스크 추가 없음**. 우려는 법적 이슈가 아닌 **제품 차별화 해자 훼손**.
  - 해자 훼손 대응은 디자인 결정이 아닌 플랫폼 제약으로 부분 완화 가능 → 노출을 허용하는 결정 권고.
- **수정 방향**: §5.10·§15를 다음으로 확정.
  1. OG에 차별화 요소 1줄 + [PRODUCT] 배지. **매체 로고·이름 렌더 금지**(FR-A5.5 근접 배치 금지 원칙 연장).
  2. 응답 헤더 `X-Robots-Tag: noai, noimageai, noarchive`.
  3. `Cache-Control`로 스크랩 경제성 완화.
  4. §6.2 OG 계약에 "매체 로고·이름 렌더 금지"를 자동화 테스트에 추가.

### Minor

#### m1. §11.2 CMS에 누적 카운터 UI 미명시
FR-A1 2축 중 한 축 카운터만 있음. 에디터가 실시간으로 양적 경계를 볼 수 있어야 함.

#### m2. FR-C3 페이지 오픈 시점 vs 지정 대리인 등록 선행 주석 누락
Phase 1 핸드오프 R2 N3이 §4 화면 비고에 반영되지 않음. "릴리스 전 등록 완료 선행, 미완료 시 503 가드" 1줄 추가 권장.

#### m3. 신규 `warn-*` 3종 토큰이 CMS 에디터 전용임에도 공개 팔레트와 네임스페이스 구분 없음
네임스페이스 분리 또는 CSS 변수 접두어 명시 권장. 공개 뷰에서 실수로 임포트 시 사고 차단.

#### m4. sticky CTA 임계치 실측 근거 부재
§15 열린 이슈로 기록됐으나, Phase 2 산출물이 이미 interim UI A/B를 포함하므로 임계치 A/B도 같은 트랙에서 검증하도록 명시 권장.

#### m5. 차별화 섹션 고유 콘텐츠 비율 집계(R2 m5 잔존) 영역 DOM 마커 미명시
본문만인지 상단 라벨 포함인지 쿼리 가능해야 함. 시각 스펙에 DOM 마커 + 집계 대상 범위 주석 추가 권장.

#### m6. `SourceMeta` "매체명 클릭은 원문으로 이동하지 않음" — 대체 인터랙션 미정의
중복 동선 방지 의도는 타당하나, 클릭 no-op은 접근성 포커스 기대값을 깨뜨린다. 최소한 "커서 기본·포커스 제외"는 명시해야 함.

---

## 강점 (유지)

- **Variant 결정 규칙 완결성** — `license_tier × content_type × isSoloSource`로 5 variant 모호 없이 결정. 핸드오프 `LicenseVariant` 네이밍 반영.
- **Tier를 배경색으로 표현하지 않음** — WCAG SC 1.4.1 color-only 위반 선제 차단. 신규 토큰을 6종으로 억제.
- **interim UI A/B 2-cell** — Phase 1 R2 M6 잔존 이슈(CTR 베이스라인의 UI 효과 vs 콘텐츠 효과 인과 분리)를 제품 설계로 내재화. 공정이용 방어 증거 품질 확보.
- **OG 이미지 서버 레벨 가드** — 본문 문자열 차단을 자동화 테스트로 명시. 실효성 있는 재배포 리스크 차단.
- **`ComplianceFooter` 3칼럼** — 실명 + AI 고지 + 컴플라이언스 링크가 단일 지점 집약. FR-B6·NFR-L1 "보수 해석" 가정과 정렬.
- **접근성 명시도(§9)** — `role`·`aria-label`·대비비 수치·reduced motion까지 포함. AA 기준 맞춤.
- **`[CorrectionFormComponent]` 3-step 본인 확인** — Phase 1 m3 반영이 Wizard UI로 자연스럽게 매핑됨. SLA 카운트다운 색 단계화도 실효적.

---

## 최종 판정

- [ ] Grade A — 개발 핸드오프 가능
- [x] **Grade B(+) — 라운드 2 필요**
- [ ] Grade C/D — 방향 오류

Critical 1(C1) · Major 4(M1~M4)로 Grade A 기준(Critical 0, Major ≤ 2) 미달. 단 구조·방향·토큰 경제성은 견고하고 수정 범위가 **국지적**(컴포넌트 1개 추가 · variant 로직 명시 · Dashboard 화면 1장 · OG 정책 종결 · 에디터 배너 축 분리) → **라운드 2 1회로 Grade A 복귀 가능**.

---

## 다음 라운드 지시사항 (Design Blue Team)

1. **[C1 해소·최우선]** `[AiLabelComponent]` 컴포넌트 신규 명세 — §5 신설 + IA 표에 행 삽입. 카피·위치·아이콘·색 비의존·variant 불변성 5개 필수 요소 명세. 공개 뷰 E2E 수용 기준 추가.

2. **[M1 해소]** FR-A0 3축 개별 검증 UI — `SimilarityWarning` 확장 또는 `[TranslationWarningComponent]` 신규. 3축 각각 표시.

3. **[M2 해소]** `cc-nd-linked` 진입 판정 — (a) `sources.language` 데이터 계약 명시, (b) 캐시 키 locale 확장, (c) 미상 시 안전 강등을 주석으로 박기.

4. **[M3 해소]** Dashboard 화면 신설 — §4 라인 추가 + §11 신규 섹션. KPI 카드 + Audit 필터 테이블 + 경고 피드.

5. **[M4 해소]** OG 정책 확정 — "차별화 요소 1줄 렌더 · 매체 로고·이름 금지 · `X-Robots-Tag: noai` · 캐시"로 §5.10·§15 종결. §6.2 OG 자동화 테스트 케이스 추가.

6. **[Minor m1~m6 일괄]** §11.2 카운터, §4 화면 DMCA 503 가드 주석, `warn-*` 토큰 네임스페이스 분리, interim UI sticky 임계치 A/B, 차별화 섹션 DOM 마커, 매체명 인터랙션 명시 — 각 1~2줄 추가로 해결 가능.

라운드 2 제출 시 위 6건(특히 1·3·4·5) 반영 확인 후 Grade A 판정 예정. Phase 1 잔존 M6는 본 스펙에 이미 포함되어 별도 지시 없음. §NFR-L1 법률 자문은 본 디자인 리뷰 범위 외 — 기존 일정 유지.
