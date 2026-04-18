# co-agents

> 다중 에이전트 기반 제품 개발 파이프라인입니다.

분야별 공개 전문 자료에서 각 직무가 가진 관점·판단 기준·체크리스트를 추려 역할별 에이전트 정의(`agent.md`)로 옮겨 적고, 그 위에 Orchestrator 를 올려 **Producer → Reviewer** 루프를 돌리는 구조입니다.

## 무엇인가요

제품을 만드는데까지 PM·디자이너·리서처·브랜딩·콘텐츠 전략·데이터 분석·개발의 시각이 모두 필요한 상황에서, 각 역할을 개별 프롬프트 파일로 분리·명문화하고 Orchestrator 가 이를 순차 또는 병렬로 호출하도록 만든 장치입니다. Producer 역할이 초안을 만들고 Reviewer 역할이 같은 분야의 기준으로 검토하는 **Blue/Red 루프**가 Grade A 판정 전까지 최대 3 라운드 반복됩니다.

- **Producer (Blue Team)**: 해당 역할 관점에서 산출물을 작성합니다.
- **Reviewer (Red Team)**: 같은 분야의 평가 루브릭·이슈 분류로 초안을 평가·피드백합니다.
- **Orchestrator**: 페이즈 전이와 품질 게이트(Grade A 도달 시 다음 페이즈)를 관리합니다.

## 구조

```
agents/
├── agent-orchestrator.md              # 전체 파이프라인 조정자
├── agent-project-manager.md           # PM (PRD, Decision Log, Validation Plan)
├── agent-prd-reviewer.md              # PRD Red Team
├── agent-product-designer.md          # 프로덕트 디자이너
├── agent-design-reviewer.md           # 디자인 Red Team
├── agent-brand-design.md              # 브랜드·마케팅 크리에이티브
├── agent-branding.md                  # 브랜드 전략
├── agent-market-researcher.md         # 시장 리서치
├── agent-content-strategy.md          # 콘텐츠 전략
├── agent-data-analyst.md              # 그로스·애널리틱스
├── prd-common-rules.md                # PRD 공통 규칙
├── design-common-rules.md             # 디자인 공통 규칙
└── development/
    ├── agent-dev-orchestrator.md      # 개발 파이프라인 조정자
    ├── agent-5-frontend.md
    ├── agent-6-backend.md
    ├── agent-8-infrastructure.md
    ├── agent-9-security.md
    ├── agent-10-mobile.md
    └── agent-11-code-review.md

.claude/
└── skills/
    └── analyze-project/
        └── SKILL.md                   # 프로젝트 전체 맥락 파악 스킬

_workspace/
├── README.md
├── input/{v1,v2}/brief.md                  # 제품 과제 입력 Brief (실제 내용)
├── phase1/{v1,v2}/review-r*.md             # PRD Reviewer 라운드별 리뷰 (실제 내용)
├── phase2/{v1,v2}/review-r*.md + *.pen     # Design 리뷰 + 원본 .pen (실제 내용)
└── handoff/{v1,v2}/prd-final.md + *.pen    # 정본 PRD 는 목차만 공개 · 디자인 원본 .pen 은 포함
```

Orchestrator 가 묶는 페이즈는 세 단계로 구성됩니다.

```
Phase 1: PRD          Phase 2: Design        Phase 3: Marketing (독립)
PM Blue → PM Red  →   Design Blue → Red  →   Research / Branding / Content Strategy
  (max 3 rounds,         (max 3 rounds,             ↕ Cross-review
   Grade A gate)          Grade A gate)
                                   ↓
                           [개발 핸드오프]
```

## 사용

[Claude Code](https://claude.ai/code) 기준 예시입니다.

```bash
# PRD 초안 작성
claude --prompt "agent-project-manager.md 역할로 다음 문제에 대한 PRD 초안을 작성해줘: [문제 설명]"

# Red Team 리뷰
claude --prompt "agent-prd-reviewer.md 역할로 방금 만든 PRD 초안을 리뷰하고 이슈 코드를 매겨줘"

# Orchestrator 로 전체 페이즈 구동
claude --prompt "agent-orchestrator.md 역할로 [과제명] 과제를 Phase 1 부터 구동해줘"

# 프로젝트 맥락 파악 스킬 호출
claude --prompt "/analyze-project"
```

각 에이전트 파일은 `Identity → Principles → Agent Behavior → Output Specifications → Quality Rubric → Issue Taxonomy → Collaboration Protocol → Usage` 구조를 공유합니다. 필요에 맞게 각 섹션을 조정하여 자신의 프로젝트 맥락으로 치환해 사용하시면 됩니다.

`.claude/skills/analyze-project` 는 이 레포를 Claude Code 로 클론해 사용할 때 자동 로드되는 보조 스킬입니다. 리서치·기획·디자인·콘텐츠·데이터 파이프라인·구현 6개 카테고리로 프로젝트 구조를 스캔해 맥락을 재구성해 줍니다 — 신규 합류·세션 재개·타 프로젝트에서의 맥락 전환 시점에 유용합니다.

`_workspace/` 아래에는 실제 프로젝트에서 이 파이프라인을 돌려 생성된 **Reviewer 단계 산출물**을 페이즈·버전별로 담아두었습니다. Issue Code·등급·체크리스트가 라운드를 거치며 어떻게 수렴하는지 확인하실 수 있습니다. 자세한 구조 설명은 [`_workspace/README.md`](./_workspace/README.md) 를 참고해 주세요.

## 이 레포를 읽는 방법

이 레포는 **재사용 가능한 템플릿**과 **실제 운영된 문서**의 중간에 있습니다. 저자가 어느 뉴스 제품을 만들며 사용한 원본 파일에서 해당 제품명과 제품 배경 섹션만 제거했고, 각 에이전트가 가진 도메인 문맥(뉴스 산업·편향 시각화·한국 시장 통계 등)은 그대로 두었습니다. 이는 **"한 에이전트가 어떻게 특정 도메인에 맞게 커스터마이징되었는가"** 를 그대로 보여주기 위한 것이며, 그 맥락은 실제 사용 시 각자의 도메인으로 치환될 수 있습니다.
