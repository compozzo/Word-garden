# Word Garden · 단어 정원

A small, friendly word-study game for phones and tablets. One self-contained HTML file — no build step, no external assets, no tracking.

Open `index.html` (or the GitHub Pages URL) on a phone or tablet.

## What's inside

**Learn Stage** — a flashcard per word. The front shows the word and a pronunciation button (browser speech synthesis, `en-US`). Tap the card to flip it: the back shows 2–3 situation pictures that explain the meaning, then the English definition, and a `한글 뜻 보기` button that reveals the Korean meaning together with the picture's Korean caption underneath it.

**Test Stage** — 9 cards (3 words × Spell / Picture / Meaning) face down in a 3×3 grid. Flip as many cards as you like, then tap three of the open cards to try a match. Three cards of the same word score +100 and a fruit sticker; a miss only clears the selection and leaves the cards open, so the child can pick again. Ten words are tested in rounds of three, with a `랜덤 3개` option.

**Sticker book** — collected fruits and total score are kept in `localStorage` on the device.

## 🎮 놀이 마당 — four games

Below the two stages, Home carries a 2×2 yard of games so every mode is one tap away on a 390 px phone. They all work on the picked pack (and its Day) plus the parent's own words.

**🔁 자동 복습미션** — spaced repetition. Every answer in this app updates `wg_srs_v1` (`{ wordId: {b, ok, miss, due, last} }`). A correct answer moves the word up one box and schedules it 1 → 3 → 7 → 21 days out; a miss drops it back to box 0 and brings it back in ten minutes, so it lands in the same day's mission. A session is the due words first (oldest due first), then a starter mix of words with no history yet, six words for a normal pack and eight for a big one. The play itself is a retrieval round: the picture and the Korean meaning are shown, the learner types the English, and `정답 보기` falls back to a self-check (알았어요 / 몰랐어요) when the word will not come.

**📖 이야기 빈칸** — a one- or two-sentence English story with the target word blanked out, toggled between 객관식 (4지선다) and 주관식 (직접 입력) inside the mode. `STORY_BANK` holds hand-written sentences (and a Korean topic hint) for the three homework packs, the elementary Day 1 words of all six grades, the first Day of 토익 베이직 and 600, and the 중등·고등·750·850 packs. Any other word falls back to a sentence built from its part of speech, its English definition and its picture, with the definition dropped when it would give the answer away. A wrong answer never locks the card: the blank fills with the answer and the word goes back into the review queue.

**🎨 그림으로 말해요** — two sub-tabs. 📱 같은 기기 2인 passes the phone: player 1 peeks at the secret word (`단어 보기` hides itself again after four seconds), draws on the canvas (eight colours, four nibs, eraser, undo, clear), then hands over; player 2 guesses by typing or from four choices and the score alternates each round. 🌐 온라인 방 puts two phones in one room over WebRTC: the host taps 방 만들기 and gets a four-digit code, the guest types it, and strokes stream over the PeerJS data channel as they are drawn (each point is sent in a 70 ms batch, and undo/clear send a full snapshot). The drawer judges the guess and sends the result and the score; `다음 라운드` hands the pen to the other side. PeerJS is fetched from unpkg, with jsDelivr as the second source, and connection failures (no such room, blocked network, no WebRTC) are reported in Korean in the status line instead of failing silently.

**🕵️ 단어 추리** — the twenty-questions hangman the parent described. The presenter (the game, or a friend in 2인 대결) says `저는 과일입니다. 5개의 알파벳으로 되어 있어요. 기회는 20번 주어집니다.` and the guesser taps letters or types the whole word. Answers come back as a chat log: `a — 1개 있습니다!`, `e는 없어요 (남은 기회 19번)`. Twenty chances, a wrong letter costs one, and 그림 힌트 / 뜻 힌트 are there when the child is stuck. Only letters are shown as blanks, so a two-word answer keeps its space.

**🎚️ 난이도·범위** — 📖 이야기 빈칸 and 🕵️ 단어 추리 (자동 출제) each carry a pill bar of their own under the mode switch: `현재 팩`, `초등 전체 (816)`, `중등 이하 (838)`, `고등 이하 (868)`, `토익 600 이하 (300)`, `전체 (1174)`. `현재 팩` keeps the old behaviour exactly — the pack (or its Day) picked on Home, plus the parent's own words — and is the default, so a device that never touches the bar plays as before. `초등 전체` is 초1~초6, `중등 이하` adds 중1~중3, `고등 이하` adds 고1~고3, `토익 600 이하` is 베이직 + 600 (not 750/850), and `전체` is every pack plus the parent's words. The counts de-duplicate by spelled word, so a word that two packs share (`desk` in 초1 and 베이직, `delay` in 600 and 여행) is asked once. A scope whose file (`data/elem.json`, `data/toeic.json`) has not arrived yet writes `단어를 불러오는 중…` into the card, awaits the same fetch the rest of the page uses, and starts the round from that pool the moment the file lands. Each game remembers its pick in `localStorage` (`wg_story_scope_v1`, `wg_hm_scope_v1`) and names the scope and word count in its subtitle (`초등 전체 (816단어) · 이야기 속 빈칸`, `자동 출제 · 토익 600 이하 (300단어)`). A story card's three distractors come from the same scope, same part of speech first, never the answer and never the same word twice; a hangman question is a word of the scope with three letters or more. 2인 대결 hides the bar, because there the presenter types the word.

**두 단계로 고르기** — Home first shows the six big categories (숙제단어 · 초등단어 · 중등단어 · 고등단어 · 토익단어 · 여행단어) as a 3×2 grid, then the packs inside the picked category as a horizontal strip. All six categories now hold packs. The category is kept in `localStorage` (`wg_cat_v1`) and the pack in `wg_pack_v1`; the hero chips, Learn Stage and Test Stage follow the picked pack. Words added with 단어 관리 ride along in whichever pack is picked. The empty-category path is still in the file for a future category: a category whose `packs` list is empty shows a "준비 중" notice and greys out Learn/Test instead of opening an empty round. Devices that saved an old pack key still work: `today`/`photo` map to `261120` and `nov23` to `261123`.

**단어 관리** — a parent can add a word (English, Korean meaning, English definition, part of speech, and a picture emoji). Words added there are stored on the device only; for illustrated multi-scene pictures, update the word list in the file.

## The word packs

`📝 숙제단어` holds the homework spelling lists, named by test date:

- `261126` — 7-3 Spelling List, test date November 26th, 2026 (12 words: ugliness ~ litter), the default pack on a fresh device. Twelve words divide into four clean rounds in Test Stage (1–3, 4–6, 7–9, 10–12).
- `261123` — the Jupiter Class November quiz (11월 23일 테스트).
- `261120` — the environment unit (오늘의 단어장).

`📝 261123` is the Jupiter Class November theme quiz (test date 11월 23일): Volcano, Magma, Lava, Ozone, Evaporation, Pollution, Oxygen, Conserve, Carbon Dioxide, Greenhouse Gas, in sheet order. It sits first in the pack strip and is the pack a device shows until another one is picked. Its twenty pictures are drawn by their own set of helpers (`sciSvg`, `sciBack`, `sciSmoke`, `sciSub`, `sciMol`, `sciTag`, plus the `SCI` scene table) because the science words that look alike on paper — magma/lava, ozone/oxygen, carbon dioxide/greenhouse gas — need two clearly different situations each. Gas formulas (`O₃`, `O₂`, `CO₂`) are drawn as separate `text` runs with an explicit subscript, not as a translatable word.

`📸 오늘의 단어장` is the environment unit (Reduce ~ Waste).

`🌱 초등단어` now carries the whole 교육부 elementary vocabulary list: **800 words** from 교육부 고시 제2022-33호 [별책 14] 영어과 교육과정 [별표 3] 어휘 — the asterisked (`*`) 초등학교 권장 어휘 (the list also prints 1,200 `**` middle/high words and 1,000 unmarked ones; only the 800 `*` words are used here). The six grades split it up by difficulty: 초1·초2 hold the concrete everyday words, 초3·초4 the school, town, sports and health words, 초5·초6 the abstract nouns, school subjects and function words (above, because, however, myself …). Sixteen words that the packs already had but that are not on the 800 list (share, healthy, compare, practice, useful, wonder, achieve, balance, challenge, curious, describe, express, improve, respect, responsible, explain) are kept as they were, so the category holds 816 words.

Every grade keeps its original ten hand-drawn words as **Day 1** (apple ~ happy, run ~ school, animal ~ smile, arrive ~ weather, agree ~ wonder, achieve ~ responsible), so the pictures that were already there and the stickers children collected still match. The rest follows in Days of twelve words: 초1–초4 have 11 Days (132 words each), 초5–초6 have 12 Days (144 words each). A Day row under the pack strip picks the twelve words that Learn Stage and Test Stage use, so a Test round stays a clean 3 × 4 words instead of 48 chips. Packs of 15 words or fewer (숙제단어, 중등단어, 고등단어, 토익 750·850, 여행단어) have no Day row and behave exactly as before.

Because 816 words would bloat a single file, the elementary packs live in `data/elem.json` (about 350 KB) and are fetched once, in the background, the first time the page opens; `index.html` keeps the homework pack inline so `261126` still renders instantly. The Day is remembered per pack in `localStorage` (`wg_day_v1`).

The two big 토익 packs follow the same route: `data/toeic.json` (about 135 KB) holds 베이직 and 600 and is fetched in the background beside `elem.json`. The 💼 토익단어 strip shows both pack names from the first paint; each pack's words arrive a moment later, and a device that had already picked 베이직 or 600 keeps its pack and its Day. Until the file answers, Learn and Test answer with a "불러오는 중" toast instead of opening an empty round, the same way 초등단어 does.

`📘 중등단어` (중1·중2·중3) and `🎓 고등단어` (고1·고2·고3) add thirty words each, ten per grade, drawn from the 2022 revised English curriculum vocabulary range for middle and high school and ordered by difficulty (adventure ~ prepare, achieve ~ honest, analyze ~ perspective, anticipate ~ verify, ambiguous ~ prevalent, alleviate ~ skeptical). Each grade pack is labelled with its short name in the strip (`중1`) and its theme in the hero (`중1 필수`).

`💼 토익단어` holds four tiers, ordered by the score band where a learner meets the words (TOEIC Service List / Business Service List frequency bands, plus the score-band word books Korean learners use):

- `베이직` — 120 words in 10 Days: office basics, time and scheduling, money and price, the people and the verbs of a working day, the things in an office, simple describing words, and the first verbs a learner reads in a TOEIC sentence. Day 1 keeps the ten pictures the pack has always had (schedule ~ request) and adds `desk` and `printer`.
- `600` — 180 words in 15 Days, one Part 1–7 theme a Day: hiring and HR, meetings, travel and reservations, customer service, orders and shipping, announcements, workplace rules, money at work, papers and mail, appointments, problems, reports and numbers, the building, and conferences. Day 1 keeps `applicant` ~ `procedure` and adds `position` and `salary`.
- `750` — ten words, contracts, billing, shipping and performance: contract ~ coordinate.
- `850` — ten words, finance, legal, compliance and abstract business verbs: compliance ~ delegate.

Words were picked from the TSL/BSL frequency bands and the 600-level word books Korean learners use, then spread across the themes above; no word appears in both 베이직 and 600, and none of the 750 or 850 words repeats in the two big packs. Eight words (bill, exchange, refund, warranty, departure, arrival, delay, fare) also appear in ✈️ 여행단어, where the travel sense is what the picture shows.

`✈️ 여행단어` holds six ten-word situations, each one a trip the child can walk through:

- `공항` — passport ~ carry-on.
- `교통` — subway ~ destination.
- `숙소` — reservation ~ luggage.
- `식당` — menu ~ refill.
- `쇼핑` — receipt ~ bargain.
- `응급` — pharmacy ~ police.

Every word carries two inline SVG scenes, built from a shared set of flat-shape pieces (`artBg`, the `G` glyph library and `artScene`) so all 1,068 words stay crisp at phone size without external files. The elementary pictures add their own family of builders (`ELEM_KIT`): one `critter` draws twenty-one four-legged animals from a parameter table, and `birdie`, `seabuddy`, `buggy`, `lizard`, `food2`, `wear`, `furni`, `bod`, `place2`, `ride`, `sportkit`, `music2`, `worker`, `pose`, `where2`, `nature2`, `tool2` and `shape` do the same for their own families. Inside one Day no two words may share a picture, otherwise the three-row picture match could not be solved, so the build checks that every Day's twelve pictures (both scenes) are distinct. The business and travel pictures add their own pieces (`BIZ_KIT`: desk, laptop, contract, stamp, handshake, briefcase, truck, cargo ship, warehouse, gavel … and `TRAVEL_KIT`: passport, boarding pass, train, turnstile, key card, menu, receipt, curtain, pharmacy sign, embassy …), merged into `G` with `Object.assign` so one art description can name any piece.

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
- Three checks sit in the unpublished `build/` folder: `check_games.py` boots the page headless (quickjs) and walks the four games, `check_scope.py` boots it with both word files and walks the six scopes (pool membership, subtitle, four distinct choices, a solvable hangman question, the saved key, 2인 대결), and `verify_scope.py` runs the 390×844 phone pass against any URL (local or the Pages site) and writes `story-scope-390.png` and `hangman-scope-390.png`.
- All state is local to the device; clearing site data resets score and stickers.
- The Korean UI text is for the parent and the child; the English definitions are the study material.
- **The English is never translated.** A phone browser with auto-translate on used to turn the English definitions into Korean on its own, which gave away the answer before the 한 / 한글 뜻 보기 button was tapped. The page now carries `<meta name="google" content="notranslate">`, `translate="no"` on `<html>`, and `translate="no"` + `.notranslate` on every English text node (`#learnWord`, `#learnPos`, `#learnEn`, `.tl-spell`, `.tl-meaning`, the Home word chips, and the word/sticker lists). `markEnglish()` re-applies those attributes after every render, so new English output must either use one of those classes or be added to `ENGLISH_SELECTORS`.
- A word with no English definition never falls back to its Korean meaning on a 뜻 card; the card shows `한 버튼을 눌러 뜻을 봐요` and the Korean stays behind the 한 button.
- `tl-ko` and `#learnKo` are hidden with the `hidden` attribute, `display:none !important` in CSS, and a state flag (`koOpen` / `showKo`) that resets on every new round, pack change and shuffle.
- `#sceneCap` is Korean too (e.g. `다 쓴 유리병을 연필꽂이로 다시 써요`), so it lives under `#learnKo` in `.ko-row` and follows the same `showKo` flag: before `한글 뜻 보기` is pressed the back shows only the picture, the English definition and the button. Empty captions stay hidden even after the button is pressed.


## Building the elementary packs

`data/elem.json` is what the page ships. The words and their pictures are generated on the authoring machine by a small set of scripts kept in a `build/` folder beside the page (that folder is listed in `.gitignore`, so it is not published, like the earlier pack generators):

- `build/elem_data.py` — the 816 words (word, part of speech, English meaning, Korean meaning, and a picture recipe of the form `background|piece@x,y,scale,name@…`).
- `build/04-elem-kit.js` … `build/10-elem-misc.js` — the elementary picture kit, inserted into `index.html` by `build/assemble_elem.py`.
- `build/build_elem.py` — writes `data/elem.json`, checking every word against the official 800 list and reporting anything missing or repeated.
- `build/check_elem.py` — runs the page's own script headless (quickjs), draws every scene and fails if a scene is malformed, a Day holds fewer than 10 or more than 12 words, or two words in one Day look the same.
- `build/elem_sheet.py g3 5 1` — renders one Day as a PNG contact sheet so the pictures can be looked at.

## Building the 토익 packs

`data/toeic.json` is built the same way, from two word files in `build/`:

- `build/toeic_basic.py` — the 108 new 베이직 words (Days 2–10) plus the two that join Day 1 (`desk`, `printer`), in the same `background|piece@x,y,scale,name@…` recipe form.
- `build/toeic_600.py` — the 168 new 600 words (Days 2–15) plus `position` and `salary`.
- `build/build_toeic.py` — reads the ten words each pack already drew out of `build/02-toeic.js` (so their pictures and word ids stay identical and collected stickers still match), adds the new words, and writes `data/toeic.json`. It stops on an unknown piece name, a Day that does not hold twelve words, or a word that appears in both packs.
- `build/check_toeic.py` — runs the page's own script headless, draws all 600 scenes and fails on a malformed picture, a Day that is not twelve words, two words in one Day that look the same, or any letter inside a picture (which would give the answer away).
- `build/toeic_sheet.py toeic2 2` — renders one Day as a labelled contact sheet.
- `build/smoke_loader.py` — boots the page with a stubbed `fetch` and checks that both word files land in `PACKS`, that a saved 베이직 Day survives, and that 750/850 stay ten-word packs.
