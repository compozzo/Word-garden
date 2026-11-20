# Word Garden · 단어 정원

A small, friendly word-study game for phones and tablets. One self-contained HTML file — no build step, no external assets, no tracking.

Open `index.html` (or the GitHub Pages URL) on a phone or tablet.

## What's inside

**Learn Stage** — a flashcard per word. The front shows the word and a pronunciation button (browser speech synthesis, `en-US`). Tap the card to flip it: the back shows 2–3 situation pictures that explain the meaning, then the English definition, and a `한글 뜻 보기` button that reveals the Korean meaning.

**Test Stage** — 9 cards (3 words × Spell / Picture / Meaning) face down in a 3×3 grid. Flip as many cards as you like, then tap three of the open cards to try a match. Three cards of the same word score +100 and a fruit sticker; a miss only clears the selection and leaves the cards open, so the child can pick again. Ten words are tested in rounds of three, with a `랜덤 3개` option.

**Sticker book** — collected fruits and total score are kept in `localStorage` on the device.

**단어 팩 고르기** — the Home screen has a horizontal strip of word packs. The pick is kept in `localStorage` (`wg_pack_v1`), and the hero chips, Learn Stage and Test Stage all follow the picked pack. Words added with 단어 관리 ride along in whichever pack is picked.

**단어 관리** — a parent can add a word (English, Korean meaning, English definition, part of speech, and a picture emoji). Words added there are stored on the device only; for illustrated multi-scene pictures, update the word list in the file.

## The word packs

`📸 오늘의 단어장` is the environment unit (Reduce ~ Waste). The six grade packs follow the 2022 revised curriculum elementary vocabulary, ten words each: 초1 기초 (apple ~ happy), 초2 일상 (run ~ school), 초3 필수 (animal ~ smile), 초4 필수 (arrive ~ weather), 초5 심화 (agree ~ wonder), 초6 심화 (achieve ~ responsible). English starts in grade 3 in the curriculum, so the 초1·초2 packs draw their words from the foundational, everyday part of that list.

Every word carries two inline SVG scenes, built from a shared set of flat-shape pieces (`artBg`, the `G` glyph library and `artScene`) so all 70 words stay crisp at phone size without external files.

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
