---
name: global-tech-trends
description: Fetch, analyze, and extract insights from the latest global emerging technology news articles.
model: haiku
---

# Global Emerging Tech Trends

Search and analyze the latest global emerging technology news using WebFetch, then deliver trend analysis and actionable insights.

## News Sites

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| MIT Technology Review | technologyreview.com | https://www.technologyreview.com/latest/ | Full URLs in article links |
| Wired | wired.com | https://www.wired.com/category/science/ | Full URLs in article links |
| Futurism | futurism.com | https://futurism.com/the-byte | Full URLs in article links |
| VentureBeat | venturebeat.com | https://venturebeat.com/category/ai/ | Full URLs in article links |
| TechCrunch | techcrunch.com | https://techcrunch.com/latest/ | Full URLs in article links |

## Steps

### 1. Determine Search Scope

- If the user specifies a date, fetch all articles published on that date.
- If no date is specified, fetch all articles published today.
- If the user specifies a topic (e.g., AI, biotech, climate tech), use it as a keyword filter when reviewing headlines.
- If the user specifies a site, fetch only that site.
- If no site preference, fetch from 2–3 sites by default (recommended: technologyreview.com + venturebeat.com + futurism.com).

### 2. Fetch Article List

Use WebFetch on the category URL(s) with this prompt:

> "Extract all article titles and their URLs published on [date]."

Fetch multiple sites in parallel when possible.

### 3. Filter by Date

From the returned headlines, select only articles published on the target date. Do not exclude articles based on count — include all matching articles.

### 4. Filter by Topic (if specified)

If the user specified a topic, further narrow the list to articles matching the keyword.

### 5. Fetch Article Content

For each selected article, use WebFetch with:

> "Extract the article title, date, author, and full body content."

Fetch articles in parallel.

### 6. Present Results

Format the output as follows:

```
## 🔭 Global Tech Trends (YYYY-MM-DD)

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
