# Summary Plugin

AI 뉴스 일일 요약 레포트 자동 생성 Claude Code 플러그인.  
지난 24시간 AI 뉴스(해외 2건·국내 3건)를 검색하여 비전공자도 쉽게 읽을 수 있는  
유머·이모지 포함 한국어 레포트를 `reports/news_{YYYYMMDD}_{HHMM}.md`에 저장함.

## 기능

| 컴포넌트 | 이름 | 역할 |
|---------|------|------|
| 스킬 | `summary:summary` | AI 뉴스 요약 레포트 생성 오케스트레이션 |
| 에이전트 | `news-researcher` | 뉴스 검색·원문 저장·핵심 추출 |
| 에이전트 | `summary-writer` | 뉴스 1건 요약문 작성 (병렬 5회) |
| 에이전트 | `summary-reviewer` | 요약 검토·교정·저장·검증 |

## 설치

### 방법 1: 로컬 디렉토리로 테스트

```bash
claude --plugin-dir /path/to/summary-plugin
```

### 방법 2: 마켓플레이스 등록 후 설치

마켓플레이스를 GitHub 저장소에 호스팅한 경우:

```bash
# 마켓플레이스 등록
/plugin marketplace add <github-user>/<repo-name>

# 플러그인 설치 (user 스코프, 기본값)
/plugin install summary@summary-team

# 팀 공유 설치 (project 스코프)
/plugin install summary@summary-team --scope project
```

### 방법 3: skills-dir 플러그인 (즉시 로드)

```bash
# 플러그인 디렉토리를 ~/.claude/skills/ 하위에 복사 또는 심볼릭 링크
cp -r /path/to/summary-plugin ~/.claude/skills/summary
# 또는
ln -s /path/to/summary-plugin ~/.claude/skills/summary

# 다음 세션부터 summary@skills-dir 로 자동 로드
```

## 사용법

플러그인 설치 후 Claude Code 세션에서 슬래시 명령으로 호출:

```
/summary:summary
```

또는 자연어로 요청:

```
오늘 AI 뉴스 요약해줘
```

## 조회

```bash
# 설치된 플러그인 목록 확인
claude plugin list

# 플러그인 상세 정보 (컴포넌트·토큰 비용)
claude plugin details summary@summary-team
```

## 업그레이드

```bash
claude plugin update summary@summary-team
```

## 삭제

```bash
# 기본 삭제
claude plugin uninstall summary@summary-team

# 데이터 디렉토리 보존하며 삭제
claude plugin uninstall summary@summary-team --keep-data
```

## 디렉토리 구조

```
summary-plugin/
├── .claude-plugin/
│   ├── plugin.json        # 플러그인 매니페스트
│   └── marketplace.json   # 마켓플레이스 카탈로그
├── skills/
│   └── summary/
│       └── SKILL.md       # 요약 스킬 (오케스트레이션 로직)
├── agents/
│   ├── news-researcher.md # 뉴스 검색·추출 에이전트
│   ├── summary-writer.md  # 요약 작성 에이전트
│   └── summary-reviewer.md# 검토·저장·검증 에이전트
├── commands/
│   └── summary.md         # 슬래시 명령 진입점
└── README.md
```
