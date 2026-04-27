---
name: korea-company-analysis
description: Analyze a Korean listed company (KOSPI/KOSDAQ) using the company-fetch skill for data and the economic-news skill for latest news. For foreign companies, use the us-company-analysis skill instead.
model: sonnet
---

# Company Analysis (Korean Listed Companies Only)

Perform a comprehensive analysis of a Korean listed company (KOSPI/KOSDAQ) by combining data from the `korea-company-fetch` skill and news from the `economic-news` skill.

> **Scope**: This skill is limited to companies listed on KOSPI or KOSDAQ. For foreign companies, use the `us-company-analysis` skill.

## Steps

### 1. Fetch Company Data

Invoke the `company-fetch` skill with the company name.

If the skill returns an `[Error]` block, stop and relay the error message to the user.

### 2. Fetch Related News

Invoke the `economic-news` skill with the company name as a keyword. Do not fetch news independently — delegate entirely to that skill.

### 3. Present Results

Using the structured data returned by `company-fetch` and the news from `economic-news`, present the analysis in the following format:

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

- All financial figures should be presented in Korean units (억원, 조원).
- If a section is marked N/A in the fetch result, reflect that in the output.
- For the news section, delegate entirely to the `economic-news` skill with the company name as keyword.
