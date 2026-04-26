---
name: tech-trends
description: 국내외 주요 기술 뉴스 사이트에서 최신 기술 트렌드 기사를 수집하고 트렌드를 분석하며 인사이트를 도출합니다.
model: haiku
---

# 기술 트렌드 검색 및 분석

WebFetch를 활용해 국내외 주요 기술 언론사의 최신 뉴스를 검색·분석하고, 트렌드 분석과 실질적인 인사이트를 제공합니다.

## 뉴스 사이트

### 국내

| 사이트 | 베이스 URL | 카테고리 URL | 비고 |
|--------|-----------|------------|------|
| 전자신문 | etnews.com | https://www.etnews.com/news/section.html?id1=01 | 기사 링크 전체 URL 포함 |
| 블로터 | bloter.net | https://www.bloter.net/news/list.html?sec=technology | 기사 링크 전체 URL 포함 |
| IT조선 | it.chosun.com | https://it.chosun.com/category/industry/ | 기사 링크 전체 URL 포함 |
| ZDNet Korea | zdnet.co.kr | https://zdnet.co.kr/news/?lstcode=0000 | 상대 URL — `https://zdnet.co.kr` 앞에 붙일 것 |
| 아이뉴스24 | inews24.com | https://inews24.com/category/it-science | 기사 링크 전체 URL 포함 |

### 글로벌

| 사이트 | 베이스 URL | 카테고리 URL | 비고 |
|--------|-----------|------------|------|
| MIT Technology Review | technologyreview.com | https://www.technologyreview.com/latest/ | 기사 링크 전체 URL 포함 |
| Wired | wired.com | https://www.wired.com/category/science/ | 기사 링크 전체 URL 포함 |
| Futurism | futurism.com | https://futurism.com/the-byte | 기사 링크 전체 URL 포함 |
| VentureBeat | venturebeat.com | https://venturebeat.com/category/ai/ | 기사 링크 전체 URL 포함 |
| TechCrunch | techcrunch.com | https://techcrunch.com/latest/ | 기사 링크 전체 URL 포함 |

## 실행 단계

### 1. 기사 목록 수집

사용자 요청에 따라 국내 또는 글로벌 사이트의 카테고리 URL에 WebFetch를 병렬로 적용하여 기사 제목과 URL을 수집합니다.
- 사용자가 주제를 지정한 경우, 해당 주제 관련 기사를 검색합니다.
- 날짜를 지정한 경우 해당 날짜, 지정하지 않은 경우 오늘 날짜의 기사를 가져옵니다.
- 국내/글로벌 구분이 없으면 국내 사이트를 기본값으로 사용합니다.

### 2. 날짜별 필터링

수집된 헤드라인 중 대상 날짜에 발행된 기사만 선택합니다. 기사 수를 기준으로 제외하지 않으며, 해당 날짜의 모든 기사를 포함합니다.

### 3. 주제별 필터링 (주제 지정 시)

사용자가 주제를 지정한 경우, 해당 키워드에 맞는 기사로 추가 필터링합니다.

### 4. 중복 제거

여러 사이트에서 수집된 기사 중 중복 기사를 제거합니다.

### 5. 기사 본문 수집

선택한 각 기사의 날짜, 작성자, 본문을 병렬로 fetch합니다.

### 6. 결과 출력

아래 형식으로 결과를 제공합니다:

```
## 📡 기술 트렌드 (YYYY-MM-DD)

### 1. [기사 제목]
- 출처: [사이트명] | 날짜: [날짜] | 기자: [기자명]
- 요약: [핵심 내용 2–3문장]
- 링크: [URL]

### 2. [기사 제목]
...

---

## 📊 트렌드 분석
[전체 기사에서 공통된 주제나 패턴을 도출합니다. 어떤 기술·산업이 수렴하고 있는지 분석합니다.]

## 💡 핵심 인사이트
[가지수를 제한하지 않고 의미 있는 인사이트를 모두 포함합니다. 각 인사이트는 신호, 시사점, 또는 주목 이유를 담아야 합니다.]
1. [인사이트 — 주목해야 할 이유와 의미]
2. [인사이트]
...
```

## 주의사항

- 뉴스는 항상 최신 내용으로 fetch하며, 캐시된 결과에 의존하지 않습니다.
- 특정 사이트에서 오류가 발생하면 조용히 건너뛰고 다음 사이트를 사용합니다.
- 기사 URL이 상대 경로인 경우, 해당 사이트의 베이스 URL을 앞에 붙여 사용합니다.
- 기사 본문 요약 및 모든 출력은 원문 언어와 무관하게 항상 한국어로 제공합니다.
- 트렌드 분석과 인사이트는 단순 요약을 넘어, 신호와 시사점을 도출해야 합니다.
