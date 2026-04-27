---
name: company-fetch
description: Fetches stock price, financial summary, and recent disclosures for a Korean (KOSPI/KOSDAQ) or US-listed (NYSE/NASDAQ) company. Auto-detects the market and returns structured text for use by other skills.
model: sonnet
---

# Company Data Fetch

Retrieve stock price, financial summary, and recent disclosures for a listed company. Automatically detects whether the company is listed on a Korean exchange (KOSPI/KOSDAQ) or a US exchange (NYSE/NASDAQ). This skill only collects data — analysis is handled by the calling skill.

## Input

- `company`: Company name or stock code / ticker symbol

## Steps

### 1. Identify the Company and Market

Search both exchanges in parallel using WebFetch:

**Naver Finance (Korean market)**
URL: `https://finance.naver.com/search/searchList.naver?query={company}`
Prompt: "Extract the company name, stock code, and market type (KOSPI/KOSDAQ) from the search results."

**Yahoo Finance (US market)**
URL: `https://finance.yahoo.com/search?p={company}`
Prompt: "Extract the company name, ticker symbol, and exchange (NYSE or NASDAQ) from the search results."

- If found on Naver Finance (KOSPI/KOSDAQ) → proceed with **Korean market flow**
- If found on Yahoo Finance (NYSE/NASDAQ) → proceed with **US market flow**
- If found on neither → return the following and stop:

```
[Error]
Company not found on KOSPI, KOSDAQ, NYSE, or NASDAQ.
```

### 2. Fetch Company Data (Parallel)

#### Korean Market Flow

Fetch the following three sources in parallel:

**2a. Company Overview & Stock Price**
URL: `https://finance.naver.com/item/main.naver?code={stock_code}`
Prompt: "Extract current price, change from previous close, change rate, market cap, shares outstanding, foreign ownership ratio, 52-week high/low, PER, PBR, dividend yield, sector, CEO, founded date, and listing date."

**2b. Financial Summary**
URL: `https://finance.naver.com/item/coinfo.naver?code={stock_code}`
Prompt: "Extract the last 3 years of revenue, operating income, net income, debt-to-equity ratio, and ROE."

**2c. DART Disclosures**
URL: `https://dart.fsc.go.kr/dsac001/search.do?textCrpNm={company}&maxCount=10`
Prompt: "Extract the 10 most recent disclosures: date, title, and disclosure type."
If inaccessible, mark as N/A.

#### US Market Flow

Fetch the following three sources in parallel:

**2a. Company Overview & Stock Price**
URL: `https://finance.yahoo.com/quote/{ticker}`
Prompt: "Extract current price, previous close, change, change rate, market cap, 52-week high/low, PE ratio, EPS, forward PE, dividend yield, sector, industry, and CEO."

**2b. Financial Summary**
URL: `https://finance.yahoo.com/quote/{ticker}/financials`
Prompt: "Extract the last 3 years of total revenue, operating income, net income, debt-to-equity ratio, and ROE."

**2c. SEC EDGAR Filings**
URL: `https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={ticker}&type=10-K&dateb=&owner=include&count=5`
Prompt: "Extract the 5 most recent filings: filing date, form type, and description."
If inaccessible, mark as N/A.

### 3. Return Structured Text

#### Korean Market

```
[Company Info]
Name: {name}
Market: KOSPI or KOSDAQ
Stock Code: {code}
Sector: {sector}
CEO: {ceo}
Founded: {founded}
Listed: {listed}

[Stock Price & Key Metrics]
Current Price: {price}
Change: {change} ({change_rate}%)
Market Cap: {market_cap}
Shares Outstanding: {shares}
Foreign Ownership: {foreign_ownership}%
52W High: {high} / 52W Low: {low}
PER: {per} / PBR: {pbr} / Dividend Yield: {dividend}%

[Financial Summary] (Unit: 억원)
(20XX) Revenue: {revenue} / Operating Income: {op_income} / Net Income: {net_income} / D/E: {de}% / ROE: {roe}%
(20XX) Revenue: {revenue} / Operating Income: {op_income} / Net Income: {net_income} / D/E: {de}% / ROE: {roe}%
(20XX) Revenue: {revenue} / Operating Income: {op_income} / Net Income: {net_income} / D/E: {de}% / ROE: {roe}%

[Recent Disclosures]
1. {date} {title} ({type})
2. {date} {title} ({type})
...
(N/A if inaccessible)
```

#### US Market

```
[Company Info]
Name: {name}
Market: NYSE or NASDAQ
Ticker: {ticker}
Sector: {sector}
Industry: {industry}
CEO: {ceo}

[Stock Price & Key Metrics]
Current Price: {price}
Change: {change} ({change_rate}%)
Previous Close: {prev_close}
Market Cap: {market_cap}
52W High: {high} / 52W Low: {low}
PE Ratio: {pe} / Forward PE: {fpe} / EPS: {eps} / Dividend Yield: {dividend}%

[Financial Summary] (Unit: USD)
(20XX) Revenue: {revenue} / Operating Income: {op_income} / Net Income: {net_income} / D/E: {de} / ROE: {roe}%
(20XX) Revenue: {revenue} / Operating Income: {op_income} / Net Income: {net_income} / D/E: {de} / ROE: {roe}%
(20XX) Revenue: {revenue} / Operating Income: {op_income} / Net Income: {net_income} / D/E: {de} / ROE: {roe}%

[Recent SEC Filings]
1. {date} {form_type} — {description}
2. {date} {form_type} — {description}
...
(N/A if inaccessible)
```

## Notes

- Always fetch fresh data; do not rely on cached results.
- If a data source returns an error, skip that section and mark it as N/A in the output.
- This skill only collects and returns data. Analysis is performed by the calling skill.
