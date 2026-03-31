# notion-docs-writer

Notion 기반 문서 작성/업그레이드 파이프라인 마켓플레이스.

6단계 파이프라인(개요 → 자료수집 → 검증 → 문서작성 → 검토 → 업무계획)을 순차 실행하여 체계적인 문서를 Notion에 작성합니다.

## 설치

```bash
# 1. 마켓플레이스 추가
/plugin marketplace add Potwings/notion-docs-writer

# 2. 플러그인 설치
/plugin install notion-docs-writer@notion-docs-writer
```

## 요구사항

- Claude Code 1.0.33 이상
- Notion MCP 서버 연결 (Claude Code 빌트인)
- Git 설치 필요 (마켓플레이스 추가 시 내부적으로 `git clone` 사용)

```bash
# Debian/Ubuntu
apt install -y git

# RedHat/CentOS
yum install -y git

# macOS
brew install git
```

## 권장 권한 설정

파이프라인이 매번 확인 없이 원활하게 작동하려면 프로젝트의 `.claude/settings.json`에 `permissions.allow`를 추가하세요:

```json
{
  "enabledPlugins": {
    "notion-docs-writer@notion-docs-writer": true
  },
  "permissions": {
    "allow": [
      "WebSearch",
      "WebFetch",
      "mcp__claude_ai_Notion__*"
    ]
  }
}
```

또는 플러그인 사용 중 권한 요청 시 **Always allow**를 선택해도 됩니다.

## 플러그인 목록

### notion-docs-writer

Notion 기반 문서 작성/업그레이드 파이프라인 플러그인.

#### 스킬

| 스킬 | 설명 |
|------|------|
| `/notion-docs-writer:solve-problem` | 문제 해결 문서를 새로 작성 |
| `/notion-docs-writer:upgrade-docs` | 기존 Notion 문서를 분석·검증·업그레이드 |
| `/notion-docs-writer:resume` | 중단된 파이프라인을 특정 단계부터 재개 |

#### 파이프라인 구조

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
| 6 | `work-planner` | sonnet | 우선순위별 업무 계획 수립 + TODO 목록 삽입 |

#### 사용 예시

```bash
# 새 문서 작성
/notion-docs-writer:solve-problem 메일 분류 모델 정확도가 낮아서 개선이 필요합니다

# 기존 문서 업그레이드
/notion-docs-writer:upgrade-docs https://www.notion.so/your-page-id

# 중단된 파이프라인 재개 (시작 단계는 자동으로 물어봅니다)
/notion-docs-writer:resume https://www.notion.so/your-page-id

# 슬래시 명령어 없이도 가능
https://www.notion.so/your-page-id 이어서 진행해줘
```

## 디렉토리 구조

```
notion-docs-writer/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   └── notion-docs-writer/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       ├── agents/
│       └── skills/
└── README.md
```
