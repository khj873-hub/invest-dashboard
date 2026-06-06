# Investment Desk — 통합 리서치 대시보드

`macro-analyst`(매크로 국면)와 `us-stock-chief`(종목 리서치) 두 에이전트의
결과를 한 화면에서 보는 정적 대시보드. 2-tier 동등 레이아웃, GitHub Pages 배포용.

## 구조
```
invest-dashboard/
├─ index.html        대시보드 (JSON을 fetch해서 렌더)
└─ data/
    ├─ macro.json     macro-analyst 출력 (샘플)
    └─ stocks.json    us-stock-chief 출력 (샘플)
```

## 동작 방식 (A안: 정적)
- 페이지가 `data/*.json`을 fetch해서 렌더링한다.
- 에이전트가 분석을 마치면 **표준 JSON 스키마**로 결과를 `data/`에 덮어쓰면 끝.
- API 키가 브라우저에 노출되지 않는다(키는 에이전트 쪽 .env에만 존재).

## 에이전트 연결 (다음 단계)
각 에이전트의 report-writer / market-regime 서브가 화면용 텍스트 외에
이 스키마의 JSON도 함께 출력하도록 한 줄 지시를 추가하면 된다.
- macro-analyst → `data/macro.json` 형식
- us-stock-chief → `data/stocks.json` 형식 (stocks 배열에 종목 누적)

스키마는 동봉된 샘플 JSON이 그대로 계약(contract)이다. 필드명을 지키면
대시보드 수정 없이 바로 반영된다.

## 로컬 실행
file://로 열면 fetch가 차단된다. 반드시 로컬 서버로:
```
cd invest-dashboard
python3 -m http.server 8000
# 브라우저에서 http://localhost:8000
```

## GitHub Pages 배포
1. 이 폴더를 리포지토리에 push (예: invest-dashboard)
2. Settings → Pages → Branch: main / root 선택
3. `https://<id>.github.io/<repo>/` 로 접속
4. 분석 갱신 때마다 `data/*.json`만 commit/push 하면 화면 자동 갱신

## 라이브(B안) 전환 시
index.html 상단 `loadJSON` 함수만 교체한다.
단, 프론트에서 직접 FRED/Finnhub를 부르면 키가 노출되므로
Railway/Vercel 프록시를 거쳐 호출해야 한다(키는 프록시 서버 환경변수에 보관).

## 면책
정보 제공용. 투자 자문이 아니며 판단·책임은 사용자 본인에게 있다.
