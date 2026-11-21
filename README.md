# Word Garden · 단어 정원

A small, friendly word-study game for phones and tablets. One self-contained HTML file — no build step, no external assets, no tracking.

Open `index.html` (or the GitHub Pages URL) on a phone or tablet.

## What's inside

**Learn Stage** — a flashcard per word. The front shows the word and a pronunciation button (browser speech synthesis, `en-US`). Tap the card to flip it: the back shows 2–3 situation pictures that explain the meaning, then the English definition, and a `한글 뜻 보기` button that reveals the Korean meaning together with the picture's Korean caption underneath it.

**Test Stage** — 9 cards (3 words × Spell / Picture / Meaning) face down in a 3×3 grid. Flip as many cards as you like, then tap three of the open cards to try a match. Three cards of the same word score +100 and a fruit sticker; a miss only clears the selection and leaves the cards open, so the child can pick again. Ten words are tested in rounds of three, with a `랜덤 3개` option.

**Sticker book** — collected fruits and total score are kept in `localStorage` on the device.

**두 단계로 고르기** — Home first shows the six big categories (숙제단어 · 초등단어 · 중등단어 · 고등단어 · 토익단어 · 여행단어) as a 3×2 grid, then the packs inside the picked category as a horizontal strip. All six categories now hold packs. The category is kept in `localStorage` (`wg_cat_v1`) and the pack in `wg_pack_v1`; the hero chips, Learn Stage and Test Stage follow the picked pack. Words added with 단어 관리 ride along in whichever pack is picked. The empty-category path is still in the file for a future category: a category whose `packs` list is empty shows a "준비 중" notice and greys out Learn/Test instead of opening an empty round. Devices that saved an old pack key still work: `today`/`photo` map to `261120` and `nov23` to `261123`.

**단어 관리** — a parent can add a word (English, Korean meaning, English definition, part of speech, and a picture emoji). Words added there are stored on the device only; for illustrated multi-scene pictures, update the word list in the file.

## The word packs

`📝 숙제단어` holds the homework spelling lists, named by test date:

- `261126` — 7-3 Spelling List, test date November 26th, 2026 (12 words: ugliness ~ litter), the default pack on a fresh device. Twelve words divide into four clean rounds in Test Stage (1–3, 4–6, 7–9, 10–12).
- `261123` — the Jupiter Class November quiz (11월 23일 테스트).
- `261120` — the environment unit (오늘의 단어장).

`📝 261123` is the Jupiter Class November theme quiz (test date 11월 23일): Volcano, Magma, Lava, Ozone, Evaporation, Pollution, Oxygen, Conserve, Carbon Dioxide, Greenhouse Gas, in sheet order. It sits first in the pack strip and is the pack a device shows until another one is picked. Its twenty pictures are drawn by their own set of helpers (`sciSvg`, `sciBack`, `sciSmoke`, `sciSub`, `sciMol`, `sciTag`, plus the `SCI` scene table) because the science words that look alike on paper — magma/lava, ozone/oxygen, carbon dioxide/greenhouse gas — need two clearly different situations each. Gas formulas (`O₃`, `O₂`, `CO₂`) are drawn as separate `text` runs with an explicit subscript, not as a translatable word.

`📸 오늘의 단어장` is the environment unit (Reduce ~ Waste). The six grade packs follow the 2022 revised curriculum elementary vocabulary, ten words each: 초1 기초 (apple ~ happy), 초2 일상 (run ~ school), 초3 필수 (animal ~ smile), 초4 필수 (arrive ~ weather), 초5 심화 (agree ~ wonder), 초6 심화 (achieve ~ responsible). English starts in grade 3 in the curriculum, so the 초1·초2 packs draw their words from the foundational, everyday part of that list.

`📘 중등단어` (중1·중2·중3) and `🎓 고등단어` (고1·고2·고3) add thirty words each, ten per grade, drawn from the 2022 revised English curriculum vocabulary range for middle and high school and ordered by difficulty (adventure ~ prepare, achieve ~ honest, analyze ~ perspective, anticipate ~ verify, ambiguous ~ prevalent, alleviate ~ skeptical). Each grade pack is labelled with its short name in the strip (`중1`) and its theme in the hero (`중1 필수`).

`💼 토익단어` holds four ten-word tiers, ordered by the score band where a learner meets the words (TOEIC Service List / Business Service List frequency bands, plus the score-band word books Korean learners use):

- `베이직` — office and daily business basics: schedule ~ request.
- `600` — hiring, scheduling and communication: applicant ~ procedure.
- `750` — contracts, billing, shipping and performance: contract ~ coordinate.
- `850` — finance, legal, compliance and abstract business verbs: compliance ~ delegate.

`✈️ 여행단어` holds six ten-word situations, each one a trip the child can walk through:

- `공항` — passport ~ carry-on.
- `교통` — subway ~ destination.
- `숙소` — reservation ~ luggage.
- `식당` — menu ~ refill.
- `쇼핑` — receipt ~ bargain.
- `응급` — pharmacy ~ police.

Every word carries two inline SVG scenes, built from a shared set of flat-shape pieces (`artBg`, the `G` glyph library and `artScene`) so all 252 words stay crisp at phone size without external files. The business and travel pictures add their own pieces (`BIZ_KIT`: desk, laptop, contract, stamp, handshake, briefcase, truck, cargo ship, warehouse, gavel … and `TRAVEL_KIT`: passport, boarding pass, train, turnstile, key card, menu, receipt, curtain, pharmacy sign, embassy …), merged into `G` with `Object.assign` so one art description can name any piece.

## The environment unit (📸 오늘의 단어장)

The environment unit, in order:

1. Reduce — To use a lesser amount of an item
2. Reuse — To use an item again
3. Recycle — Altering materials so they can be used in a different form
4. Natural Resource — Something we use that comes from the Earth
5. Protect — To keep safe and not harm
6. Harmful — Something that causes harm
7. Less — The opposite of more
8. Light — Not heavy
9. Environment — The area in which something lives
10. Waste — To use carelessly or too much

Every word carries two hand-drawn inline SVG scenes.
## Notes

- Layout targets 360–430 px widths with no horizontal scrolling and large touch targets.
- All state is local to the device; clearing site data resets score and stickers.
- The Korean UI text is for the parent and the child; the English definitions are the study material.
- **The English is never translated.** A phone browser with auto-translate on used to turn the English definitions into Korean on its own, which gave away the answer before the 한 / 한글 뜻 보기 button was tapped. The page now carries `<meta name="google" content="notranslate">`, `translate="no"` on `<html>`, and `translate="no"` + `.notranslate` on every English text node (`#learnWord`, `#learnPos`, `#learnEn`, `.tl-spell`, `.tl-meaning`, the Home word chips, and the word/sticker lists). `markEnglish()` re-applies those attributes after every render, so new English output must either use one of those classes or be added to `ENGLISH_SELECTORS`.
- A word with no English definition never falls back to its Korean meaning on a 뜻 card; the card shows `한 버튼을 눌러 뜻을 봐요` and the Korean stays behind the 한 button.
- `tl-ko` and `#learnKo` are hidden with the `hidden` attribute, `display:none !important` in CSS, and a state flag (`koOpen` / `showKo`) that resets on every new round, pack change and shuffle.
- `#sceneCap` is Korean too (e.g. `다 쓴 유리병을 연필꽂이로 다시 써요`), so it lives under `#learnKo` in `.ko-row` and follows the same `showKo` flag: before `한글 뜻 보기` is pressed the back shows only the picture, the English definition and the button. Empty captions stay hidden even after the button is pressed.
