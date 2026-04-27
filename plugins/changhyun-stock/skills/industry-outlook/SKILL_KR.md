---
name: industry-outlook
description: 주요 글로벌 싱크탱크, 컨설팅 기업, 연구기관의 최신 경제·미래 산업 전망 아티클을 수집·요약하고 인사이트를 도출합니다.
model: haiku
---

# 산업 전망 분석

WebFetch를 활용해 주요 글로벌 싱크탱크, 컨설팅 기업, 국내외 연구기관의 최신 경제·미래 산업 전망 아티클을 수집·분석하고, 경제 전망, 산업/비즈니스 인사이트, 투자 인사이트를 구조적으로 제공합니다.

## 아티클 출처

### 글로벌

| 사이트 | 기본 URL | 카테고리 URL | 비고 |
|--------|----------|--------------|------|
| Deloitte Insights | deloitte.com | https://www.deloitte.com/us/en/Industries/industry-outlooks.html | 아티클 링크에 전체 URL 포함 |
| Oxford Economics | oxfordeconomics.com | https://www.oxfordeconomics.com/ | 아티클 링크에 전체 URL 포함 |
| Harvard Business Review | hbr.org | https://hbr.org/topic/subject/economics | 아티클 링크에 전체 URL 포함 |
| Project Syndicate | project-syndicate.org | https://www.project-syndicate.org/section/economics | 상대 URL — `https://www.project-syndicate.org` 앞에 붙이기 |
| MIT Sloan Management Review | sloanreview.mit.edu | https://sloanreview.mit.edu/topic/strategy/ | 아티클 링크에 전체 URL 포함 |
| Brookings Institution | brookings.edu | https://www.brookings.edu/topic/economic-studies/ | 아티클 링크에 전체 URL 포함 |

### 국내

| 사이트 | 기본 URL | 카테고리 URL | 비고 |
|--------|----------|--------------|------|
| KIET 산업연구원 | kiet.re.kr | https://www.kiet.re.kr/trends/ecolookList | 상대 URL — `https://www.kiet.re.kr` 앞에 붙이기 |
| 현대경제연구원 (HRI) | hri.co.kr | https://www.hri.co.kr/ | 아티클 URL 형식: `https://www.hri.co.kr/report/report-view.html?bmain=view&uid=<uid>` |

## 실행 단계

### 1. 아티클 목록 수집

사용자 요청에 따라 카테고리 URL에 WebFetch를 병렬로 실행합니다.
- 사용자가 특정 주제를 지정한 경우, 해당 주제 관련 아티클을 우선 수집합니다.
- 주제가 지정되지 않은 경우, 최신 아티클을 수집합니다.
- 국내/글로벌이 지정되지 않은 경우, 글로벌 사이트를 기본으로 사용합니다.
- 사이트당 최대 5개 아티클을 수집합니다.

### 2. 아티클 필터링

- 중복 아티클을 제거합니다.
- 단순 현황 보도보다 전망·예측·미래 트렌드를 다루는 아티클을 우선합니다.

### 3. 아티클 본문 수집

선별된 아티클의 날짜, 저자, 본문을 병렬로 수집합니다.

### 4. 결과 출력

아래 형식으로 출력합니다.

```
## 🌐 산업 전망 (YYYY-MM-DD)

### 1. [아티클 제목]
- 출처: [사이트명] | 날짜: [날짜] | 저자: [저자]
- 요약: [핵심 내용 2–3문장]
- 링크: [URL]

### 2. [아티클 제목]
...

---

## 📊 경제 전망
[수집된 아티클 기반 거시경제 트렌드 및 전망 요약. 전체적인 경제 흐름은?]

## 🏭 산업·비즈니스 인사이트
1. [인사이트 — 주목해야 할 이유와 산업/비즈니스 시사점]
2. [인사이트]
...

## 💰 투자 인사이트
1. [인사이트 — 주목해야 할 이유와 투자 관점 시사점]
2. [인사이트]
...
```

## 참고 사항

- 항상 최신 콘텐츠를 수집합니다. 캐시된 결과에 의존하지 마세요.
- 사이트에서 오류가 반환되면 조용히 건너뛰고 다음 사이트를 사용합니다.
- 상대 URL은 사이트의 기본 URL을 앞에 붙여 완성합니다.
- 원문 언어에 관계없이 모든 내용은 한국어로 요약·출력합니다.
- 인사이트는 단순 요약을 넘어 신호와 시사점을 도출해야 합니다.
- 경제/비즈니스 인사이트와 투자 인사이트를 명확히 구분합니다.
