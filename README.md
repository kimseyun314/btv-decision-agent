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

**저장소에 커밋된 검증용 샘플**이다 — `docs/3_데이터세트/samples/` (10,000명 + 데모
고객 5명, 기준일 2026-09-01). 학습된 모델 12종이 실제로 낸 출력이다.

샘플은 `make_samples.py --mode joined` 로 잘려 **원장·예측입력·예측결과가 같은
고객**을 가리킨다. 그래서 "이 고객의 상품 구성은 이렇고, 그래서 이 확률이 나왔다"
를 화면에서 그대로 확인할 수 있다.

세분 예측 8종은 상품 가입 여부에 따라 채워지거나 비며, 「예측·신호 커버리지」
카드가 **"해당 상품 미가입" · "이미 최고 등급" · "미제공"** 을 구분해 보여준다.

확률 분포가 합성과 다르므로(학습된 모델의 최대 해지 확률 0.686) 정책 임계값도
`signals.yaml` 의 `calibration.profiles.prediction` 으로 함께 갈아끼운다.
구조·연산자·상호배타 경계의 의미는 그대로다.

데모 고객 A~E 는 실데이터에 대응물이 없어 합성값 그대로다 — 발표의 A↔B 대조가
이 5명에 걸려 있기 때문이다. 이들은 커버리지 카드가 전부 "미제공" 으로 보인다.


## 갱신 방법

팀 저장소에서 빌드해 이 저장소에 덮어쓴다.

```bash
cd backend && POPULATION_SOURCE=prediction \
  POPULATION_DATA_ROOT="docs/3_데이터세트/samples" LLM_ENABLED=false \
  .venv/bin/uvicorn app.main:app --port 8000 &
cd frontend && npm run build:static
# dist/ 를 이 저장소에 복사해 커밋·푸시
```
