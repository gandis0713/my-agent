---
name: industry-outlook
description: Collect, summarize, and extract insights from the latest economic and future industry outlook articles from major global think tanks, consulting firms, and research institutions.
model: haiku
---

# Industry Outlook Analysis

Fetch and analyze the latest economic and future industry outlook articles from top global think tanks, consulting firms, and research institutions. Deliver a structured analysis covering economic outlook, industry/business insights, and investment perspectives.

## Article Sources

### Global

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| Deloitte Insights | deloitte.com | https://www.deloitte.com/us/en/Industries/industry-outlooks.html | Full URLs in article links |
| Oxford Economics | oxfordeconomics.com | https://www.oxfordeconomics.com/ | Full URLs in article links |
| Harvard Business Review | hbr.org | https://hbr.org/topic/subject/economics | Full URLs in article links |
| Project Syndicate | project-syndicate.org | https://www.project-syndicate.org/section/economics | Relative URLs — prepend `https://www.project-syndicate.org` |
| MIT Sloan Management Review | sloanreview.mit.edu | https://sloanreview.mit.edu/topic/strategy/ | Full URLs in article links |
| Brookings Institution | brookings.edu | https://www.brookings.edu/topic/economic-studies/ | Full URLs in article links |

### Korea

| Site | Base URL | Category URL | Notes |
|------|----------|--------------|-------|
| KIET | kiet.re.kr | https://www.kiet.re.kr/trends/ecolookList | Relative URLs — prepend `https://www.kiet.re.kr` |
| HRI (현대경제연구원) | hri.co.kr | https://www.hri.co.kr/ | Article URLs follow the pattern `https://www.hri.co.kr/report/report-view.html?bmain=view&uid=<uid>` |

## Steps

### 1. Fetch Article List

Use WebFetch in parallel on the category URLs based on the user's request.
- If the user specifies a topic, search for articles related to that topic.
- If no topic is specified, fetch the latest articles.
- If domestic/global is not specified, use global sites by default.
- Collect up to 5 articles per site.

### 2. Filter Articles

- Remove duplicate articles.
- Prioritize articles that cover outlook, forecast, or future trends over simple current-event reports.

### 3. Fetch Article Content

Fetch the date, author, and body of each selected article in parallel.

### 4. Present Results

Format the output as follows:

```
## 🌐 Industry Outlook (YYYY-MM-DD)

### 1. [Article Title]
- Source: [Site Name] | Date: [Date] | Author: [Author]
- Summary: [2–3 sentence summary in Korean]
- Link: [URL]

### 2. [Article Title]
...

---

## 📊 Economic Outlook
[Summarize key economic trends and forecasts derived from the articles. What is the macro picture?]

## 🏭 Industry & Business Insights
1. [Insight — what to watch and why it matters for business/industry]
2. [Insight]
...

## 💰 Investment Insights
1. [Insight — what to watch and why it matters for investment]
2. [Insight]
...
```

## Notes

- Always fetch fresh content; do not rely on cached results.
- If a site returns an error, silently skip it and use the next available site.
- When article URLs are relative, prepend the site's base URL before fetching.
- Summarize all content and present output in Korean regardless of the original language.
- Insights should go beyond summary — identify signals, not just facts.
- Clearly distinguish between industry/business insights and investment insights.
