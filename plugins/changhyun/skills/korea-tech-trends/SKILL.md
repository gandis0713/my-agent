---
name: korea-tech-trends
description: Fetch, analyze, and extract insights from the latest Korean emerging technology news articles.
model: haiku
---

# Korea Emerging Tech Trends

Search and analyze the latest Korean emerging technology news using WebFetch, then deliver trend analysis and actionable insights.

## News Sites

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| 전자신문 | etnews.com | https://www.etnews.com/news/section.html?id1=01 | Full URLs in article links |
| 블로터 | bloter.net | https://www.bloter.net/news/list.html?sec=technology | Full URLs in article links |
| IT조선 | it.chosun.com | https://it.chosun.com/category/industry/ | Full URLs in article links |
| ZDNet Korea | zdnet.co.kr | https://zdnet.co.kr/news/?lstcode=0000 | Relative URLs — prepend `https://zdnet.co.kr` |
| 아이뉴스24 | inews24.com | https://inews24.com/category/it-science | Full URLs in article links |

## Steps

### 1. Determine Search Scope

- If the user specifies a date, fetch all articles published on that date.
- If no date is specified, fetch all articles published today.
- If the user specifies a topic (e.g., AI, semiconductor, robotics), use it as a keyword filter when reviewing headlines.
- If the user specifies a site, fetch only that site.
- If no site preference, fetch from 2–3 sites by default (recommended: etnews.com + bloter.net + it.chosun.com).

### 2. Fetch Article List

Use WebFetch on the category URL(s) with this prompt:

> "[날짜]에 발행된 모든 기사 제목과 URL을 추출해주세요."

Fetch multiple sites in parallel when possible.

### 3. Filter by Date

From the returned headlines, select only articles published on the target date. Do not exclude articles based on count — include all matching articles.

### 4. Filter by Topic (if specified)

If the user specified a topic, further narrow the list to articles matching the keyword.

### 5. Fetch Article Content

For each selected article, use WebFetch with:

> "기사 제목, 날짜, 작성자, 본문 전체 내용을 추출해주세요."

Fetch articles in parallel.

### 6. Present Results

Format the output as follows:

```
## 🔬 국내 기술 트렌드 (YYYY-MM-DD)

### 1. [기사 제목]
- 출처: [사이트명] | 날짜: [날짜] | 기자: [기자명]
- 요약: [핵심 내용 2–3문장]
- 링크: [URL]

### 2. [기사 제목]
...

---

## 📊 트렌드 분석
[전체 기사에서 공통된 주제나 패턴을 도출합니다. 국내 산업·정책·투자 흐름에서 수렴하는 방향을 분석합니다.]

## 💡 핵심 인사이트
[Include all insights deemed significant — do not limit the count. Each insight should highlight a signal, its implications, or a reason to watch.]
1. [Insight — what to watch and why it matters]
2. [Insight]
...
```

## Notes

- Always fetch fresh content; do not rely on cached results for news.
- If a site returns an error, silently skip it and use the next available site.
- When article URLs are relative, prepend the site's base URL before fetching.
- Summarize article body and present all output in Korean.
- Trend analysis and insights should go beyond summary — identify signals, not just facts.
- Focus on domestic Korean tech industry signals: policy, investment, startup ecosystem, and R&D developments.
