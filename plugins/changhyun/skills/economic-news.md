---
name: economic-news
description: Fetch, summarize, and analyze the latest Korean economic news articles from major financial news sites using WebFetch.
---

# Economic News Search & Analysis

Search and analyze the latest Korean economic news articles using WebFetch.

## Trigger

Use this skill when the user asks for:
- Latest economic news (e.g., "오늘 경제 뉴스", "최신 경제 기사")
- News on a specific economic topic (e.g., "반도체 관련 뉴스", "환율 동향")
- News from a specific source (e.g., "한경 오늘 뉴스", "서울경제 기사")
- Economic trend analysis or briefings

## Supported Sites

The following sites are confirmed to work with WebFetch:

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| 한국경제 | hankyung.com | https://www.hankyung.com/economy | Full URLs in article links |
| 서울경제 | sedaily.com | https://www.sedaily.com/NewsList/GD | Full URLs in article links |
| 이데일리 | edaily.co.kr | https://www.edaily.co.kr/economy/ | Relative URLs — prepend `https://www.edaily.co.kr` |
| 헤럴드경제 | biz.heraldcorp.com | https://biz.heraldcorp.com/economy | Full URLs in article links |
| 뉴시스 | newsis.com | https://www.newsis.com/economy/ | Relative URLs — prepend `https://www.newsis.com` |
**Blocked or unavailable sites (do not use):**
- mk.co.kr — blocked
- yna.co.kr — blocked
- biz.chosun.com — blocked
- fnnews.com — 404
- news1.kr — body content gets compressed/summarized by WebFetch; excluded for quality

## Steps

### 1. Determine Search Scope

- If the user specifies a topic, use it as a keyword filter when reviewing headlines.
- If the user specifies a site, fetch only that site.
- If no preference, fetch from 2–3 sites by default (recommended: hankyung.com + sedaily.com + biz.heraldcorp.com).

### 2. Fetch Article List

Use WebFetch on the category URL(s) with this prompt:

> "최신 경제 뉴스 기사 제목 10개와 각 기사의 URL을 추출해주세요."

Fetch multiple sites in parallel when possible.

### 3. Filter by Topic (if specified)

From the returned headlines, select the 3–5 most relevant articles matching the user's keyword or topic.

### 4. Fetch Article Content

For each selected article, use WebFetch with:

> "기사 제목, 날짜, 작성자, 본문 전체 내용을 추출해주세요."

Fetch articles in parallel.

### 5. Present Results

Format the output as follows:

```
## 📰 오늘의 경제 뉴스 (YYYY-MM-DD)

### 1. [기사 제목]
- 출처: [사이트명] | 날짜: [날짜] | 기자: [기자명]
- 요약: [2–3문장 핵심 요약]
- 링크: [URL]

### 2. [기사 제목]
...

---
💡 핵심 동향: [전체 기사를 바탕으로 한 1–2문장 경제 동향 요약]
```

## Notes

- Always fetch fresh content; do not rely on cached results for news.
- If a site returns an error, silently skip it and use the next available site.
- When article URLs are relative, prepend the site's base URL before fetching.
- Summarize article body in Korean regardless of the original language.
