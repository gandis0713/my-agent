# Skills

This directory contains Claude Code skills for the `changhyun-stock` plugin.

---

## economic-news

Fetch, summarize, and analyze the latest Korean economic news using WebFetch.

### Supported Sites

The following sites have been verified to work with WebFetch, including full article body access:

| Site | Domain | Body Quality |
|------|--------|-------------|
| 한국경제 (Korea Economic Daily) | hankyung.com | Key content + structured figures |
| 서울경제 (Seoul Economic Daily) | sedaily.com | Section-by-section breakdown, rich quotes |
| 헤럴드경제 (Herald Economy) | biz.heraldcorp.com | Highest fidelity to original text |
| 이데일리 (eDaily) | edaily.co.kr | Detailed content with figures |
| 뉴시스 (Newsis) | newsis.com | Key content with quotes |

### Excluded Sites

| Site | Reason |
|------|--------|
| mk.co.kr | Blocked by WebFetch |
| yna.co.kr | Blocked by WebFetch |
| biz.chosun.com | Blocked by WebFetch |
| fnnews.com | 404 error |
| news1.kr | Body content gets compressed/summarized — quality insufficient |

### Usage

Invoke this skill when a user asks for:
- Today's economic news
- News on a specific topic (e.g., semiconductors, exchange rate, real estate)
- News from a specific source
- Economic trend briefings

---

## git-commit

Analyze staged git changes and generate a commit message using Claude Haiku, then commit.

### Behavior

- Reads staged diff via `git diff --cached`
- Generates a concise commit message using Claude Haiku (`claude-haiku-4-5-20251001`)
- Confirms the message with the user before committing
- Handles single-line and multi-line commit messages

### Usage

Invoke this skill when a user asks to:
- Commit staged changes
- Auto-generate a commit message from staged files
- Create a git commit
