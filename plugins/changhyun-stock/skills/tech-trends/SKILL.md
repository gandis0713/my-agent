---
name: tech-trends
description: Fetch, analyze, and extract insights from the latest technology trend articles from major domestic and global tech news sites.
model: haiku
---

# Tech Trends Search & Analysis

Search and analyze the latest technology news from domestic and global sources using WebFetch, then deliver trend analysis and actionable insights.

## News Sites

### Domestic (Korea)

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| 전자신문 | etnews.com | https://www.etnews.com/news/section.html?id1=01 | Full URLs in article links |
| 블로터 | bloter.net | https://www.bloter.net/news/list.html?sec=technology | Full URLs in article links |
| IT조선 | it.chosun.com | https://it.chosun.com/category/industry/ | Full URLs in article links |
| ZDNet Korea | zdnet.co.kr | https://zdnet.co.kr/news/?lstcode=0000 | Relative URLs — prepend `https://zdnet.co.kr` |
| 아이뉴스24 | inews24.com | https://inews24.com/category/it-science | Full URLs in article links |

### Global

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| MIT Technology Review | technologyreview.com | https://www.technologyreview.com/latest/ | Full URLs in article links |
| Wired | wired.com | https://www.wired.com/category/science/ | Full URLs in article links |
| Futurism | futurism.com | https://futurism.com/the-byte | Full URLs in article links |
| VentureBeat | venturebeat.com | https://venturebeat.com/category/ai/ | Full URLs in article links |
| TechCrunch | techcrunch.com | https://techcrunch.com/latest/ | Full URLs in article links |

## Steps

### 1. Fetch Article List

Use WebFetch in parallel on the category URLs of domestic or global sites based on the user's request.
- If the user specifies a topic, search for articles related to that topic.
- If a date is specified, fetch articles from that date; otherwise fetch today's articles.
- If domestic/global is not specified, use domestic sites by default.

### 2. Filter by Date

From the returned headlines, select only articles published on the target date. Do not exclude articles based on count — include all matching articles.

### 3. Filter by Topic (if specified)

If the user specified a topic, further narrow the list to articles matching the keyword.

### 4. Remove Duplicates

Remove duplicate articles collected across multiple sites.

### 5. Fetch Article Content

Fetch the date, author, and body of each selected article in parallel.

### 6. Present Results

Format the output as follows:

```
## 📡 Tech Trends (YYYY-MM-DD)

### 1. [Article Title]
- Source: [Site Name] | Date: [Date] | Author: [Author]
- Summary: [2–3 sentence summary in Korean]
- Link: [URL]

### 2. [Article Title]
...

---

## 📊 Trend Analysis
[Identify common themes or patterns across all articles. What technologies or sectors are converging?]

## 💡 Key Insights
[Include all insights deemed significant — do not limit the count. Each insight should highlight a signal, its implications, or a reason to watch.]
1. [Insight — what to watch and why it matters]
2. [Insight]
...
```

## Notes

- Always fetch fresh content; do not rely on cached results for news.
- If a site returns an error, silently skip it and use the next available site.
- When article URLs are relative, prepend the site's base URL before fetching.
- Summarize article body and present all output in Korean regardless of the original language.
- Trend analysis and insights should go beyond summary — identify signals, not just facts.
