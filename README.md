# 꼬르륵 원격 콘텐츠

`content.json` 파일 하나만 고치면 **앱 업데이트 없이** 바로 반영돼요.
(앱을 다시 열거나, 앱으로 돌아올 때 최대 10분 간격으로 새로 받아와요.)

바꿀 수 있는 것: 홈 배너 · 공지 팝업 · 쿠팡 상품 링크 · 레시피 추가/수정/숨김 · 재료 추가 · 전면광고 빈도
바꿀 수 없는 것(앱 업데이트 필요): 기능, 화면 디자인, 앱 아이콘, 권한

모든 항목을 채운 예시는 `example.json`에 있어요.

---

## 처음 한 번: GitHub Pages에 올리기

1. GitHub에서 새 저장소 만들기 → 이름 `kkoreuk-content`, **Public**
2. `content.json` 파일 업로드 (Add file → Upload files)
3. Settings → Pages → Branch를 `main` / `(root)`로 두고 Save
4. 1~2분 뒤 주소가 생겨요: `https://아이디.github.io/kkoreuk-content/content.json`
5. 이 주소를 `www/js/config.js`의 `contentUrl`에 넣고 **앱을 한 번만** 업데이트
   → 그 뒤로는 GitHub에서 `content.json`만 고치면 끝

수정할 때는 GitHub에서 `content.json` → 연필 아이콘 → 고치고 → Commit changes.
실수해도 파일의 History에서 예전 버전으로 되돌릴 수 있어요.

> 파일에 문법 오류(쉼표 빠짐 등)가 있으면 앱은 **그 파일을 무시하고 직전에 받은 내용**을 계속 써요. 앱이 멈추지는 않아요.
> 고친 뒤에는 https://jsonlint.com 에 붙여넣어 오류가 없는지 확인하면 안전해요.

---

## 항목 설명

### updatedAt
수정한 날짜. 설정 화면 버전 옆에 표시돼요. 고칠 때마다 바꿔주세요.

### banners (홈 상단 배너)
맨 앞에 순서대로 들어가요. 기본 배너(오늘의 한 끼 등)는 그 뒤에 이어져요.

| 항목 | 설명 |
|---|---|
| id | 영문 고유 이름 (필수) |
| title | 큰 글씨 (필수) |
| go | 누르면 갈 화면 (필수) — 아래 "화면 주소" 참고 |
| tag | 작은 라벨 (예: 이벤트) |
| sub | 설명 한두 줄 |
| emoji | 오른쪽 그림 (이모지 하나) |
| color | 배경색 (예: #FFE7B8) |
| dark | true면 검정 배너 |
| start / end | 보여줄 기간 (예: 2026-12-20). 비우면 항상 |
| night | true = 밤(악마 모드)에만, false = 낮에만, 비우면 항상 |

### notice (공지 팝업)
홈에서 **사람마다 한 번만** 떠요. 새 공지를 띄우려면 `id`를 바꾸세요. 없애려면 `null`.

### shopLinks (쿠팡 상품 링크)
`"재료이름": { "url": "쿠팡 파트너스 링크", "label": "버튼에 보일 상품명" }`
재료 이름은 앱에 있는 이름과 똑같아야 해요 (예: 불닭볶음면, 스팸, 참치캔, 슬라이스치즈).
등록한 재료에만 쿠팡 버튼과 대가성 문구가 떠요.
앱에 기본으로 들어간 링크를 없애려면 `"재료이름": null`.

### recipes (레시피)
- **새 레시피 추가**: `example.json`의 `ramen-fried-rice`처럼 모든 항목을 채워요.
- **기존 레시피 고치기**: `{ "id": "marks-set", "cost": 8900 }`처럼 id와 바꿀 항목만.
- **숨기기**: `{ "id": "affogato", "remove": true }`
- **봉인 레시피로 추가**: `"hidden": true`를 넣으면 광고 보고 여는 레시피가 돼요.

| 항목 | 설명 |
|---|---|
| th | 테마: beggar(거지) devil(악마) cvs(편의점) micro(전자레인지) fast(10분) late(야식) hangover(해장) |
| cost / kcal | 1인분 대략 재료비(원) / 칼로리 |
| min / lv | 조리시간(분) / 난이도 1~3 |
| devil | 악마지수 0~5 |
| serves | 분량 (기본 1인분) |
| ing | `["재료", "분량"]`, 선택 재료는 `["재료", "분량", 1]` |
| steps | 문장, 타이머가 필요하면 `["문장", 초]` |

재료 이름을 앱에 있는 이름으로 써야 냉장고 매칭이 돼요.
새 재료가 필요하면 `ingredients`에 추가하세요 (cat: egg / meat / veg / carb / cvs / pantry).

이모지는 앱에 없는 것도 인터넷에서 자동으로 받아와요.

### settings
| 항목 | 설명 |
|---|---|
| interstitialEvery | 레시피 몇 번 열 때마다 전면광고 1번 (최소 2) |
| interstitialCooldown | 전면광고 최소 간격(초, 최소 60) |

---

## 화면 주소 (go에 쓰는 값)
| 주소 | 화면 |
|---|---|
| `#/r/레시피id` | 레시피 상세 |
| `#/theme/devil` | 테마 (beggar, devil, cvs, micro, fast, late, hangover) |
| `#/sealed` | 봉인된 레시피 |
| `#/pick` | 메뉴 뽑기 |
| `#/fridge` | 냉장고 |
| `#/search?q=라면` | 검색 결과 |
