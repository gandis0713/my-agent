---
name: us-company-analysis
description: Analyze a US-listed company (NYSE/NASDAQ) using Yahoo Finance for stock and financial data, SEC EDGAR for disclosures, and Reuters for latest news. For Korean listed companies, use the korea-company-analysis skill instead.
model: sonnet
---

# US Company Analysis (US Listed Companies Only)

Perform a comprehensive analysis of a US-listed company using Yahoo Finance, SEC EDGAR, and Reuters news.

> **Scope**: This skill is limited to companies listed on US exchanges (NYSE, NASDAQ). For Korean listed companies (KOSPI/KOSDAQ), use the `korea-company-analysis` skill. Non-US foreign companies are not supported.

## Steps

### 1. Identify the Company

Use WebFetch to find the ticker symbol on Yahoo Finance:

URL: `https://finance.yahoo.com/search?p={company name}`
Prompt: "Extract the company name, ticker symbol, and exchange (NYSE or NASDAQ) from the search results."

If multiple results appear, select the closest match and extract the ticker symbol (e.g., `AAPL` for Apple).

If the company is not listed on NYSE or NASDAQ, stop and inform the user that this skill only supports US-listed companies.

### 2. Fetch Company Data (Parallel)

Fetch the following three sources in parallel using WebFetch:

**2a. Company Overview & Stock Price**

URL: `https://finance.yahoo.com/quote/{ticker}`
Prompt: "Extract current price, previous close, change, change%, market cap, 52-week high/low, PE ratio, EPS, forward PE, dividend yield, sector, industry, and CEO."

**2b. Financial Summary**

URL: `https://finance.yahoo.com/quote/{ticker}/financials`
Prompt: "Extract the last 3 years of total revenue, operating income, net income, debt-to-equity ratio, and ROE."

**2c. SEC EDGAR Disclosures**

URL: `https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={ticker}&type=10-K&dateb=&owner=include&count=5`
Prompt: "Extract the 5 most recent filings: filing date, form type, and description."

If EDGAR is inaccessible, silently skip this section and note it in the output.

### 3. Fetch Related News

Use WebFetch on Reuters to retrieve the latest news:

URL: `https://www.reuters.com/search/news?blob={company name}`
Prompt: "Extract the 5 most recent article titles, dates, and URLs."

Then fetch each article in parallel:
Prompt: "Extract the article title, date, author, and a 2–3 sentence summary of the body."

### 4. Present Results

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

- Always fetch fresh data; do not rely on cached results.
- All financial figures should be presented in USD.
- If a data source returns an error, skip that section and note it in the output.
- Summarize news in Korean regardless of the original language.
