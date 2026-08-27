# DESIGN — ESOL Scripture Handout Generator

## Overview

A single-page web app that generates printable multilingual Bible scripture handouts for ESOL (English for Speakers of Other Languages) ministry. Users pick a scripture letter (A–Z) and a set of languages, then print or save a formatted handout showing that verse in every selected language.

## Architecture

```
index.html          Single-file SPA (HTML + CSS + JS, no build step)
translations.json   All verse data: 25 scriptures x 17 languages
CLAUDE.md           Ground truth for scriptures and languages
SOURCES.md          Verified per-language sources, parsing rules, verification status
```

There is no server. The app is static and deployable on GitHub Pages or any file host.

### How it works

1. `index.html` loads `translations.json` via `fetch()` on startup.
2. JavaScript builds a grid of letter buttons (A–Z, minus V) and language checkboxes.
3. User clicks a letter and toggles languages.
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

Each letter A–Z (except V) maps to a Bible verse whose English key phrase starts with that letter. The mapping is defined in the Scriptures section of `CLAUDE.md`. Example:

| Letter | Reference     | Key Phrase               |
|--------|---------------|--------------------------|
| A      | Matthew 7:7   | Ask and you will receive |
| B      | Ephesians 4:32| Be kind and tenderhearted|
| ...    | ...           | ...                      |

V is intentionally absent from the original Scripture Alphabet source material.

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
    "en": { "version": "GNT", "text": "Ask, and you will receive; ..." },
    "es": { "version": "NVI", "text": "Pidan y se les dará; ..." },
    ...
  },
  "R": {
    "reference": "Ecclesiastes 12:1",
    "key": "Remember your Creator while you are still young",
    "it": { "version": "NR", "ref": "Ecclesiaste 12:3", "text": "Ma ricòrdati ..." },
    ...
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
| Korean (KOR) | ebible.org (`kor`) | ✅ 2026-08-27 — single-sourced |
| Vietnamese (VIE) | ebible.org (`vie1934`) | ⚠️ not yet |
| Arabic (AVD/SVD) | ebible.org (`arb-vd`) + copticchurch.net | ✅ 2026-08-27 |
| Tamil (IRV) | ebible.org (`tam2017`) | ✅ 2026-08-27 — **CC BY-SA** |
| Belarusian (BEL) | ebible.org (`bel`, cross-checked vs `beln`) | ✅ 2026-08-27 — **CC BY-ND** |

**One language still needs verification.** Seven of the nine that were checked had a
real defect — a versification offset, an off-by-one source, a mixed dialect, truncated
verses, a mislabelled translation, leaked section headings and footnotes. French and Arabic
were already correct. Assume the remaining one is unchecked rather than clean; see
the Verification status section of `SOURCES.md` for the specific traps to look for.

### Adding a new language

1. Add a row to the Languages table in `CLAUDE.md` with the language code, display name, Bible version, abbreviation, and direction.
2. Probe a source and record the working URL pattern, parsing rules, and access terms in `SOURCES.md`. Check `robots.txt` and honour any crawl delay.
3. For each of the 25 scriptures, fetch the verse and **assert the returned reference matches the one requested** before storing it.
4. Add the language to the `LANGUAGES` array in `index.html` (with `dir: "rtl"` if needed).
5. Update the verification status table in `SOURCES.md`.

### Adding a new scripture

1. Add a row to the Scriptures table in `CLAUDE.md` with the letter, reference, version, and key text.
2. Add a new entry to `translations.json` with `reference`, `key` (the abbreviated heading phrase), and all language translations fetched from official sources.

## UI Design

- **Minimal, print-first**: The app is styled to look clean on screen and produce a well-formatted handout on paper.
- **No dependencies**: Zero npm packages, no build step, no framework. One HTML file + one JSON file.
- **Georgia serif** for verse text (readability across scripts), **system-ui sans-serif** for UI chrome.
- **Print stylesheet** hides all UI elements and formats the handout for US Letter paper with appropriate margins.
