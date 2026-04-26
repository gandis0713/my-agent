---
name: korea-company-analysis
description: Analyze a Korean listed company (KOSPI/KOSDAQ) using Naver Finance for stock and financial data, DART for recent disclosures, and the economic-news skill for latest news. For foreign companies, use the us-company-analysis skill instead.
model: sonnet
---

# Company Analysis (Korean Listed Companies Only)

Perform a comprehensive analysis of a Korean listed company (KOSPI/KOSDAQ) using Naver Finance, DART disclosures, and the economic-news skill.

> **Scope**: This skill is limited to companies listed on KOSPI or KOSDAQ. For foreign companies, use the `us-company-analysis` skill.

## Steps

### 1. Identify the Company

Use WebFetch to search for the company on Naver Finance:

URL: `https://finance.naver.com/search/searchList.naver?query={기업명}`
Prompt: "검색 결과에서 회사명, 종목코드, 시장 구분(KOSPI/KOSDAQ)을 추출해주세요."

If multiple results appear, select the closest match and extract the stock code (e.g., `005930` for Samsung Electronics).

If the company is not found on KOSPI/KOSDAQ, stop and inform the user to use the `us-company-analysis` skill instead.

### 2. Fetch Company Data (Parallel)

Fetch the following three sources in parallel using WebFetch:

**2a. Company Overview & Stock Price**

URL: `https://finance.naver.com/item/main.naver?code={종목코드}`

Prompt: "현재가, 전일 대비, 등락률, 시가총액, 상장 주식수, 외국인 소진율, 52주 최고/최저, PER, PBR, 배당수익률, 업종, 대표이사, 설립일, 상장일을 추출해주세요."

**2b. Financial Summary**

URL: `https://finance.naver.com/item/coinfo.naver?code={종목코드}`

Prompt: "최근 3개년 매출액, 영업이익, 당기순이익, 부채비율, ROE를 추출해주세요."

**2c. DART Disclosures**

URL: `https://dart.fsc.go.kr/dsac001/search.do?textCrpNm={기업명}&maxCount=10`

Prompt: "최근 공시 10건의 공시 날짜, 공시 제목, 공시 유형을 추출해주세요."

If DART is inaccessible (JavaScript-heavy or blocked), silently skip this section and note it in the output.

### 3. Fetch Related News

Invoke the `economic-news` skill with the company name as a keyword. Do not fetch news independently — delegate entirely to that skill.

### 4. Present Results

```
## 🏢 [기업명] 기업 분석 (YYYY-MM-DD)

### 📊 기업 개요
- 종목코드: [코드] | 시장: [KOSPI/KOSDAQ] | 업종: [업종명]
- 대표이사: [이름] | 설립일: [날짜] | 상장일: [날짜]

### 💰 주가 & 투자 지표
- 현재가: [가격] | 전일 대비: [±금액] ([등락률]%)
- 시가총액: [금액] | 상장주식수: [수량] | 외국인소진율: [%]
- 52주 최고: [가격] | 52주 최저: [가격]
- PER: [배] | PBR: [배] | 배당수익률: [%]

### 📈 재무 요약 (최근 3개년)
| 연도 | 매출액 | 영업이익 | 순이익 | 부채비율 | ROE |
|------|--------|---------|--------|---------|-----|
| 20XX |        |         |        |         |     |
| 20XX |        |         |        |         |     |
| 20XX |        |         |        |         |     |

### 📋 최근 주요 공시
1. [날짜] [공시 제목] ([공시 유형])
...

### 📰 최신 관련 뉴스
1. [기사 제목] — 출처 | 날짜
   요약: [2–3문장]
...

---
💡 종합 의견: [전체 데이터를 바탕으로 한 기업 현황 2–3문장 종합 분석]
```

## Notes

- Always fetch fresh data; do not rely on cached results.
- All financial figures should be presented in Korean units (억원, 조원).
- If a data source returns an error, skip that section and note it in the output.
- For the news section, delegate entirely to the `economic-news` skill with the company name as keyword.
