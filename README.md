# notion-docs-writer

Notion 기반 문서 작성/업그레이드 파이프라인 플러그인.

6단계 파이프라인(개요 → 자료수집 → 검증 → 문서작성 → 검토 → 업무계획)을 순차 실행하여 체계적인 문서를 Notion에 작성합니다.

## 스킬

| 스킬 | 설명 |
|------|------|
| `/solve-problem` | 문제 해결 문서를 새로 작성 |
| `/upgrade-docs` | 기존 Notion 문서를 분석·검증·업그레이드 |
| `/resume` | 중단된 파이프라인을 특정 단계부터 재개 |

## 파이프라인 구조

```
Step 1: 개요 작성 ──→ Step 2: 자료 수집 ──→ Step 3: 자료 검증
                                                      │
Step 6: 업무 계획 ←── Step 5: 문서 검토 ←── Step 4: 문서 작성
```

| Step | 에이전트 | 모델 | 역할 |
|------|---------|------|------|
| 1 | `solution-draft-writer` / `draft-rewriter` | sonnet | 개요 작성 (스킬별 전용) |
| 2 | `draft-researcher` | sonnet | [조사 필요] 태그 기반 자료 수집 |
| 3 | `research-verifier` | sonnet | 수집 자료 독립 검증 |
| 4 | `document-composer` | opus | 개요 + 검증 자료 → 완성 문서 |
| 5 | `document-reviewer` | opus | 5가지 관점 검토 → 최종본 |
| 6 | `work-planner` | sonnet | 우선순위별 업무 계획 수립 |

## 사용 예시

```bash
# 새 문서 작성
/solve-problem 메일 분류 모델 정확도가 낮아서 개선이 필요합니다

# 기존 문서 업그레이드
/upgrade-docs https://www.notion.so/your-page-id

# 3단계부터 재개
/resume https://www.notion.so/your-page-id 3
```

## 설치

```bash
/plugin marketplace add Potwings/notion-docs-writer
```

## 요구사항

- Claude Code 1.0.33 이상
- Notion MCP 서버 연결 (아래 설정 참고)

## Notion 연동

이 플러그인은 Notion 연동이 필요합니다. Claude Code에서 Notion 공식 플러그인을 설치하세요:

```bash
/plugin install notion@claude-plugins-official
```
