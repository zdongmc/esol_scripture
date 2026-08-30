# DESIGN — ESOL Scripture Handout Generator

## Overview

A single-page web app that generates printable multilingual Bible scripture handouts for ESOL (English for Speakers of Other Languages) ministry. Users pick a scripture letter (A–Z) and a set of languages, then print or save a formatted handout showing that verse in every selected language.

## Architecture

```
index.html          Single-file SPA (HTML + CSS + JS, no build step)
translations.json   All verse data: 25 scriptures x 17 languages, English x 3 versions
userguide.html      Standalone one-page printable guide for volunteers
CLAUDE.md           Ground truth for scriptures, languages and English versions
SOURCES.md          Verified per-language sources, parsing rules, verification status
```

There is no server. The app is static and deployable on GitHub Pages or any file host.

**Deployment:** GitHub Pages serves `main`, at https://zdongmc.github.io/esol_scripture/.
A feature branch publishes nothing — work only goes live when it reaches `main`.

`userguide.html` shares no CSS with `index.html` and must print on exactly one
letter-portrait page; see the User guide section of `CLAUDE.md` for that constraint and
how to verify it.

### How it works

1. `index.html` loads `translations.json` via `fetch()` on startup.
2. JavaScript builds a grid of letter buttons (A–Z, minus V), a radio group for the English
   version, and language checkboxes.
3. User clicks a letter, picks an English version, and toggles languages.
4. The preview renders immediately in the DOM — no framework, no templating library.
5. "Print / Save as PDF" triggers `window.print()`. A `@media print` stylesheet hides the UI and formats the handout for paper (letter size).

### Data flow

```
_archive/ScriptureAlphabet.pdf (original source document — letters, refs, key phrases)
        |
        v
CLAUDE.md + SOURCES.md         (human-curated: what each language is, where text comes from)
        |
        v
translations.json              (machine-readable, used by the app)
        |
        v
index.html                     (renders handouts in the browser)
```

The PDF is the authority for the letter/verse mapping and the heading phrases. Note it
contains three reference typos that `CLAUDE.md` corrects and must keep corrected:
D is Matthew 7:12 (not 11:28), M is Psalm 121:2 (not 12:12), Y is Matthew 5:13 (not 5:15).

## Scripture Alphabet

Each letter A–Z maps to a Bible verse whose English key phrase starts with that letter, and
fifteen letters offer a second verse the volunteer can pick instead. The mapping is defined in the Scriptures section of `CLAUDE.md`. Example:

| Letter | Reference     | Key Phrase               |
|--------|---------------|--------------------------|
| A      | Matthew 7:7   | Ask and you will receive |
| B      | Ephesians 4:32| Be kind and tenderhearted|
| ...    | ...           | ...                      |

V was absent from the original Scripture Alphabet source material and was added later by the
ESOL team (Proverbs 4:8), so the alphabet is now complete. The alternates are listed in the
Alternate verses section of `CLAUDE.md`.

## translations.json

### Structure

```json
{
  "_meta": {
    "languages": ["en", "es", "zh-tw", "pt", "it", "fr", "am", "hi", "uk", "te", "fa", "zh-cn", "ko", "vi", "ar", "ta", "be"],
    "note": "..."
  },
  "A": {
    "reference": "Matthew 7:7",
    "key": "Ask and you will receive",
    "en": {
      "version": "GNT",
      "text": "Ask, and you will receive; ...",
      "versions": {
        "CEV": "Ask, and you will receive; ...",
        "ERV": "Continue to ask, and God will give to you; ...",
        "BSB": "Ask, and it will be given to you; ..."
      }
    },
    "es": { "version": "NVI", "text": "Pidan y se les dará; ..." },
    ...
  },
  "R": {
    "reference": "Ecclesiastes 12:1",
    "key": "Remember your Creator while you are still young",
    "it": { "version": "NR", "ref": "Ecclesiaste 12:3", "text": "Ma ricòrdati ..." },
    ...
    "alternates": [
      {
        "reference": "Psalm 37:31",
        "key": "Remember God's teachings",
        "en": { "version": "GNT", "text": "...", "versions": { "CEV": "...", "ERV": "...", "BSB": "..." } },
        ...
      }
    ]
  }
}
```

Each scripture entry contains:
- `reference` — the Bible book, chapter, and verse
- `key` — the abbreviated phrase exactly as printed in `_archive/ScriptureAlphabet.pdf`,
  ellipses included. Used as the handout heading. Deliberately *not* derived from the
  verse text: the PDF's phrasing is the mnemonic the alphabet is built on.
- One object per language with:
  - `version` — the translation abbreviation. **Read per verse, not per language**:
    letters K and U are NIV while the rest of the English column is GNT.
  - `text` — the verse
  - `ref` *(optional)* — that language's own reference, where its versification
    differs. Italian NR places Ecclesiastes 12:1 at 12:3. Rendered under the version.
  - `versions` *(English only)* — alternate wordings keyed by abbreviation, currently
    `CEV`, `ERV` and `BSB`. `version`/`text` stay the default, so anything reading the file
    without knowing about alternates still gets the right verse.
- `alternates` *(optional)* — an array of **whole second verses** for that letter, each
  shaped exactly like a letter entry: its own `reference`, its own `key`, and its own row
  per language. The letter's own verse stays at the top level and is what the app shows
  first, so the same rule holds — a reader that ignores `alternates` still gets the right
  verse.

**Two pickers, two different things.** The *English Version* picker swaps the wording of one
row and leaves the heading alone. The *Verse* picker swaps the whole verse — reference, key
and all 17 rows — and appears only for letters that have an alternate.

`_meta.englishVersions` names the four choices the picker offers.

**Only English is switchable.** The handout heading — letter, `reference` and `key` — is
deliberately unaffected by the choice: `key` comes from the source PDF, which was written
from the GNT, and no other version opens Revelation 1:3 with "Happy" or Exodus 20:3 with
"Worship". Every alternate loses the letter's own word somewhere (ERV on 5 letters, BSB on
3); the fixed heading is what keeps the alphabet legible on those handouts.

### How translations are obtained

Every translation in `translations.json` is sourced from an official, published Bible translation — **not** machine-translated or AI-generated. The process:

1. **Identify the canonical Bible version** for each language (defined in the Languages section of `CLAUDE.md`):

   | Language   | Code  | Version                          | Abbreviation |
   |------------|-------|----------------------------------|--------------|
   | English    | en    | Good News Translation            | GNT          |
   | Spanish    | es    | Nueva Version Internacional      | NVI          |
   | Chinese (Trad.) | zh-tw | Chinese Union Version (Trad.) | CUV         |
   | Portuguese | pt    | Nova Versao Internacional        | NVI          |
   | Italian    | it    | Nuova Riveduta (2006)            | NR           |
   | French     | fr    | Louis Segond                     | LSG          |
   | Amharic    | am    | መጽሐፍ ቅዱስ (Bible Society of Ethiopia) | 1962      |
   | Hindi      | hi    | Indian Revised Version (IRV)     | IRV          |
   | Ukrainian  | uk    | UKR "Ukrainian Bible"            | UKR          |
   | Telugu     | te    | Indian Revised Version (IRV)     | IRV          |
   | Farsi      | fa    | Old Persian Version              | OPV          |
   | Chinese (Simp.) | zh-cn | Contemporary Bible Simplified | CCB        |
   | Korean     | ko    | Korean Bible                     | KOR          |
   | Vietnamese | vi    | Vietnamese Bible 1934            | VIE          |
   | Arabic     | ar    | Van Dyke Arabic Bible            | AVD          |
   | Tamil      | ta    | Indian Revised Version (IRV)     | IRV          |
   | Belarusian | be    | Biblija (Bokun translation)      | BEL          |

2. **Fetch each verse from its verified source.** The working URL pattern, parsing
   rules, and access terms for each language live in **[`SOURCES.md`](SOURCES.md)**.
   Sources that were tried and rejected are recorded there too, with the reason —
   several widely-cited ones return subtly wrong text.

3. **Assert the reference came back.** Do not trust the URL. Every fetch reads the
   returned page title, heading, or echoed book/chapter/verse and checks it matches
   what was requested. A wrong verse is still real scripture and reads plausibly;
   this is the only defence against it.

4. **Copy the exact text — no paraphrasing, no editing, never self-translate.**
   Formatting decisions, all of which preserve rather than alter the published text:

   - **Verses that begin mid-sentence keep their lowercase opening.** Eight do
     (E, F, N, S across it/fr/en/pt). Capitalising them would alter the published
     text, so it is not done.
   - **The small-caps divine name is rendered uppercase** — `LORD`, `SEÑOR`,
     `SENHOR`. The small caps are the marker distinguishing YHWH from the ordinary
     title "Lord"; flattening the HTML span silently destroys that distinction.
   - **The reverential space before `神`** in Chinese CUV is preserved. It is
     typography, not a stray space.
   - **Quote marks left unmatched by extraction are dropped.** Quoted discourse runs
     across verse boundaries, so a single verse often carries an opening `「`, `“` or
     `«` it never closes. `‘ ’` are never touched — they double as apostrophes.
   - **Editorial apparatus is removed** — e.g. CUV's `（有古卷沒有末句）`
     ("some manuscripts omit the final clause") is notation, not scripture.
   - **Parsing artifacts are removed** — a stray `፤` inherited from the previous
     verse, or the stray `ፕ` in the Amharic Exodus 20:3 page.

### Verification sources

Full URL patterns, parsing rules, and access terms are in **[`SOURCES.md`](SOURCES.md)**.
Summary, with verification status:

| Language | Primary source | Verified verse-by-verse |
|----------|----------------|:--:|
| English (GNT + NIV) | BibleGateway | ✅ 2026-08-26 |
| English alt. (ERV) | ebible.org (`engerv`) + BibleGateway | ✅ 2026-08-29 — 1,000-verse allowance |
| English alt. (BSB) | ebible.org (`engbsb`) + bereanbible.com | ✅ 2026-08-29 — **public domain** |
| Spanish (NVI) | BibleGateway (`NVI`) | ✅ 2026-08-26 |
| Chinese Trad. (CUV) | FHL 信望愛, Taiwan (JSON API) | ✅ 2026-08-26 |
| Portuguese (NVI) | BibleGateway (`NVI-PT`) | ✅ 2026-08-26 |
| Italian (NR 2006) | LaParola.net | ✅ 2026-08-26 |
| French (LSG) | SainteBible | ✅ 2026-08-26 |
| Amharic (1962) | wordproject.org | ✅ 2026-08-26 |
| Hindi (IRV) | ebible.org (`hin2017`) | ✅ 2026-08-27 — **CC BY-SA** |
| Ukrainian (UKR) | BibleGateway (`UKR`) + htmlbible.com | ✅ 2026-08-27 |
| Telugu (IRV) | ebible.org (`tel2017`) | ✅ 2026-08-27 — **CC BY-SA** |
| Farsi (OPV) | ebible.org (`pesOPV`, cross-checked vs `pesopcb`) | ✅ 2026-08-27 |
| Chinese Simp. (CCB) | ebible.org (`cmncbs`) | ✅ 2026-08-27 — **CC BY-SA** |
| Korean (1910) | ebible.org (`kor`) + studybible.info | ✅ 2026-08-27 |
| Vietnamese (VIE) | ebible.org (`vie1934`) | ✅ 2026-08-27 |
| Arabic (AVD/SVD) | ebible.org (`arb-vd`) + copticchurch.net | ✅ 2026-08-27 |
| Tamil (IRV) | ebible.org (`tam2017`) | ✅ 2026-08-27 — **CC BY-SA** |
| Belarusian (BEL) | ebible.org (`bel`, cross-checked vs `beln`) | ✅ 2026-08-27 — **CC BY-ND** |

**All 17 languages are verified.** Twelve of the seventeen had a real defect —
truncated verses, a mixed or mislabelled translation, a versification offset, an
off-by-one source, or a wrong version label. Truncation was the most common and the
most dangerous, since a cut verse still reads as a complete sentence. The damaged
letters were always the multi-line poetry verses: F, R, T and X. See the Verification
status section of `SOURCES.md`.

### Adding a new language

1. Add a row to the Languages table in `CLAUDE.md` with the language code, display name, Bible version, abbreviation, and direction.
2. Probe a source and record the working URL pattern, parsing rules, and access terms in `SOURCES.md`. Check `robots.txt` and honour any crawl delay.
3. For each of the 26 letters **and each of the 15 alternates** (41 verses), fetch the verse and **assert the returned reference matches the one requested** before storing it.
4. Add the language to the `LANGUAGES` array in `index.html` (with `dir: "rtl"` if needed).
5. Update the verification status table in `SOURCES.md`.

### Adding an English version

1. Check the licence *first* — it decides the question more often than readability does.
   A version with no stated verse allowance (the GNT and CEV both carry the restrictive
   American Bible Society notice) is a weaker position than an open one, whatever its
   reading level.
2. Fetch all 41 entries (26 letters + 15 alternates) and **measure two things before
   committing to it**: words per sentence, and how many still contain the letter's own
   word. The second is the one that matters here and the one that eliminates
   otherwise-appealing candidates — the Bible in Basic English has the simplest
   vocabulary of any English Bible and fails on 12 of 25.
3. Cross-check every verse against a second independent source, as for any language.
   **CEV is the standing exception**: only BibleGateway carries it (eBible returns 404),
   so it rests on the per-fetch reference assertion alone. Record any such exception in
   `SOURCES.md` rather than letting it pass silently.
4. Add the text to `en.versions` in `translations.json`, keyed by abbreviation.
5. Add an entry to `EN_VERSIONS`, `EN_VERSION_NOTES` and `EN_VERSION_HINTS` in
   `index.html`, and to `_meta.englishVersions`. `EN_VERSION_NOTES` needs both an `own`
   and an `alt` label: the "matches the heading" marker follows the selected verse,
   because the letters' Key Texts are GNT-worded and the alternates' are CEV-worded.
6. Add a `CREDITS` line **only if the licence requires one** — public-domain versions are
   omitted by design.
7. Update the English versions table in `CLAUDE.md` and the source rows in `SOURCES.md`.
8. The user guide names the versions in step 3; it must still print on one page.

### Adding a new scripture

1. Add a row to the Scriptures table in `CLAUDE.md` with the letter, reference, version, and key text.
2. Add a new entry to `translations.json` with `reference`, `key` (the abbreviated heading phrase), and all language translations fetched from official sources.

## UI Design

- **Minimal, print-first**: The app is styled to look clean on screen and produce a well-formatted handout on paper.
- **No dependencies**: Zero npm packages, no build step, no framework. One HTML file + one JSON file.
- **Georgia serif** for verse text (readability across scripts), **system-ui sans-serif** for UI chrome.
- **Print stylesheet** hides all UI elements and formats the handout for US Letter paper with appropriate margins.
