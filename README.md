# B tv AI Decision Agent — 데모 배포본

SK AI 해커톤 과제의 **빌드 결과물만** 담은 저장소다. 소스는 팀 저장소에 있다.

👉 **https://kimseyun314.github.io/btv-decision-agent/**

## 이게 왜 따로 있나

GitHub Pages 는 정적 파일만 서빙한다. FastAPI 가 돌 곳이 없으므로, 빌드 시점에
백엔드를 잠깐 띄워 7개 엔드포인트의 응답을 JSON 으로 뜨고 프론트와 함께 올린다.
화면 코드는 그대로이고, `client.ts` 가 정적 모드에서 그 파일을 읽을 뿐이다.

**결정은 Policy Layer 가 내린 것이다.** 내보내기는 결과를 옮겨 적을 뿐 아무것도
계산하지 않는다.

## 지금 보이는 데이터

합성 모집단 **12,005명**(고정 시드 20260824)이다. 학습된 예측 모델의 실데이터는
아직 배선되지 않았다 — 세분 예측 8종이 비어 있는 것이 그 때문이며, 화면의
「예측·신호 커버리지」 카드가 그 사실을 그대로 보여준다.

## 갱신 방법

팀 저장소에서 빌드해 이 저장소에 덮어쓴다.

```bash
cd backend && LLM_ENABLED=false .venv/bin/uvicorn app.main:app --port 8000 &
cd frontend && npm run build:static
# dist/ 를 이 저장소에 복사해 커밋·푸시
```
