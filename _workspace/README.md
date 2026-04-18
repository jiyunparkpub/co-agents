# _workspace

에이전트 파이프라인의 실제 산출물이 쌓이는 공간입니다. `agent-orchestrator.md` 는 각 페이즈의 출력을 아래 경로에 저장하도록 명시하고 있습니다.

## 구조

```
_workspace/
├── input/
│   ├── v1/brief.md                # 제품 과제·참조 자료·비즈니스 목표·제약을 정리한 최초 Brief
│   └── v2/brief.md                # 리비전된 Brief
├── phase1/                        # PRD (PM Blue/Red Team)
│   ├── v1/
│   │   ├── prd.md                 # PM Blue Team 초안
│   │   ├── review-r1.md           # PM Red Team 라운드 1 리뷰
│   │   └── review-r2.md           # 라운드 2 리뷰 (Grade A 도달 시 종료)
│   └── v2/
│       ├── prd.md
│       └── review-r1~r3.md
├── phase2/                        # Design (Design Blue/Red Team)
│   ├── v1/
│   │   ├── design.md or design.pen
│   │   └── review-r1~r3.md
│   └── v2/
│       ├── design.md
│       └── review-r1~r3.md
└── handoff/                       # 핸드오프 산출물 (최종 PRD·Design·구현 계획)
    ├── v1/
    └── v2/
```

## 이 레포에 포함된 것 / 포함되지 않은 것

| 범주 | 포함 여부 | 비고 |
|---|---|---|
| `input/*/brief.md` | ✅ 실제 내용 | 제품 과제·참조·비즈니스 목표·제약 |
| `phase{1,2}/*/review-r*.md` | ✅ 실제 내용 | Issue Code·Grade·체크리스트가 라운드별로 수렴하는 실제 리포트 |
| `phase2/*/*.pen` | ✅ 디자인 원본 | Pencil MCP 로 열람 |
| `handoff/*/prd-final.md` | ⬛ 목차만 | 최종 PRD 는 비즈니스 결정이 집약되어 섹션 구성만 공개 |
| `handoff/*/*.pen` | ✅ 디자인 원본 | Pencil MCP 로 열람 |
| Phase 내부 Producer 초안 (`prd.md`·`design.md` 등) | ❌ 미포함 | 이터레이션 중간본은 제외 |
| Handoff 마크다운 정본 (design-final·implementation-plan·content-production-guide 등) | ❌ 미포함 | 이행 세부·경쟁 포지셔닝이 집약되어 제외 |

## Version 관리 관례

페이즈 내부에서 `v1`, `v2` 가 대규모 리비전을 나눕니다. 같은 `vN` 안에서 `review-r1 → r2 → r3` 로 진행되며 Grade A 도달 시 해당 버전이 종료되고 `handoff/vN/` 으로 최종본이 승격됩니다. v2 는 v1 대비 스코프 변경이나 근본 설계 수정이 있을 때 시작됩니다.
