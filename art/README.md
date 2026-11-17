# 그림 자산 — 열두달

조선 회화·천문도로 다시 지은 그림 한 벌(기준 문서 D53·D55, 앞판 D50 의 Unsplash 사진은 걷었다). 모두 코드로 다시 만들 수 있다.

- 만드는 도구(이 순서로): `node tools/art-classic.mjs`(서사 그림 `classic/`·목록의 `classic` 칸) → `node tools/art-covers.mjs`(표지·공유 표지·첫 화면 공유 미리보기·목록의 나머지 칸) · `node tools/art-marks.mjs`(문양 SVG·`marks/list.json`).
  원본 그림은 `research/사진원본/joseon/`, 글꼴은 `research/사진원본/fonts/`(둘 다 git 무시) — 없으면 `--fetch` 를 붙이면 위키미디어 공용 API 로 받는다. 받을 때 공용의 조건(LicenseShortName)을 다시 읽어 공개 영역·CC0 가 아니면 멈춘다. 공용이 원본 파일을 이어 받는 것을 429 로 막아 표준 축소판 폭(3840·1920·1280)으로 받는다(문자도 병풍만 12000).
- 원천 규칙: 위키미디어 공용의 공개 영역(평면 회화의 충실한 복제, PD-Art)·CC0 만. 캐릭터·AI 그림·Unsplash 사진은 쓰지 않는다. 그림마다 출처는 `research/사진-출처.csv` 의 `joseon/` 줄(원문 주소·작가·조건·확인 날짜).
- 숫자 근거: `docs/수치-근거.md` N-130~N-136(앞판 설계 — 띠·폭·품질), N-140~N-149(이번 판 — 크기 상한·곡선·색 통일·품질).
- 대조판(배포에 들어가지 않음): `research/측정/그림/대조-표지-전후.jpg`(공유 표지 14종 이전↔이번), `card-<id>.jpg`(공유 표지), `classic-<key>.jpg`(원본 자리↔가공 결과), `share-home.jpg`, 크기는 `크기.json`·`크기-classic.json`.
- 공통 색(D53): 먹 `#0E0D0B` · 먹2 `#17150F` · 금 `#C9A45C` · 밝은 금 `#E3C88E` · 한지 `#F4EEE2` · 인주 `#B5432F`(공유 카드 인장).

## 목록 파일 `manifest.json`

```json
{
  "covers":  { "<상품 id>": { "alt", "credit", "source", "base": "covers/<id>", "variants": { "4x5": {"widths":[360,540,720],"lqip":"data:…"}, "16x9": {"widths":[720,1200],"lqip":"…"} },
                              "share": { "card": "share/<id>-4x5-1080.jpg", "og": "share/<id>-1200x630.jpg" } } },
  "classic": { "<key>": { "alt", "credit", "source"(공용 원문 주소), "license", "base": "classic/<key>", "kind": "line"|"color", "ratio": "1x1"|"3x1"|"16x9", "widths": […], "lqip" } },
  "share":   { "home": { "og": "share/home-1200x630.jpg", "alt" } },
  "marks":   { "tti": […12], "element": […5], "month": ["01"…"12"], "ornament": […], "path": {…} }
}
```

`hero`·`heroes` 칸(첫 화면 Unsplash 사진 셋)은 없앴다 — 첫 화면은 `classic` 그림과 코드로 그린 금빛 선으로 짓고, 공유 미리보기는 `share.home.og` 를 쓴다.
파일 경로: 표지 `public/art/<base>-<비율>-<폭>.<avif|webp|jpg>`(예: `covers/love-16x9-1200.webp`), 옛 그림 `public/art/<base>-<폭>.<avif|webp|jpg>`(예: `classic/starmap-1600.avif`). `<picture>` 에 AVIF → WebP → JPEG 순서로 `srcset` 을 건다.

## 옛 그림 `classic/`

| key | 그림 | kind · 비율 · 폭 | AVIF(가장 큰 폭) |
|---|---|---|---|
| `starmap` | 「천상열차분야지도」 인본 — 원형 성도만 정사각으로, 원 밖은 지움 | line · 1x1 · 900·1600 | 49KB(상한 60KB, N-140) |
| `mountains` | 정선 「인왕제색도」 — 위아래를 먹으로 녹인 띠 | line · 3x1 · 1200·2000 | 21KB |
| `irworobong` | 「일월오봉도」 전체 | color · 16x9 · 720·1200 | 120KB |
| `irworobong-sq` | 「일월오봉도」 가운데 봉우리 | color · 1x1 · 360·720 | 78KB |
| `seodang` | 김홍도 「서당」 | color · 1x1 · 360·720 | 23KB |
| `daejanggan` | 김홍도 「대장간」 | color · 1x1 · 360·720 | 31KB |
| `wolha` | 신윤복 「월하정인」(달과 두 사람) | color · 1x1 · 360·720 | 19KB |

- **line**: 흑백 → 뒤집기 → 곡선(뒤집힌 종이 바탕은 순흑 0, 선은 흰-회색 최대 0.86) → 흐림 0.7. 바탕이 0 이라 화면에서 `mix-blend-mode: screen` 으로 겹치면 선만 남는다. 장식이므로 읽는 이에게는 숨긴다(`alt=""`·`aria-hidden`).
- **color**: 채도를 낮추고(일월오봉도 0.62, 풍속화 0.8) 밝은 곳을 밝은 금 쪽으로, 전체를 0.84·0.8 배로 눌러 먹빛 화면에서 종이의 흰빛이 튀지 않게 했다. 원형 액자·두루마리 판에 넣는다.

## 상품 표지(14종)

화면용(`covers/`)에는 글자·테두리를 굽지 않는다. 상품 이름·값·금테는 화면이 HTML/CSS 로 얹는다. 아래 62%→94% 가 먹 띠다.
공유용(`share/`)은 HTML 을 얹을 수 없는 곳(메신저 미리보기 `og:image`, 내려받는 카드)을 위해 금테·모서리 무늬·`열두달`·상품 이름(마루 부리 Bold)·인장(한자 두 글자)을 합성했다. 도구가 글자 자리 대비를 재서 4.5:1 미만이면 실패로 끝난다.

| 상품 id | 그림 | 인장 |
|---|---|---|
| ny-core | 「일월오봉도」 붉은 해 쪽 | 新年 |
| ny-full | 「일월오봉도」 흰 달 쪽 | 新年 |
| ny-pair | 「십장생도 병풍」(국립고궁박물관) 소나무 위를 나는 학 | 同行 |
| life | 이인문 「강산무진도」 | 平生 |
| match | 김홍도 「신행」 | 宮合 |
| love | 신윤복 「월하정인」 | 戀情 |
| money | 김홍도 「장터길」 | 財物 |
| career | 김홍도 「대장간」 | 職業 |
| child | 김홍도 「서당」 | 子女 |
| bundle-life-ny | 정선 「금강전도」 | 合本 |
| tojeong | 「책거리」(메트로폴리탄 미술관, CC0) | 土亭 |
| tarot-year | 「천상열차분야지도」 목판본(서울대 규장각) | 月相 |
| tarot-love | 신윤복 「월야밀회」 | 戀心 |
| name | 「문자도 여덟 폭 병풍」(국립민속박물관) 첫 폭 孝 | 作名 |

모두 공개 영역(조선 회화의 평면 복제)이고 `tojeong` 만 CC0 다 — 출처 표시 의무는 없지만 '사진·그림 출처' 쪽에 작가·작품·소장처를 싣는다(`credit`).
상품 이름은 `src/content/products.ts` 에서 읽는다 — 이름을 바꾸면 도구를 다시 돌린다. 상품이 늘면 `tools/art-covers.mjs` 의 PHOTOS·SEAL·KICKER 에 줄을 더한다(빠지면 도구가 실패로 끝난다).

## 첫 화면 공유 미리보기 `share/home-1200x630.jpg`

먹 바탕 + 오른쪽에 `starmap` 선 그림(세기 0.8) + 천문도의 적도·황도 원을 본뜬 금빛 궤도선 셋 + 왼쪽에 `열두달`(금빛)·`사주 · 운세 풀이`. 첫 화면의 `og:image` 로 쓴다.

## 문양 SVG(41개)

모두 `stroke="currentColor"`(선)·`fill="currentColor"`(한자·점·달의 밝은 쪽)이라 CSS `color` 로 금빛·먹빛을 바꾼다. 동물 형상은 그리지 않는다 — 십이지 방위 바퀴 + 한자 인장으로 나타낸다.

| 파일 | 무엇 | 판 |
|---|---|---|
| `marks/tti-{rat,ox,tiger,rabbit,dragon,snake,horse,sheep,monkey,rooster,dog,pig}.svg` | 띠: 두 겹 원 + 12방위 눈금 + 그 띠 방위의 마름모 + 가운데 한자(子~亥) | 64×64 |
| `marks/element-{wood,fire,earth,metal,water}.svg` | 오행: 전통 오행 형상(목 기둥·화 세모·토 네모·금 원·수 물결) 테두리 + 한자 | 64×64 |
| `marks/month-01..12.svg` | 열두 달: 방위 바퀴에서 그 달 칸을 굵은 호로 밝히고 가운데 숫자 + 月 | 64×64 |
| `ornament/corner.svg` | 모서리 장식(왼쪽 위 — 다른 모서리는 `transform: scale(-1,1)` 등으로 뒤집기) | 64×64 |
| `ornament/divider.svg` | 가는 금선 구분자(가운데 마름모) | 240×20 |
| `ornament/cloud-band.svg` | 구름무늬 띠(가로로 이어 붙는 타일, `background-repeat: repeat-x`) | 96×24 |
| `ornament/stars-tile.svg` | 별 바탕 무늬 타일(이어 붙음, 고정 씨앗이라 다시 만들어도 같음) | 200×200 |
| `ornament/moon-0..7.svg` | 달 위상 8: 0 삭 · 1 초승 · 2 상현 · 3 차오름 · 4 보름 · 5 기욺 · 6 하현 · 7 그믐 | 64×64 |

## 글꼴(그림 안 글자에만 쓰고, 화면 웹 글꼴은 따로)

| 글꼴 | 쓰는 곳 | 라이선스 | 받은 곳 |
|---|---|---|---|
| 마루 부리(MaruBuri) Bold·SemiBold·Regular | 공유 표지의 제목·`열두달`·작은 글, 열두 달 문양 숫자 | SIL Open Font License 1.1(네이버, 상업 이용·그림에 넣기 가능) | https://hangeul.naver.com/font — 파일 `hangeul.pstatic.net/hangeul_static/webfont/MaruBuri/` |
| 본명조(Noto Serif CJK KR) Black | 인장 한자, 띠·오행 문양 한자, 月 | SIL Open Font License 1.1(Adobe·Google) | https://github.com/notofonts/noto-cjk (Serif/OTF/Korean) |

글꼴 파일은 배포하지 않는다 — 글자는 그림(픽셀) 또는 SVG 경로로 바뀌어 들어간다. sharp(pango)의 글꼴 파일 지정이 Windows 판에서 먹지 않아 기본 고딕으로 대체되는 것을 확인해(2026-11-12), HarfBuzz(`harfbuzzjs`, subset-font 와 함께 설치됨)로 글자를 모양 잡아 윤곽을 그린다.
