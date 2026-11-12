# 그림 자산 — 열두달

디자인 재건(기준 문서 D50, `docs/디자인-재건.md` 2-2)의 그림 한 벌. 모두 코드로 다시 만들 수 있다.

- 만드는 도구: `node tools/art-covers.mjs`(첫 화면·표지·공유 표지·`manifest.json`), `node tools/art-marks.mjs`(문양 SVG·`marks/list.json`). 원본 사진과 글꼴은 `research/사진원본/`(git 무시) — 없으면 `node tools/art-covers.mjs --fetch` 가 출처 주소에서 받는다(Unsplash 원본 받기가 막히면 브라우저로 받아 같은 자리에 둔다).
- 숫자 근거: `docs/수치-근거.md` N-130~N-136(설계), N-230(실측 크기). 사진 출처: `research/사진-출처.csv` 의 `art/*.jpg` 줄.
- 대조판(배포에 들어가지 않음): `research/측정/그림/card-<id>.jpg`(공유 표지 14장), `research/측정/그림/marks.png`(문양 41개).
- 공통 색: 밤 `#0E1530` · 밤2 `#1B2552` · 금 `#C9A45C` · 밝은 금 `#E6CF96` · 한지 `#F6F1E7` · 먹 `#1F1D1A` · 인주 `#B5432F`.
- 생성형 AI 그림, 일반인 얼굴 스톡 사진은 쓰지 않는다.

## 목록 파일 `manifest.json`

```json
{
  "hero":   { "alt", "credit", "base": "hero/hero", "variants": { "4x5": {"widths":[640,828,1080],"lqip":"data:…"}, "16x9": {…}, "9x16": {…} }, "dominant": "#0E1530" },
  "heroes": { "moon": {…}, "palace": {…}, "sunrise": {…} },
  "covers": { "<상품 id>": { "alt", "credit", "base": "covers/<id>", "variants": { "4x5": {"widths":[360,540,720]}, "16x9": {"widths":[720,1200]} },
                             "share": { "card": "share/<id>-4x5-1080.jpg", "og": "share/<id>-1200x630.jpg" } } },
  "marks":  { "tti": […12], "element": […5], "month": ["01"…"12"], "ornament": […], "path": {…} }
}
```

파일 경로 = `public/art/<base>-<variant>-<width>.<avif|webp|jpg>` (예: `hero/hero-4x5-640.avif`, `covers/love-16x9-1200.webp`). `<picture>` 에 AVIF → WebP → JPEG 순서로 `srcset` 을 건다.

## 첫 화면 큰 장면(3벌, 한 색감)

| 이름 | 장면 | 비율·폭 | 출처 |
|---|---|---|---|
| `moon`(기본 = `hero`) | 눈 덮인 산맥 위 보름달 | 4:5 640·828·1080 / 16:9 1080·1600 / 9:16 720·1080 | YunFengQ / Unsplash |
| `palace` | 해 질 녘 경복궁 광화문과 북악산 | 같음 | Jin-Woo Lee / Unsplash |
| `sunrise` | 성산일출봉 뒤로 밝아 오는 바다 | 같음 | Sung Shin / Unsplash |

아래 45%부터 밤색으로 떨어져 85% 지점에서 거의 불투명(0.96)하다 — 글자는 이 띠 위에 얹는다(사진 위에 바로 얹지 않는다).

## 상품 표지(14종)

화면용(`covers/`)에는 글자·테두리를 굽지 않는다. 상품 이름·값·금테는 화면이 HTML/CSS 로 얹는다. 아래 62%→94% 가 밤색 띠다.
공유용(`share/`)은 HTML 을 얹을 수 없는 곳(메신저 미리보기 `og:image`, 내려받는 카드)을 위해 금테·모서리 무늬·`열두달`·상품 이름(마루 부리 Bold)·인장(한자 두 글자)을 합성했다. 도구가 글자 자리 대비를 재서 4.5:1 미만이면 실패로 끝난다.

| 상품 id | 사진 | 인장 | 출처 |
|---|---|---|---|
| ny-core | 한라산 구름바다 위 해돋이 | 新年 | 제주영상문화산업진흥원, 공공누리 제1유형 — **출처 표시 의무** |
| ny-full | 노을 진 눈산 위 보름달 | 新年 | Mateusz Klein / Unsplash |
| ny-pair | 눈밭의 두루미 한 쌍 | 同行 | paul milley / Unsplash |
| life | 산 위 은하수 | 平生 | Nathan Anderson / Unsplash |
| match | 원앙 한 쌍 | 宮合 | David Clode / Unsplash |
| love | 붉은 동백 한 송이 | 戀情 | Annie Spratt / Unsplash |
| money | 붉은 술에 매달린 금빛 엽전꼴 장식 | 財物 | Avishkar Darjee / Unsplash |
| career | 눈산으로 뻗은 길 | 職業 | Mads Schmidt Rasmussen / Unsplash |
| child | 흙에서 돋은 새싹 | 子女 | KC Shum / Unsplash |
| bundle-life-ny | 보름달 아래 불 밝힌 누각(경주 월정교) | 合本 | Seongho Jang / Unsplash |
| tojeong | 옛 글씨 종이 위 붓과 붓받침 | 土亭 | 五玄土 ORIENTO / Unsplash |
| tarot-year | 구름 사이 보름달 | 月相 | Aron Visuals / Unsplash |
| tarot-love | 두 손으로 감싼 등잔불 | 戀心 | Prateek Gautam / Unsplash |
| name | 붓으로 한자를 쓰는 손 | 作名 | Niketh Vellanki / Unsplash |

상품 이름은 `src/content/products.ts` 에서 읽는다 — 이름을 바꾸면 도구를 다시 돌린다. 상품이 늘면 `tools/art-covers.mjs` 의 PHOTOS·SEAL·KICKER 에 줄을 더한다(빠지면 도구가 실패로 끝난다).

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
