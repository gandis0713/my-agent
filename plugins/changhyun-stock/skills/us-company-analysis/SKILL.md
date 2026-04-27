---
name: us-company-analysis
description: Analyze a US-listed company (NYSE/NASDAQ) using the company-fetch skill for data and Reuters for latest news. For Korean listed companies, use the korea-company-analysis skill instead.
model: sonnet
---

# US Company Analysis (US Listed Companies Only)

Perform a comprehensive analysis of a US-listed company (NYSE/NASDAQ) by combining data from the `us-company-fetch` skill and news from Reuters.

> **Scope**: This skill is limited to companies listed on US exchanges (NYSE, NASDAQ). For Korean listed companies (KOSPI/KOSDAQ), use the `korea-company-analysis` skill. Non-US foreign companies are not supported.

## Steps

### 1. Fetch Company Data

Invoke the `company-fetch` skill with the company name.

If the skill returns an `[Error]` block, stop and relay the error message to the user.

### 2. Fetch Related News

Use WebFetch on Reuters to retrieve the latest news:

URL: `https://www.reuters.com/search/news?blob={company name}`
Prompt: "Extract the 5 most recent article titles, dates, and URLs."

Then fetch each article in parallel:
Prompt: "Extract the article title, date, author, and a 2–3 sentence summary of the body."

### 3. Present Results

Using the structured data returned by `company-fetch` and the news from Reuters, present the analysis in the following format:

```
## 🏢 [Company Name] Analysis (YYYY-MM-DD)

### 📊 Company Overview
- Ticker: [symbol] | Exchange: [NYSE/NASDAQ] | Sector: [sector] | Industry: [industry]
- CEO: [name]

### 💰 Stock Price & Key Metrics
- Current Price: [price] | Prev Close: [price] | Change: [±amount] ([%])
- Market Cap: [amount]
- 52W High: [price] | 52W Low: [price]
- PE Ratio: [x] | Forward PE: [x] | EPS: [amount] | Dividend Yield: [%]

### 📈 Financial Summary (Last 3 Years)
| Year | Revenue | Operating Income | Net Income | D/E Ratio | ROE |
|------|---------|-----------------|------------|-----------|-----|
| 20XX |         |                 |            |           |     |
| 20XX |         |                 |            |           |     |
| 20XX |         |                 |            |           |     |

### 📋 Recent SEC Filings
1. [Date] [Form Type] — [Description]
...

### 📰 Latest News
1. [Article Title] — Reuters | [Date]
   Summary: [2–3 sentences]
...

---
💡 Overall Assessment: [2–3 sentence summary of the company's current position and outlook based on all data]
```

## Notes

- All financial figures should be presented in USD.
- If a section is marked N/A in the fetch result, reflect that in the output.
- Summarize news in Korean regardless of the original language.
