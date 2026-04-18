# PRD Common Rules (PM 공통 규칙)

PM Blue Team / Red Team 에이전트가 공통으로 준수하는 규칙을 정의한다.

---

## 파일 저장 위치

### 파이프라인 실행 시 (\_workspace/)

Orchestrator가 파이프라인을 실행할 때, 중간 산출물은 `_workspace/`에 저장한다.

| 산출물 유형   | 경로                              | 비고                        |
| ------------- | --------------------------------- | --------------------------- |
| PRD           | `_workspace/phase1/prd.md`        | Blue Team 최신본 (덮어쓰기) |
| Review Report | `_workspace/phase1/review-rN.md`  | 라운드별 별도 파일          |
| 최종 PRD      | `_workspace/handoff/prd-final.md` | Grade A 통과본 복사         |

### 독립 실행 시 (docs/)

Orchestrator 없이 단독 실행할 때, 산출물은 `docs/`에 저장한다.

| 산출물 유형       | 경로                                    | 예시                                         |
| ----------------- | --------------------------------------- | -------------------------------------------- |
| PRD               | `docs/prd/<기능명>.md`                  | `docs/prd/onboarding-flow.md`                |
| Problem Statement | `docs/problem/<문제명>.md`              | `docs/problem/checkout-drop-off.md`          |
| Decision Log      | `docs/decisions/<YYYY-MM-DD>-<제목>.md` | `docs/decisions/2026-04-04-auth-provider.md` |
| Validation Plan   | `docs/validation/<기능명>.md`           | `docs/validation/onboarding-flow.md`         |
| Review Report     | `docs/reviews/<산출물명>-r<라운드>.md`  | `docs/reviews/onboarding-flow-r1.md`         |

### 파일명 규칙

- **소문자 kebab-case** 사용: `payment-checkout.md` (O), `PaymentCheckout.md` (X)
- Decision Log는 날짜 접두사 필수: `YYYY-MM-DD-<제목>.md`
- Review Report는 라운드 접미사 필수: `-r1`, `-r2`, `-r3`

---

## 산출물 형식

- 모든 PM 산출물은 **Markdown**(.md) 파일로 작성한다.
- 각 산출물은 Blue Team `agent-project-manager.md`에 정의된 **Output Specifications** 템플릿을 따른다.
- 리뷰 리포트는 Red Team `agent-prd-reviewer.md`에 정의된 **Review Report Format**을 따른다.

---

## 태그 규칙

Blue/Red Team 간 커뮤니케이션에 사용하는 태그:

| 태그                 | 사용 주체  | 용도                                  |
| -------------------- | ---------- | ------------------------------------- |
| `[ASSUMPTION]`       | Blue Team  | 검증되지 않은 가정 표시               |
| `[TECH CHECK]`       | Blue Team  | 기술적 실현 가능성 확인 필요          |
| `[DESIGN CHALLENGE]` | Design팀   | PRD 방향에 대한 디자인 관점 이의 제기 |
| `[FIXED: PI-XX]`     | Blue Team  | Red Team 이슈 수정 완료               |
| `[REBUTTAL: RI-XX]`  | Blue Team  | Red Team 이슈에 대한 근거 있는 반론   |
| `[PRD ORIGIN]`       | Design Red | 디자인 리뷰에서 발견된 PRD 기인 이슈  |
| `[ALIGNMENT CHECK]`  | Red Team   | PRD 의도와 산출물 방향 불일치 감지    |

---

## 이터레이션 규칙

- **최대 3라운드** 내에 Grade A 도달을 목표로 한다.
- 3라운드 초과 시 근본 원인을 요약하여 에스컬레이션한다.
- 각 라운드의 Review Report는 별도 파일로 저장하여 이력을 보존한다.
- Blue Team 수정본은 동일 파일을 업데이트하되, 변경 이력은 git으로 관리한다.