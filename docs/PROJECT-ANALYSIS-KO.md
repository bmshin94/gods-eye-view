# God's Eye View — 프로젝트 분석 & 활용 정리 (한국어)

> 이 저장소를 처음 받아본 사람이 **"이게 뭐고, 어떻게 쓰고, 뭘 할 수 있는지"** 를
> 한 번에 파악할 수 있도록 정리한 문서입니다.

## 🔗 저장소 주소

| 구분 | 주소 |
|---|---|
| **이 저장소 (포크)** | https://github.com/bmshin94/gods-eye-view |
| **원본 (upstream)** | https://github.com/bilawalsidhu/gods-eye-view |
| 원작자 | Bilawal Sidhu |
| 라이선스 | MIT (**코드만** — 데이터는 별도, 아래 3장 참고) |

---

## 목차

1. [프로젝트 개요](#1-프로젝트-개요)
2. [코드베이스 구조](#2-코드베이스-구조)
3. [⚠️ 라이선스 — 상업화 전 필독](#3-️-라이선스--상업화-전-필독)
4. [설치 및 실행](#4-설치-및-실행)
5. [사용법](#5-사용법)
6. [API 키 가이드](#6-api-키-가이드)
7. [문제 해결](#7-문제-해결)
8. [수익화 아이디어](#8-수익화-아이디어)
9. [PHP 전환 검토](#9-php-전환-검토)

---

## 1. 프로젝트 개요

### 한 줄 정의

> **"살아 움직이는 구글어스"** — 3D 지구본 위에 전 세계가 지금 이 순간 공개로
> 송출 중인 신호를 실시간으로 얹어서 보여주는 브라우저 앱.

구글어스가 *과거에 찍어둔 사진* 이라면, 이건 *지금 일어나는 일* 입니다.
비행기의 ADS-B 트랜스폰더, 선박의 AIS 비콘, 위성의 궤도 요소, 지진계,
공공 CCTV — 전부 원래 공개되어 있던 신호이고, 이 프로젝트는 그것을
**한 화면에 모아주는 인터페이스**입니다. 기밀 접근 권한은 필요 없습니다.

### 배경

- 유튜브 God's Eye View 시리즈(구 WorldView, 5백만 뷰 이상)에서 파생된 프로젝트
- 2026년 오픈소스 공개, **GitHub 트렌딩 1주간 1위**
- 프레임워크 없이 **바닐라 JS + CesiumJS + Vite** 로 구현

### 실시간 레이어 13종

13개 중 **11개가 키·회원가입 없이** 동작합니다. (🟢 무료 · 🟡 무료 키 · 🔴 과금)

| 레이어 | 내용 | 출처 | 키 |
|---|---|---|---|
| 🗺️ 맵 스택 | Esri 위성 / 구글 3D / OSM | Esri·Google·ion·OSM | 🟢🟡🔴 |
| ✈️ 항공기 | 실시간 11,000대+ · 항적 이력 | OpenSky + adsb.lol | 🟢 |
| 🎖️ 군용기 | ADS-B 군용 트래픽 | adsb.lol | 🟢 |
| 🚢 선박 | 전 세계 AIS | AISStream | 🟡 |
| 🛰️ 위성 | 838개 · 스타링크 셸 | CelesTrak | 🟢 |
| 🌍 지진 | 최근 24시간 | USGS | 🟢 |
| 🚗 교통 | 실시간 정체 → 차량 흐름 | TomTom + OSM | 🟢/🟡 |
| 📹 CCTV | 공공 카메라 약 800대를 3D에 투영 | 오스틴·Caltrans·TfL | 🟢 |
| 📻 라디오 | 아날로그 튜너 · 최대 750국 | Radio Browser | 🟢 |
| 🚲 공유자전거 | 실시간 거치대 현황 | GBFS | 🟢 |
| 🔥 산불 | NASA 실시간 감지 (24h) | NASA FIRMS | 🟡 |
| 🚀 우주발사 | 최근 30일 · 페이로드/회수 정보 | Launch Library 2 | 🟢 |
| 🎖️ 군사시설 | 뷰포트 기반 매핑 컨텍스트 | OpenStreetMap | 🟢 |

**번들 정적 데이터:** 데이터센터 4,351개 · 댐 704개 · 해저케이블 712개

### 킬러 기능

- **🛩️ 콕핏 모드** — 실제 비행 중인 항공기를 클릭 → `C` → 조종석 탑승.
  실제 지형이 아래로 흐르고, 비행 중 야시경/열화상 센서 교체 가능
- **🎙️ 음성 조종** — OpenAI Realtime 기반, **툴 28개** 등록.
  *"LAX로 가서 가장 가까운 항공기 선택해"* 같은 명령이 실제로 동작
- **🖊️ 음성 화이트보드** — *"텍사스 외곽선 그려줘"* → 실제 행정경계 폴리곤을 그림
- **🎨 센서 필터** — GLSL 셰이더로 CRT/NVG/FLIR/느와르/스노우

---

## 2. 코드베이스 구조

### 규모

| 항목 | 수치 |
|---|---|
| 전체 파일 | 461개 |
| src 코드 라인 | 약 158,000줄 |
| JS 모듈 | 146개 |
| **테스트 파일** | **165개 (모듈 수보다 많음)** |
| Cesium 임포트 파일 | 84개 |
| 프론트 → `/api/*` 호출 지점 | 54곳 |
| 서버 미들웨어 마운트 | 47개 |

### 폴더 지도

```
gods-eye-view/
├── vite.config.js   ⚠️ 7,735줄 — 이름만 설정파일, 실제로는 백엔드
│                       /api/* 프록시 라우트 28종이 전부 여기 있음
├── index.html       55KB — UI 마크업 전체
├── style.css        256KB — HUD·패널 디자인 전체
│
├── src/
│   ├── main.js              부팅 · 레이어 등록
│   ├── ui.js / hud.js       런타임 UI · 인텔리전스 HUD
│   ├── keySetup.js          POWER UP 패널 (인앱 키 입력, dev 서버 전용)
│   ├── mapStackController.js 베이스맵 전환 (Google 3D / Esri / OSM / ion)
│   ├── iconOrientation.js   화면 투영 기반 실세계 헤딩 + 수평선 컬링
│   ├── data/                레이어 1개당 모듈 1개 + 오케스트레이션
│   │   ├── local_data/      번들 데이터셋 (폴더별 출처 README 포함)
│   │   └── fixtures/        테스트 픽스처
│   ├── voice/               OpenAI Realtime 세션 + 음성 툴 28개
│   ├── styles/              GLSL 센서 필터 (noir/retro/anime/thermal/snow/surveillance)
│   ├── overlays/            탐지 오버레이 (워커 스레드 포함)
│   ├── annotations/         음성 주석 엔진 (스크린/월드/하이브리드 렌더러)
│   └── scenes/              시네마틱 씬 디렉터
│
├── scripts/         셋업 닥터 + QA 스크립트 50여 개 (puppeteer 자동 검증)
├── pinokio/         원클릭 설치 런처
├── config/          CCTV 카메라 카탈로그 (austin, shinjuku)
├── tools/           오프라인 렌더링·파노라마 유틸
├── docs/            CURRENT-STATE · KNOWN-ISSUES · PERFORMANCE
└── DATA_SOURCES.md  21KB — 데이터 출처·라이선스 전수 문서
```

### 엔지니어링 관점의 볼거리

- **월드 고정 아이콘** — 화면 좌표계 투영으로 실제 헤딩 유지 (카메라 각도 무관)
- **끊긴 데이터의 부드러운 보간** — 15~30초 간격 피드를 1주기 지연 렌더 + 추측항법으로 메움
- **정직한 위성** — SGP4 전파, GMST 재정렬로 궤도링 드리프트 제거
- **실제 지면 접지** — EGM96 지오이드 + 렌더된 지형 메시 샘플링 기반 수직 데이텀
- **쿼터 방어** — OpenSky 크레딧 거버너, TomTom 일일 타일 예산, TLE 디스크 캐시
- **키 브로커링** — 개인 키가 붙는 API는 전부 서버사이드 프록시 경유
  (SSRF 방어 · 응답 크기 상한 · 에러 새니타이즈). 브라우저에 내려가는 키는
  Google Maps / Cesium ion 둘뿐 (반드시 제공사에서 URL 제한 설정할 것)

---

## 3. ⚠️ 라이선스 — 상업화 전 필독

**코드는 MIT라 상업적 사용이 자유롭지만, 데이터는 각자 다른 라이선스입니다.**
(근거: `DATA_SOURCES.md`)

### 🔴 상업적 사용 시 반드시 처리해야 하는 항목

| 데이터 | 문제 | 조치 |
|---|---|---|
| **OpenSky** (항공기) | **비상업 라이선스.** 라이브 제품에서 REST API 운용 시 사전 서면 합의가 필요할 수 있음 | `adsb.lol`(ODbL, 상업 가능)로 대체 또는 OpenSky와 별도 계약 |
| **TeleGeography** (해저케이블) | **CC BY-NC-SA — NonCommercial** | `src/data/local_data/telegeography_submarine_cables/` 폴더 삭제 (자체 완결형이라 지워도 나머지 동작) 또는 상업 라이선스 구매 |
| **Google News RSS** (콕핏 헤드라인) | 개인·비상업 사용만 허용 | GDELT로 대체 (상업 사용 가능, 인용 필요) |
| **Cesium ion 무료 Community** | 개인·비상업 한정 | 유료 ion 플랜 또는 Google Maps 키 직접 사용 |
| **AISStream** (선박) | 무료 베타, 정식 약관 없음 | 상용은 유료 위성 AIS 검토 |

### 🟢 상업적 사용이 깨끗한 조합

```
지진(USGS, 미국 공공저작물)
+ 산불(NASA FIRMS)
+ 위성(CelesTrak)
+ 우주발사(Launch Library 2)
+ 항공기(adsb.lol, ODbL)
+ 데이터센터·댐(OSM ODbL — 출처 표기 + 데이터 share-alike)
+ CCTV(공공 API — 런던 TfL은 출처 표기 필수)
+ 지형(Re:Earth, CC BY 4.0)
```

이 조합만으로도 **재난·인프라 모니터링 제품**이 성립합니다.

### 그 외 주의사항

- **출처 표기 유지 필수** — 인앱 크레딧 라인과 "Data attribution" 팝오버를 지우면 안 됨
- **Google Maps 콘텐츠는 캐시·저장·재호스팅 금지** — 항상 라이브로만 사용
- **README의 데모 GIF는 단독 재사용 불가** (프로모션 용도로만 허용)
- **원작자 정책** — 특정 개인 검색·얼굴 인식·개인 추적 기능은 추가하지 않으며 관련 PR도 머지되지 않음

---

## 4. 설치 및 실행

### 0단계. Node.js 버전 확인 ← 가장 흔한 실패 원인

```bash
node -v
```

| 버전 | 결과 |
|---|---|
| v22 이하 | ❌ 실행 불가 |
| **v24.14.0 이상 ~ v25 미만** | ✅ 정상 |
| v25 | ⚠️ 동작하나 EOL — 셋업 닥터가 경고 |
| **v26.x** | ✅ 정상 |

버전이 맞지 않으면:

```bash
nvm install 24
nvm use 24
```

nvm이 없으면 https://nodejs.org 에서 24 LTS 설치.

### 방법 A — 터미널 (권장)

```bash
cd gods-eye-view
npm install       # 2~3분
npm run doctor    # 환경 점검 (권장)
npm run dev
```

접속: **http://localhost:4173**

`npm run doctor` 출력 예시 — `[--]` 는 "미설정"이며, **전부 `[--]` 여도 정상 실행**됩니다.

```
[OK] Cesium ion (env)
[--] OpenAI voice
[--] AISStream vessels
```

**macOS 보너스**

```bash
./scripts/dev-fresh.sh   # Vite 캐시 정리 + 키체인에서 키 자동 로드
```

### 방법 B — Pinokio 원클릭

1. https://pinokio.computer 에서 Pinokio 설치
2. **Discover → Download from URL** 에 저장소 주소 붙여넣기
   - 이 포크: `https://github.com/bmshin94/gods-eye-view`
   - 원본: `https://github.com/bilawalsidhu/gods-eye-view`
3. **Install** → **Start**

> 🚨 **Pinokio 8.0.40 주의**
> Pinokio 자체의 **Configure** 패널에 API 키를 입력하지 마세요.
> 해당 릴리스는 중첩 앱 파일을 올바르게 저장하지 못하며 **입력값을 로그에 남깁니다.**
> 키는 반드시 앱 내부의 **POWER UP** 패널로 입력하세요.

### 빌드 (배포용)

```bash
npm run build     # dist/ 에 정적 파일 생성
npm run preview
```

### 테스트

```bash
npm test          # 유닛 테스트 165개
```

---

## 5. 사용법

### 첫 5분 코스

1. **Flights** 레이어 ON → 실시간 항공기 수천 대 표시
2. 항공기 하나 클릭 → 카메라 자동 추적 + 항적 + 텔레메트리 카드
3. **`C`** → 콕핏 진입. 실제 지형 위를 비행
4. **`1`~`7`** → 센서 필터 전환 (CRT/NVG/FLIR/느와르/스노우)
5. **CCTV** 레이어 ON → 오스틴·런던·캘리포니아에서 공공 카메라를 3D 공간에 투영
6. **Satellites** ON → ISS 클릭 → 궤도링과 함께 동반 비행
7. **`Esc`** 또는 **Reset Globe** 로 복귀

### 단축키

| 키 | 기능 |
|---|---|
| `1` ~ `7` | 비주얼 스타일 전환 |
| `H` | HUD 토글 |
| `D` | 탐지 오버레이 토글 |
| `C` | 콕핏 진입 |
| `Esc` | 빠져나오기 |

### 음성 명령 (OpenAI 키 필요)

**GEV MIC** 클릭 → 마이크 권한 허용 → 발화. 실제 테스트된 명령들:

**카메라 조종**
- *"Take me to Tokyo."* / *"도쿄로 데려가줘"*
- *"Orbit around this area slowly."*
- *"Zoom out to a globe view."*
- *"Draw the walking route from the Capitol to Zilker Park."* → *"Fly the route we just drew."*

**주석 (화이트보드)**
- *"Outline the state of Texas."* — 실제 경계 폴리곤을 그림
- *"How far is the Eiffel Tower from the Louvre?"* — 화살표 + 거리 음성 안내
- *"Clear the map."* — 전체 삭제

**분석 질의**
- *"How many flights are over Texas right now?"*
- *"What is the biggest fire near Los Angeles?"*
- *"When does the ISS pass over next?"*
- *"Which ships are headed to Oakland?"*

**콘솔 조작**
- *"Switch to night vision and turn on the flights layer."*
- *"Track that plane."* → *"Enter cockpit."*
- *"Play a news radio station near Austin."*
- *"What's turned on right now?"*

### 필드 미션 (익숙해진 뒤)

- **최종 접근** — 착륙 정렬 중인 여객기를 추적 → 콕핏 → 활주로까지 동승
- **야간 감시** — 자기 도시로 이동 → NVG → 탐지 메시 + HUD
- **입항** — 롱비치항에 선박 레이어 → 유조선 클릭 → CCTV 패널의 **NEAREST**
- **화재선** — 캘리포니아에 FIRMS → 감지점 클릭 → **NEAREST** 로 지상 카메라
- **발사 재생** — Space Missions → 최근 30일 발사 선택 → 0.25×~4× 스크럽
  (`RECONSTRUCTED ESTIMATE` 라벨 — 실측 텔레메트리가 아닌 재구성)

---

## 6. API 키 가이드

**키 없이도 13개 중 11개 레이어가 동작합니다.** 키는 필수가 아니라 업그레이드입니다.

### 입력 방법

앱 우측 하단 **POWER UP** 클릭 → Provider Settings → 붙여넣기 → **SAVE KEYS** → 자동 재시작.
버튼이 보이지 않으면 `http://localhost:4173/?setup=1`

저장 위치:
- Pinokio 실행 시 → `pinokio/ENVIRONMENT` (gitignore 됨)
- 터미널 클론 → 저장소 루트 `.env`
- 두 파일 모두 **비밀 값 기록 전에 소유자 전용 권한으로 설정**되며 로컬을 벗어나지 않음

### 추천 순서

| 순위 | 키 | 효과 | 비용 | 발급처 |
|---|---|---|---|---|
| 1 | **Cesium ion** | 🗺️ 구글 포토리얼 3D 도시 + 월드 지형. 체감 차이 최대 | 무료(개인·비상업) | https://cesium.com/ion |
| 2 | **AISStream** | 🚢 전 세계 선박 | 무료 | https://aisstream.io |
| 3 | **NASA FIRMS** | 🔥 실시간 산불 | 무료 | https://firms.modaps.eosdis.nasa.gov/api/map_key/ |
| 4 | **TomTom** | 🚦 시뮬레이션 → 실제 교통량 | 무료 티어 | https://developer.tomtom.com |
| 5 | **OpenAI** | 🎙️ 음성 + AI HUD 요약 | **과금** | https://platform.openai.com |
| 옵션 | **Google Maps** | 구글 3D 직접 + 장소 검색 | **과금** | Google Cloud Console |
| 옵션 | **OpenSky** | ✈️ 폴링 크레딧 증가 | 무료 | https://opensky-network.org |
| 옵션 | **Launch Library 2** | 🚀 요청 한도 상향 | 무료 | https://thespacedevs.com |

### 파일로 입력하는 경우

`.env.example` 참고. 주요 변수:

```
CESIUM_ION_TOKEN=
GOOGLE_MAPS_API_KEY=
OPENAI_API_KEY=
AISSTREAM_API_KEY=
FIRMS_MAP_KEY=
TOMTOM_API_KEY=
OPENSKY_CLIENT_ID=
OPENSKY_CLIENT_SECRET=
LL2_API_TOKEN=
PORT=4173
```

### 💸 비용 방어 장치

음성(OpenAI Realtime)이 유일하게 실비가 나가는 기능이며, 앱이 스스로 막습니다.

- 마이크 옆 **실시간 세션 사용액 표시**
- **$2 경고** → **$5 하드캡 시 세션 강제 종료**
- `STD` / `MINI` 모델 토글
- 음성 컨텍스트 윈도우를 의도적으로 짧게 유지

TomTom은 일일 타일 예산(`TOMTOM_DAILY_TILE_BUDGET`, 기본 40,000)으로 방어됩니다.
단, 이는 **앱 레벨 안전장치이지 청구 상한이 아니므로** 제공사 쪽 예산 알림도 설정하세요.

### 🔒 공유 시 주의

기본값은 **localhost 바인딩**이라 외부에서 접근 불가입니다.
`--host 0.0.0.0` 으로 LAN에 열면 **접근 가능한 누구나 설정된 API 키를 사용**하게 됩니다.
공유 모드에서는 Provider Settings가 자동으로 비활성화되지만,
반드시 제공사 측 예산 상한과 `GEV_RATELIMIT_*` 스로틀을 먼저 설정하세요.
자세한 위협 모델은 `SECURITY.md` 참고.

---

## 7. 문제 해결

| 증상 | 원인 / 해결 |
|---|---|
| `npm install` 실패 | Node 버전 확인 (24 또는 26) — 대부분 이것 |
| 지구가 밋밋함 / 2D 같음 | 정상. Cesium ion 무료 토큰을 넣으면 3D 도시 활성화 |
| 공항에서 항공기가 떠 있음 | 알려진 이슈. 지형 고도 해석에 30~60초(1~2 폴링 주기) 소요 |
| 도심에서 교통 표시가 느림/불균일 | 알려진 이슈. 뷰포트 타일을 순차 로드하는 구조 |
| CCTV 패널이 사라짐 | 패널 위치가 localStorage에 저장되어 화면 밖으로 나간 경우 |

**CCTV 패널 복구** — 브라우저 콘솔(F12):

```js
localStorage.removeItem('godsEyeView.v7.panelPos.cctv-panel');
localStorage.removeItem('godsEyeView.v6.panelCollapsed.cctv-panel');
location.reload();
```

관련 키: 패널 위치 `godsEyeView.v7.panelPos.<panel-id>` ·
접힘 상태 `godsEyeView.v6.panelCollapsed.<panel-id>` ·
CCTV 캘리브레이션 `godsEyeView.cctv.calibration.v2`

전체 목록은 `docs/KNOWN-ISSUES.md` 참고.

---

## 8. 수익화 아이디어

> 전제: **3장(라이선스)의 정리 작업을 먼저 끝내야 합니다.**
> 특히 OpenSky → adsb.lol 교체, 해저케이블 폴더 삭제, 구글뉴스 → GDELT 교체.

### 아이디어 목록

| # | 아이디어 | 내용 | 진입장벽 | 단가 |
|---|---|---|---|---|
| 1 | **🇰🇷 한국 데이터 레이어 팩** | ITS 국가교통정보센터 CCTV·소통정보, 기상청 API 허브, 해양수산부 항만, 국토지리정보원 DEM, 소방청 재난정보 등을 레이어로 추가 | 중 | 높음 |
| 2 | **전시·키오스크·기업 로비 스크린** | 데이터센터 기업(레이어에 4,351개 이미 존재), 물류·해운사, 항공사, 에너지 기업(댐 704개), 박람회·과학관 | 낮음 | **매우 높음** |
| 3 | **방송용 그래픽** | 재난 보도(지진·산불)는 데이터가 전부 무료 + 상업 사용 가능. 씬 디렉터로 큐 구성, 공유링크로 카메라 상태 직렬화 | 중 | 높음 |
| 4 | **콘텐츠 채널** | 원작자가 검증한 모델. 한국어권 공백. 위 1~3번의 영업 자산이 됨 | 낮음 | 중 |
| 5 | **교육 상품** | 코드가 MIT라 라이선스 리스크 0. 주제: 프레임워크 없는 대규모 앱, **과금 API 안전 설계**, AI 음성 에이전트, 3D 지오스페이셜 입문 | **가장 낮음** | 중 |
| 6 | **B2B 버티컬 SaaS** | 재난·보험 리스크(FIRMS+USGS, 데이터 완전 무료·상업 가능) / 물류 가시성(위성 AIS 유료 필요) / 인프라 모니터링 | 높음 | 매우 높음 |
| 7 | **커스터마이징 대행** | "우리 데이터 얹어주세요" 용역. 원작자 1인이 전부 소화 불가 → 한국·아시아 포지셔닝 | 낮음 | 중 |

### 권장 경로

```
5번(교육·콘텐츠)으로 인지도 확보
  → 7번(대행) + 2번(전시·키오스크)로 현금흐름 확보
    → 1번(한국 레이어 팩)을 자산으로 축적
```

**근거**
- 5번: 라이선스 리스크가 없고 즉시 시작 가능
- 2번: 단가가 높고 데모 하나로 계약이 성사되는 유형
- 1번: 시간이 갈수록 대체 불가능한 해자가 됨

### 리스크

원작자 브랜드가 강해서 **동일 제품을 그대로 재판매하는 방식은 통하지 않습니다.**
반드시 *한국 데이터* 또는 *특정 산업 버티컬* 로 좁혀야 경쟁력이 생깁니다.

### 실행 순서

1. 상업 버전 브랜치 생성 — 해저케이블 삭제, OpenSky→adsb.lol, 구글뉴스→GDELT
2. 한국 데이터 레이어 1개 추가 (ITS CCTV 권장 — 기존 CCTV 모듈 구조 재사용 가능)
3. 해당 화면으로 데모 영상 제작
4. 타겟 3곳 콜드메일 (데이터센터 기업 / 물류사 / 지역 방송)
5. 병행: 원본 저장소에 PR 1건 — 컨트리뷰터 이력이 영업 자산이 됨

---

## 9. PHP 전환 검토

### 결론

```
🖥️ 프론트엔드 (지구본 화면)  →  ❌ PHP 불가능
⚙️ 백엔드 (API 서버)         →  ✅ PHP로 전면 교체 가능 (권장)
```

### 프론트엔드가 불가능한 이유

**84개 파일이 CesiumJS를 임포트**합니다. CesiumJS는 **WebGL** 기반이며,
WebGL은 브라우저가 GPU를 직접 제어하는 기술이라 **브라우저 내 JavaScript로만** 실행됩니다.
PHP는 서버에서 실행되는 언어이므로 영역이 다릅니다.
(구글어스·카카오맵도 동일한 제약을 갖습니다.)

### 백엔드가 가능한 이유

구조를 보면 프론트와 백엔드의 접점은 **JSON을 주고받는 HTTP 엔드포인트뿐**입니다.

```
브라우저 (JS + Cesium)
     ↕  fetch('/api/...') — 호출 지점 54곳
서버 (현재는 vite.config.js 안의 미들웨어 47개)
     ↕
외부 API (NASA, USGS, OpenAI, TomTom, ...)
```

`/api/firms` 가 산불 JSON만 반환하면 되며, **그 구현체가 Node인지 PHP인지는 무관**합니다.

### 중요: 상업화하려면 어차피 백엔드를 새로 써야 합니다

현재 서버 역할을 하는 `vite.config.js` 는 **개발용 Vite dev server** 입니다.
README에도 *"hardened production service가 아니다"* 라고 명시되어 있습니다.
따라서 **상용 배포 시 별도 백엔드 구현이 필수**이며, PHP는 충분히 타당한 선택입니다.

특히 국내 환경에서는:
- 공유 호스팅 대부분이 PHP 기반
- 지자체·공공기관 납품 스택이 여전히 PHP + Apache 중심
- 8장의 "한국 데이터 레이어 팩" 노선과 궁합이 좋음

### 목표 아키텍처

```
┌─────────────────────────────────────────┐
│  브라우저                                │
│  Cesium 지구본 (JS · 기존 코드 재사용)    │  ← 수정 불필요
└──────────────┬──────────────────────────┘
               │ fetch('/api/...')
┌──────────────▼──────────────────────────┐
│  Apache / Nginx + PHP                    │
│  ├ /api/firms          산불 프록시 + 캐시  │
│  ├ /api/earthquakes    지진               │
│  ├ /api/celestrak      위성 TLE           │
│  ├ /api/its-cctv       🇰🇷 한국 CCTV (신규)│
│  ├ /api/realtime/token OpenAI 토큰 발급   │
│  └ 키 보관 · 쿼터 관리 · 레이트리밋        │
└─────────────────────────────────────────┘
```

빌드 결과물은 정적 파일이므로 그대로 웹서버에 올리면 됩니다.

```bash
npm run build   # dist/ 생성 → 웹서버 루트에 배치, /api/* 만 PHP가 처리
```

### 이식 난이도

| 부분 | PHP 가능 | 비고 |
|---|---|---|
| 지구본 렌더링 | ❌ | JS 그대로 재사용 (수정 불필요) |
| UI · HUD · 셰이더 | ❌ | JS 그대로 |
| API 프록시 45개 | ✅ | 대부분 단순 프록시 — 난이도 낮음 |
| 키 보관 · 토큰 발급 | ✅ | PHP가 오히려 편함 |
| 캐시 · 쿼터 관리 | ✅ | Redis / 파일 캐시 |
| **AIS 실시간 (선박)** | ⚠️ | **아래 참고** |

**실제 작업량:** 프론트 15만 줄은 손댈 필요가 없고, 서버만 새로 작성하면 됩니다.
그중 절반은 사용하지 않을 기능(런던 CCTV 등)이므로 **실질적으로 10~15개
엔드포인트만 이식하면 충분**합니다.

### 예시 — 프록시 1개 (순수 PHP)

```php
<?php
// api/firms.php — NASA 산불 프록시 (캐시 포함)

declare(strict_types=1);
header('Content-Type: application/json; charset=utf-8');

$cacheFile = sys_get_temp_dir() . '/firms_cache.json';
$cacheTtl  = 300; // 5분

// 1) 캐시가 유효하면 그대로 반환 (쿼터 절약)
if (is_file($cacheFile) && (time() - filemtime($cacheFile)) < $cacheTtl) {
    readfile($cacheFile);
    exit;
}

// 2) 키는 서버에만 — 브라우저로 내려가지 않음
$key = getenv('FIRMS_MAP_KEY');
if (!$key) {
    http_response_code(503);
    echo json_encode(['error' => 'FIRMS key not configured', 'rows' => []]);
    exit;
}

// 3) 외부 호출 (타임아웃 · 리다이렉트 차단 = SSRF 방어)
$url = "https://firms.modaps.eosdis.nasa.gov/api/area/csv/{$key}/VIIRS_SNPP_NRT/world/1";
$ch  = curl_init($url);
curl_setopt_array($ch, [
    CURLOPT_RETURNTRANSFER => true,
    CURLOPT_TIMEOUT        => 10,
    CURLOPT_FOLLOWLOCATION => false,
    CURLOPT_SSL_VERIFYPEER => true,
]);
$body = curl_exec($ch);
$code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

// 4) 실패 시 오래된 캐시라도 제공 (fail-soft)
if ($body === false || $code !== 200) {
    if (is_file($cacheFile)) { readfile($cacheFile); exit; }
    http_response_code(502);
    echo json_encode(['error' => 'upstream failed', 'rows' => []]);
    exit;
}

// 5) 변환 후 캐시 저장
$rows = array_map('str_getcsv', explode("\n", trim($body)));
$out  = json_encode(['rows' => $rows, 'source' => 'NASA FIRMS'], JSON_UNESCAPED_UNICODE);

file_put_contents($cacheFile, $out, LOCK_EX);
echo $out;
```

47개 미들웨어 중 대부분이 위 패턴(**키 주입 → 외부 호출 → 캐시 → JSON 반환**)의
반복입니다. Laravel을 쓰면 더 짧아집니다.

```php
Route::get('/api/firms', fn () =>
    Cache::remember('firms', 300, fn () => Http::timeout(10)->get($url)->json())
);
```

> ⚠️ 이식할 때 **원본의 방어 로직을 함께 옮겨야 합니다**:
> SSRF 방어(리다이렉트 차단·주소 검증), 응답 크기 상한, 에러 새니타이즈,
> 쿼터 거버너, 옵트인 레이트리밋. 이 부분이 원본 코드에서 가장 배울 만한 자산입니다.

### 유일한 난관 — AIS(선박)

`/api/ais-live` 는 다른 구조입니다. AISStream과 **WebSocket 연결을 상시 유지**하면서
서버 기동 이후의 선박 위치를 누적합니다(`ensureAisStreamConnection`, `readAisTrack`).
PHP는 기본적으로 요청·응답 단위로 동작하므로 상시 연결에 부적합합니다.

| 방안 | 내용 | 난이도 |
|---|---|---|
| PHP 데몬 | Workerman / Swoole / ReactPHP로 상주 프로세스 → Redis에 적재 → PHP는 Redis만 조회 | 중 |
| **Node 워커만 유지** ⭐ | AIS 수집만 소형 Node 스크립트로 분리, 나머지는 전부 PHP | 낮음 |
| AIS 제외 | AISStream은 정식 약관이 없어 상업용에는 어차피 부적합 | 최저 |

### 권장 스택

- **Laravel + Redis** (일반 상용)
- **순수 PHP + 파일 캐시** (지자체 납품 등 경량 요건)

### 이식 실행 순서

1. `npm run build` 로 `dist/` 생성
2. 최소 세트 5개만 PHP로 이식 — 지진 · 산불 · 위성 · 항공기(adsb.lol) · 지형
3. 한국 데이터 1개 추가 (ITS CCTV)
4. Apache/Nginx + PHP 로 배포
5. 이 시점이 곧 "상업 납품 가능 버전"

---

## 참고 문서

| 파일 | 내용 |
|---|---|
| `README.md` | 공식 소개 · 퀵스타트 · 레이어 · 키 |
| `DATA_SOURCES.md` | **데이터 출처·라이선스 전수 (상업화 전 필독)** |
| `SECURITY.md` | 위협 모델 · 키 제한 · LAN 공유 규칙 |
| `docs/CURRENT-STATE.md` | 런타임 레퍼런스 (권위 문서) |
| `docs/KNOWN-ISSUES.md` | 알려진 이슈 및 우회 방법 |
| `docs/PERFORMANCE.md` | 성능 측정 기준 |
| `TESTING.md` | 테스트 실행 방법 |
| `CONTRIBUTING.md` | 기여 가이드 |
| `.env.example` | 환경변수 전체 목록 |
