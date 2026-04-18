# Common Rules (공통 규칙)

모든 에이전트가 준수하는 공통 규칙을 정의한다.

---

## Design Tool: Pencil (pencil.dev)

모든 디자인 관련 산출물은 **Pencil** (.pen 파일)을 사용하여 작성하고 리뷰한다.

### 파일 저장 위치

#### 파이프라인 실행 시 (\_workspace/)

Orchestrator가 파이프라인을 실행할 때, 중간 산출물은 `_workspace/`에 저장한다.

| 산출물 유형   | 경로                                  | 비고                        |
| ------------- | ------------------------------------- | --------------------------- |
| Design Spec   | `_workspace/phase2/design.pen`        | Blue Team 최신본 (덮어쓰기) |
| Review Report | `_workspace/phase2/review-rN.md`      | 라운드별 별도 파일          |
| 최종 Design   | `_workspace/handoff/design-final.pen` | Grade A 통과본 복사         |

#### 독립 실행 시 (designs/)

Orchestrator 없이 단독 실행할 때, 산출물은 `designs/`에 저장한다.

- 프로젝트 루트 기준 경로: `designs/<기능명>.pen`
- 예시: `designs/onboarding-flow.pen`, `designs/payment-checkout.pen`
- 하위 분류가 필요한 경우: `designs/<도메인>/<기능명>.pen`
- 예시: `designs/settings/profile-edit.pen`, `designs/settings/notification-preferences.pen`

### 기본 원칙

- 디자인 산출물은 `.pen` 파일로 생성한다.
- `.pen` 파일은 반드시 **Pencil MCP 도구**를 통해서만 읽고 쓴다. (`Read`, `Grep` 등 일반 도구 사용 금지)
- 디자인 리뷰 시에도 Pencil MCP 도구를 통해 산출물을 확인한다.

### 사용 도구

| 도구                         | 용도                                          |
| ---------------------------- | --------------------------------------------- |
| `get_editor_state`           | 현재 열린 .pen 파일 상태 확인                 |
| `open_document`              | .pen 파일 열기 또는 새 파일 생성              |
| `get_guidelines`             | 가이드라인 및 스타일 로드                     |
| `batch_get`                  | 노드 검색 및 조회                             |
| `batch_design`               | 디자인 요소 삽입/복사/수정/삭제 (최대 25 ops) |
| `get_screenshot`             | 디자인 스크린샷 캡처                          |
| `export_nodes`               | 노드 내보내기                                 |
| `get_variables`              | 디자인 변수 조회                              |
| `set_variables`              | 디자인 변수 설정                              |
| `snapshot_layout`            | 레이아웃 스냅샷 캡처                          |
| `find_empty_space_on_canvas` | 캔버스 빈 공간 탐색                           |

### 워크플로우

```
[Blue Team] batch_design으로 디자인 생성/수정
     |
[Red Team] batch_get + get_screenshot으로 산출물 확인 및 리뷰
     |
[Blue Team] 피드백 반영하여 batch_design으로 수정
```