# ESOL Multi-language Bible Scripture Handout Generator

A static single-page web app for ESOL ministry that generates printable multilingual Bible scripture handouts. Deployable on GitHub Pages.

See DESIGN.md

## How it works

- `index.html` — single-file SPA (HTML + CSS + JS, no build step, no dependencies)
- `translations.json` — 26 scriptures (A–Z) x 17 languages, 15 of which offer a second verse
- User picks a letter and languages, previews the handout, prints or saves as PDF via the browser

## Key files

- `DESIGN.md` — full architecture and translation sourcing process
- `userguide.html` — one-page printable guide for ESOL volunteers (see below)

## User guide

`userguide.html` is a standalone one-page guide handed to volunteers. It carries the app URL
and a QR code, the five steps for using the app, the 25-letter verse index, the language
roster, and the credit-retention note. It is self-contained — no build step, no shared CSS
with `index.html` — and is served from Pages alongside the app.

**Constraint: it must print on exactly one letter-portrait page**, and the type must stay
readable — do not solve an overflow by shrinking the font. Reclaim space through layout
(the QR sits in the masthead, the five steps run three across, the languages are one flowing
line) or by cutting content.

Two things learned adding the fifth step, both counter-intuitive:

- **More columns is not always shorter.** Moving the steps from 2 columns to 3 made the
  block *taller* (3.25in → 4.38in) — narrower cells wrap more lines than the saved row
  gives back. It only paid off once the copy was cut to suit the narrower measure.
- **The step numeral was costing more than it was worth.** At 3 columns a 2rem numeral
  took 19% of each cell's width; narrowing it to 1.4rem removed a wrapped line from most
  cells, and a two-line `<h2>` cost more than a whole line of body text.

There is now only **~0.1in of slack in the fallback case** (8px fits, 10px breaks to a
second page); the web-font rendering has more. **The fallback is the binding case** — check
it, not just the published rendering.

Adding V as a 26th letter cost a whole index row and broke the fallback to two pages. Two
more data points from fixing it, both consistent with the lessons above:

- **Six columns was worse than five**, and broke even the web-font case: "Ephesians 4:32"
  does not fit a sixth-width cell, so every wrapped item cost more than the saved row.
- **The row gap paid for the row.** Tightening `.verse-index` row-gap from 0.3rem to 0.12rem
  recovered ~0.17in across the six rows — more than the new row cost — with no change to
  type size. Spacing, not size, is where the give is.

Verify a change with headless Chrome rather than by eye, and check the web-font fallback
too — Source Serif 4 / IBM Plex Sans fall back to Georgia and system-ui, which set wider:

```bash
chrome --headless --no-pdf-header-footer --virtual-time-budget=8000 \
  --print-to-pdf=out.pdf file://$PWD/userguide.html
python3 -c "import pypdf; print(len(pypdf.PdfReader('out.pdf').pages))"   # must be 1
```

The QR code is inlined SVG (error-correction level H) generated with `segno` and decode-checked;
regenerate and re-verify it if the URL ever changes.

**Contact:** Jojo is credited on the guide as the app's creator and the person volunteers
should reach for questions, suggestions, or requests for additional languages.

## Scriptures

Ground truth for Scripture Alphabet verses used in the ESOL handout generator.
Each entry maps a letter to a specific verse reference, short key text, and source Bible version.

> To add or edit a scripture, update this section and regenerate `translations.json`.

| Letter | Reference        | Version | Key Text (English)                                                                 |
|--------|------------------|---------|------------------------------------------------------------------------------------|
| A      | Matthew 7:7      | GNT     | Ask and you will receive                                                           |
| B      | Ephesians 4:32   | GNT     | Be kind and tenderhearted to one another                                           |
| C      | Matthew 11:28    | GNT     | Come to me … and I will give you rest                                              |
| D      | Matthew 7:12     | GNT     | Do for others what you want them to do for you                                     |
| E      | James 1:17       | GNT     | Every good gift … comes from heaven                                                |
| F      | Luke 11:4        | GNT     | Forgive us our sins, for we forgive everyone who does us wrong                     |
| G      | 1 John 4:8       | GNT     | God is love                                                                        |
| H      | Revelation 1:3   | GNT     | Happy is the one who reads this book                                               |
| I      | 1 John 1:9       | GNT     | If we confess our sins to God … he will forgive us our sins                        |
| J      | Hebrews 13:8     | GNT     | Jesus Christ is the same yesterday, today, and forever                             |
| K      | 1 Corinthians 7:19 | NIV   | Keeping God's commands is what counts                                              |
| L      | Mark 12:31       | GNT     | Love your neighbor as you love yourself                                            |
| M      | Psalm 121:2      | GNT     | My help will come from the Lord                                                    |
| N      | Luke 22:42       | GNT     | Not my will … but your will be done                                                |
| O      | Hebrews 13:17    | GNT     | Obey your leaders and follow their orders                                          |
| P      | James 5:16       | GNT     | Pray for one another                                                               |
| Q      | Isaiah 30:15     | GNT     | Quietly trust in me                                                                |
| R      | Ecclesiastes 12:1 | GNT    | Remember your Creator while you are still young                                    |
| S      | 1 Samuel 3:9     | GNT     | Speak, Lord, your servant is listening                                             |
| T      | Proverbs 22:6    | GNT     | Teach children how they should live, and they will remember it all their life      |
| U      | John 3:3         | NIV     | Unless he is born again no one can see the kingdom of God                          |
| V      | Proverbs 4:8     | GNT     | Value Wisdom and hold tightly to her                                               |
| W      | Exodus 20:3      | GNT     | Worship no god but me                                                              |
| X      | Psalm 26:2       | GNT     | Examine me and test me, Lord                                                       |
| Y      | Matthew 5:13     | GNT     | You are like salt for the whole human race                                         |
| Z      | Luke 19:6        | GNT     | Zacchaeus … welcomed him with great joy                                            |

- **V was absent from the original Scripture Alphabet source document** and was added later by
  the ESOL team (Proverbs 4:8). It is a full letter, not an alternate — the alphabet is now A–Z.
- Key Text is the English prompt phrase, not necessarily the full verse. Full verse text lives in `translations.json`.
- Version listed is the English source version. Each language uses its own canonical version (see Languages section below).
- The Version column is the **default** English wording. The app also offers two alternates —
  see English versions below.

## Alternate verses

Fifteen letters offer a **second verse**, chosen by the ESOL team. The app shows a *Verse*
picker above the language list whenever the selected letter has one; letters with a single
verse show no picker at all.

| Letter | Alternate reference | Key Text (English) |
|--------|---------------------|--------------------|
| D | Matthew 7:1     | Don't condemn others, and God won't condemn you |
| H | Revelation 4:8  | Holy, holy, holy is the Lord, the all-powerful God |
| J | 1 Timothy 1:15  | Jesus came into the world to save sinners |
| K | Matthew 6:13    | Keep us from being tempted and protect us from evil |
| L | Luke 10:27      | Love your neighbor as much as you love yourself |
| M | Mark 11:17      | My house should be a place of worship for all nations |
| N | Romans 8:38     | Nothing can separate us from God's love |
| O | Matthew 19:17   | Only God is good |
| P | 1 Timothy 2:1   | Pray for everyone |
| R | Psalm 37:31     | Remember God's teachings |
| S | Proverbs 2:4    | Search for wisdom |
| U | Psalm 90:12     | Use wisely all the time we have |
| W | Psalm 32:6      | We worship you, Lord |
| X | Colossians 4:3  | Explain the mystery about Christ |
| Y | Galatians 3:26  | You are God's children because of your faith in Christ Jesus |

- **An alternate is a whole verse, not a wording.** It carries its own reference, its own Key
  Text and its own row for every one of the 17 languages, each in that language's canonical
  version. This is the opposite of the English-version picker, which swaps the wording of one
  row and leaves the heading alone.
- **The Key Texts are the team's CEV phrasing**, and V's is too. Every earlier Key Text was
  taken from the GNT wording in the source PDF, so this is a new provenance — recorded here
  because the phrase on the handout will not always match the English verse beneath it
  (GNT Proverbs 4:8 opens "Love wisdom", not "Value Wisdom"; GNT Revelation 4:8 opens with the
  four living creatures, not "Holy"). The heading carries the alphabet cue; the body need not.
- **The letter's own verse is always first** in the picker and is what the app shows until the
  volunteer chooses otherwise. Changing letter resets the choice.
- Alternates live in `translations.json` under `alternates` on the letter, an array of entries
  shaped exactly like a letter. A reader that ignores `alternates` still gets the right verse.

## English versions

The English row alone is switchable; every other language has one version. The picker sits
above the language checkboxes and offers:

| Choice | Version | Licence | Keeps the cue: letters | alternates |
|--------|---------|---------|:---:|:---:|
| Good News (default) | GNT, and NIV for K and U | © ABS / © Biblica, no stated verse allowance | 25/26 | 9/15 |
| Contemporary English | CEV © 1995 American Bible Society | same ABS notice as the GNT | 19/26 | **15/15** |
| Easy-to-Read | ERV © 2006 Bible League International | up to **1,000 verses**, <50% of the work | 20/26 | 10/15 |
| Berean Standard | BSB | **public domain** | 22/26 | 8/15 |

- **The handout heading never changes with this choice.** The letter, the reference and the
  Key Text stay as written; only the English verse below them follows the picker. This is
  deliberate — the Key Text was written from the GNT, and no other version opens
  Revelation 1:3 with "Happy" or Exodus 20:3 with "Worship".
- **Every non-default version loses the letter's word on H, and most also on K, U, X.** The
  fixed heading carries the alphabet cue on those handouts; the body will visibly differ.
- **The two columns pull in opposite directions, which is the point of having four choices.**
  On the 26 letters the GNT is almost perfect (25/26 — it misses only V, whose Key Text is
  CEV-worded). On the 15 alternates it manages just 9/15, while **CEV scores 15/15** — those
  Key Texts were written from the CEV, so it is the one version that can put the English
  verse into the same words as the heading above it. Pick GNT for the original letters, CEV
  when showing an alternate.
- **CEV was rejected once, then adopted.** The original reasoning still stands on its own
  terms — same American Bible Society notice as the GNT, so no licence gain, and it was
  rejected against ERV on that basis. What changed is not the licence but the content: the
  alternates and V arrived with CEV-worded Key Texts, which no other version matches. Record
  it as licence-neutral, adopted for coherence. **BBE was rejected**: smallest vocabulary of any English
  Bible but the *longest* sentences (20.8 words vs ERV's 12.0) and only 13/25 on the cue —
  it reads harder than ERV despite the simpler words. **OEB was rejected**: no Old
  Testament, which would blank 5 of the 25 letters.
- Alternates live in `translations.json` under `en.versions`; `en.text` remains the default.
  `EN_VERSIONS` and `EN_VERSION_HINTS` in `index.html` drive the picker.
- BSB is public domain, so it prints **no** credit line — consistent with the rule that
  public-domain versions are omitted from the footer. ERV and CEV both print one.
- **CEV has no second source.** eBible does not carry it (404), so unlike every other version
  here it rests on BibleGateway's per-fetch reference assertion alone. See `SOURCES.md`.

## Languages

Ground truth for languages supported in the ESOL handout generator.
Each entry defines the language code, display name, Bible version, and text direction.

> To add or edit a language, update this section and regenerate `translations.json`.

| Code  | Display Name              | Bible Version                          | Abbreviation | Direction |
|-------|---------------------------|----------------------------------------|--------------|-----------|
| en    | English                   | Good News Translation                  | GNT          | LTR       |
| am    | አማርኛ (Amharic)            | መጽሐፍ ቅዱስ (Bible Society of Ethiopia)   | 1962         | LTR       |
| ar    | العربية (Arabic)           | Van Dyke Arabic Bible                  | AVD          | RTL       |
| be    | Беларуская (Belarusian)   | Біблія (пераклад А.Бокуна)             | BEL          | LTR       |
| zh-cn | 简体中文 (Simplified Chinese) | 圣经当代译本 (Contemporary Bible Simplified) | CCB   | LTR       |
| zh-tw | 繁體中文 (Traditional Chinese) | 中文和合本 (Chinese Union Version)     | CUV          | LTR       |
| fa    | فارسی (Farsi)              | Old Persian Version                    | OPV          | RTL       |
| fr    | Français (French)         | Louis Segond                           | LSG          | LTR       |
| hi    | हिन्दी (Hindi)             | Indian Revised Version (IRV) Hindi 2019 | IRV         | LTR       |
| it    | Italiano (Italian)        | Nuova Riveduta                         | NR           | LTR       |
| ko    | 한국어 (Korean)              | 한국어 성경 (Korean Bible)              | KOR          | LTR       |
| pt    | Português (Portuguese)    | Nova Versão Internacional              | NVI          | LTR       |
| es    | Español (Spanish)         | Nueva Versión Internacional            | NVI          | LTR       |
| ta    | தமிழ் (Tamil)              | Indian Revised Version (IRV) Tamil     | IRV          | LTR       |
| te    | తెలుగు (Telugu)            | Indian Revised Version (IRV) Telugu 2019 | IRV         | LTR       |
| uk    | Українська (Ukrainian)    | UKR "Ukrainian Bible" (public domain)  | UKR          | LTR       |
| vi    | Tiếng Việt (Vietnamese)   | Vietnamese Bible 1934                  | VIE          | LTR       |

- `code` is the key used in `translations.json` and in the web app.
- Arabic (`ar`) and Farsi (`fa`) use RTL layout; all others use LTR.
- **Ordering:** English first, then alphabetical by English name — except that variants of one language sort under a shared base name so they stay adjacent (both Chinese sort under "Chinese"). The `LANGUAGES` array in `index.html` drives both the picker and the handout row order; keep this table, `_meta.languages`, and that array in the same order.
- Ukrainian (`uk`) uses UKR throughout. An earlier ONPU-based version covered only NT+Psalms and was mixed with a second translation for the 5 OT verses; UKR replaces both. Chosen because the Ukrainian learners are typically grandparents, for whom the older wording is the familiar one — see `SOURCES.md`.
- To add a language: add a row here, add a verified source to `SOURCES.md`, then populate `translations.json`.
- **Verification status: all 17 languages verified** (2026-08-26/27), each verse fetched individually with its reference asserted. Every language has either a second source or an independent structural check. See `SOURCES.md`.
- **Licensing:** Belarusian (Bokun) is CC BY-ND 4.0 — it may not be modified. Simplified Chinese (CCB), Hindi, Tamil and Telugu (IRV) are CC BY-SA 4.0. The handout prints a credit footer for these (see `CREDITS` in `index.html`); public-domain versions need none and are omitted. Every other verified language is public domain or permissive. See `SOURCES.md`.

### Lookup Sources by Language

All translations in `translations.json` are copied verbatim from official published Bible versions — never AI-generated or machine-translated.

**See [`SOURCES.md`](SOURCES.md)** for the verified per-language URL pattern, parsing rules, access terms, and the sources tested and rejected. It also records what verification found in each language, the traps to check for first if another is added, and the caveats that remain.

`DESIGN.md` describes the overall verification process.
