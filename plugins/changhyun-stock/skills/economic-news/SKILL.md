---
name: economic-news
description: Fetch, summarize, and analyze the latest economic news articles from major domestic and global financial news sites.
model: haiku
---

# Economic News Search & Analysis

Search and analyze the latest economic news articles using WebFetch.

## News Sites

### Domestic (Korea)

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| 한국경제 | hankyung.com | https://www.hankyung.com/economy | Full URLs in article links |
| 서울경제 | sedaily.com | https://www.sedaily.com/NewsList/GD | Full URLs in article links |
| 이데일리 | edaily.co.kr | https://www.edaily.co.kr/economy/ | Relative URLs — prepend `https://www.edaily.co.kr` |
| 헤럴드경제 | biz.heraldcorp.com | https://biz.heraldcorp.com/economy | Full URLs in article links |
| 뉴시스 | newsis.com | https://www.newsis.com/economy/ | Relative URLs — prepend `https://www.newsis.com` |

### Global

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| Yahoo Finance | finance.yahoo.com | https://finance.yahoo.com/topic/economic-news/ | Full URLs in article links |
| Investing.com | investing.com | https://www.investing.com/news/economy | Full URLs in article links |
| Fortune | fortune.com | https://fortune.com/section/economy/ | Full URLs in article links |
| Project Syndicate | project-syndicate.org | https://www.project-syndicate.org/ | Relative URLs — prepend `https://www.project-syndicate.org` |
| SCMP | scmp.com | https://www.scmp.com/economy | Relative URLs — prepend `https://www.scmp.com` |

## Steps

### 1. Fetch Article List

Use WebFetch in parallel on the category URLs of domestic or global sites based on the user's request.
- If the user specifies a topic, search for articles related to that topic.
- If no topic is specified, fetch the latest news articles for today's date.
- If domestic/global is not specified, use domestic sites by default.

### 2. Filter Articles

Remove duplicate articles from the collected headlines.

### 3. Fetch Article Content

Fetch the date, author, and body of each selected article in parallel.

### 4. Present Results

Format the output as follows:

```
## 📰 오늘의 경제 뉴스 (YYYY-MM-DD)

### 1. [기사 제목]
- 출처: [사이트명] | 날짜: [날짜] | 기자: [기자명]
- 요약: [핵심 내용 2–3문장]
- 링크: [URL]

### 2. [기사 제목]
...

---
💡 핵심 동향: [전체 기사 기반 경제 동향 1–2문장 요약]
```

## Notes

- Always fetch fresh content; do not rely on cached results for news.
- If a site returns an error, silently skip it and use the next available site.
- When article URLs are relative, prepend the site's base URL before fetching.
- Summarize article body in Korean regardless of the original language.
