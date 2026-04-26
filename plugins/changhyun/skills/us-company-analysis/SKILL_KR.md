---
name: us-company-analysis
description: Yahoo Finance로 주가 및 재무 데이터, SEC EDGAR로 공시, Reuters로 최신 뉴스를 수집해 미국 상장 기업(NYSE/NASDAQ)을 종합 분석합니다. 한국 상장 기업은 korea-company-analysis 스킬을 사용하세요.
model: sonnet
---

# 미국 기업 분석 (미국 상장 기업 전용)

Yahoo Finance, SEC EDGAR, Reuters 뉴스를 활용해 미국 상장 기업을 종합 분석합니다.

> **적용 범위**: 이 스킬은 미국 거래소(NYSE, NASDAQ)에 상장된 기업에 한정됩니다. 한국 상장 기업(KOSPI/KOSDAQ)은 `korea-company-analysis` 스킬을 사용하세요. 미국 외 외국 기업은 지원하지 않습니다.

## 실행 단계

### 1. 기업 식별

WebFetch로 Yahoo Finance에서 티커 심볼을 검색합니다:

URL: `https://finance.yahoo.com/search?p={기업명}`
프롬프트: "검색 결과에서 회사명, 티커 심볼, 거래소(NYSE 또는 NASDAQ)를 추출해주세요."

검색 결과가 여러 개인 경우, 가장 근접한 결과를 선택하고 티커 심볼을 추출합니다 (예: Apple → `AAPL`).

NYSE 또는 NASDAQ에 상장되지 않은 기업인 경우, 중단하고 사용자에게 이 스킬은 미국 상장 기업만 지원함을 안내합니다.

### 2. 기업 데이터 수집 (병렬)

아래 세 가지 소스를 WebFetch로 병렬 수집합니다:

**2a. 기업 개요 & 주가 정보**

URL: `https://finance.yahoo.com/quote/{ticker}`
프롬프트: "현재가, 전일 종가, 등락액, 등락률, 시가총액, 52주 최고/최저, PER, EPS, 선행 PER, 배당수익률, 섹터, 업종, CEO를 추출해주세요."

**2b. 재무 요약**

URL: `https://finance.yahoo.com/quote/{ticker}/financials`
프롬프트: "최근 3개년 총매출, 영업이익, 순이익, 부채비율, ROE를 추출해주세요."

**2c. SEC EDGAR 공시 목록**

URL: `https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={ticker}&type=10-K&dateb=&owner=include&count=5`
프롬프트: "최근 5건의 공시 날짜, 양식 유형, 설명을 추출해주세요."

EDGAR 접근이 불가능한 경우, 해당 섹션을 조용히 건너뛰고 결과에 명시합니다.

### 3. 관련 뉴스 수집

WebFetch로 Reuters에서 최신 뉴스를 수집합니다:

URL: `https://www.reuters.com/search/news?blob={기업명}`
프롬프트: "최신 기사 5건의 제목, 날짜, URL을 추출해주세요."

이후 각 기사를 병렬로 fetch합니다:
프롬프트: "기사 제목, 날짜, 기자, 본문의 2–3문장 요약을 추출해주세요."

### 4. 결과 출력

```
## 🏢 [기업명] 기업 분석 (YYYY-MM-DD)

### 📊 기업 개요
- 티커: [심볼] | 거래소: [NYSE/NASDAQ] | 섹터: [섹터] | 업종: [업종]
- CEO: [이름]

### 💰 주가 & 투자 지표
- 현재가: [가격] | 전일 종가: [가격] | 등락: [±금액] ([%])
- 시가총액: [금액]
- 52주 최고: [가격] | 52주 최저: [가격]
- PER: [배] | 선행 PER: [배] | EPS: [금액] | 배당수익률: [%]

### 📈 재무 요약 (최근 3개년)
| 연도 | 매출액 | 영업이익 | 순이익 | 부채비율 | ROE |
|------|--------|---------|--------|---------|-----|
| 20XX |        |         |        |         |     |
| 20XX |        |         |        |         |     |
| 20XX |        |         |        |         |     |

### 📋 최근 SEC 공시
1. [날짜] [양식 유형] — [설명]
...

### 📰 최신 관련 뉴스
1. [기사 제목] — Reuters | [날짜]
   요약: [2–3문장]
...

---
💡 종합 의견: [전체 데이터를 바탕으로 한 기업 현황 및 전망 2–3문장 종합 분석]
```

## 주의사항

- 항상 최신 데이터를 fetch하며, 캐시된 결과에 의존하지 않습니다.
- 재무 수치는 USD로 표기합니다.
- 특정 소스에서 오류가 발생하면 해당 섹션을 건너뛰고 결과에 명시합니다.
- 뉴스 요약은 원문 언어와 무관하게 항상 한국어로 제공합니다.
