# DESIGN — ESOL Scripture Handout Generator

## Overview

A single-page web app that generates printable multilingual Bible scripture handouts for ESOL (English for Speakers of Other Languages) ministry. Users pick a scripture letter (A–Z) and a set of languages, then print or save a formatted handout showing that verse in every selected language.

## Architecture

```
index.html          Single-file SPA (HTML + CSS + JS, no build step)
translations.json   All verse data: 25 scriptures x 17 languages
CLAUDE.md           Ground truth for scriptures, languages, and lookup sources
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
CLAUDE.md                      (human-curated source of truth)
        |
        v
translations.json              (machine-readable, used by the app)
        |
        v
index.html                     (renders handouts in the browser)
```

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
    "en": { "version": "GNT", "text": "Ask, and you will receive; ..." },
    "es": { "version": "NVI", "text": "Pedid, y se os dará; ..." },
    ...
  },
  ...
}
```

Each scripture entry contains:
- `reference` — the Bible book, chapter, and verse
- One object per language with `version` (Bible translation abbreviation) and `text` (the verse)

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
   | Amharic    | am    | Amharic Bible (Bible Society)    | ABB          |
   | Hindi      | hi    | Indian Revised Version (IRV)     | IRV          |
   | Ukrainian  | uk    | Open New Translation Ukrainian   | ONPU         |
   | Telugu     | te    | Indian Revised Version (IRV)     | IRV          |
   | Farsi      | fa    | Old Persian Version              | OPV          |
   | Chinese (Simp.) | zh-cn | Contemporary Bible Simplified | CCB        |
   | Korean     | ko    | Korean Bible                     | KOR          |
   | Vietnamese | vi    | Vietnamese Bible 1934            | VIE          |
   | Arabic     | ar    | Van Dyke Arabic Bible            | AVD          |
   | Tamil      | ta    | Indian Revised Version (IRV)     | IRV          |
   | Belarusian | be    | Biblija (Bokun translation)      | BEL          |

2. **Fetch each verse from an official online source**:
   - **English (GNT), Spanish (NVI), Portuguese (NVI), French (LSG)**: Retrieved from standard Bible study sites (biblestudytools.com, bibleserver.com, saintebible.com).
   - **Chinese Traditional (CUV)**: Retrieved from biblestudytools.com or bible.com.
   - **Italian (NR 2006)**: Verified against BibleGateway.com (`version=NR2006`), extracted from `og:description` meta tags via `curl`.
   - **Amharic (ABB)**: Fetched from wordproject.org (`/bibles/am/[book]/[chapter].htm`), extracting verse text from the HTML `<span class="verse">` structure.

3. **Copy the exact text** — no paraphrasing, no editing. Minor formatting decisions:
   - Verses that begin mid-sentence in the original (e.g., James 1:17 starting with lowercase) are capitalized for standalone display.
   - Guillemets (« ») and typographic quotes are preserved as they appear in the source version.
   - Leading punctuation artifacts from HTML parsing (e.g., a stray `፤` from the previous verse) are removed.

### Verification sources

| Language   | Primary Source              | URL Pattern |
|------------|-----------------------------|-------------|
| Italian    | BibleGateway.com (NR2006)   | `biblegateway.com/passage/?search=...&version=NR2006` |
| Amharic    | WordProject.org             | `wordproject.org/bibles/am/[book]/[chapter].htm` |
| Spanish    | BibleStudyTools / BibleServer | various |
| Chinese (Trad.) | BibleStudyTools / Bible.com | various |
| Portuguese | BibliaTodo / bo.net.br      | various |
| French     | SainteBible.com / BibleServer | various |
| Hindi      | ebible.org (hin2017)        | `ebible.org/Scriptures/hin2017_html.zip` |
| Ukrainian  | ebible.org (ukronpu)        | `ebible.org/Scriptures/ukronpu_html.zip` (NT+Psalms only) |
| Telugu     | ebible.org (tel2017)        | `ebible.org/Scriptures/tel2017_html.zip` |
| Farsi      | ebible.org (pesOPV)         | `ebible.org/Scriptures/pesOPV_html.zip` |
| Chinese (Simp.) | ebible.org (cmncbs)    | `ebible.org/Scriptures/cmncbs_html.zip` |
| Korean     | ebible.org (kor)            | `ebible.org/Scriptures/kor_html.zip` |
| Vietnamese | ebible.org (vie1934)        | `ebible.org/Scriptures/vie1934_html.zip` |
| Arabic     | ebible.org (arb-vd)         | `ebible.org/Scriptures/arb-vd_html.zip` |
| Tamil      | ebible.org (tam2017)        | `ebible.org/Scriptures/tam2017_html.zip` |
| Belarusian | ebible.org (bel)            | `ebible.org/Scriptures/bel_html.zip` |

### Adding a new language

1. Add a row to the Languages table in `CLAUDE.md` with the language code, display name, Bible version, abbreviation, and direction.
2. Add the source to the Lookup Sources table in `CLAUDE.md`.
3. For each of the 25 scriptures, fetch the verse text from the source and add the language entry to `translations.json`.
4. Add the language to the `LANGUAGES` array in `index.html`.

### Adding a new scripture

1. Add a row to the Scriptures table in `CLAUDE.md` with the letter, reference, version, and key text.
2. Add a new entry to `translations.json` with the reference and all language translations fetched from official sources.

## UI Design

- **Minimal, print-first**: The app is styled to look clean on screen and produce a well-formatted handout on paper.
- **No dependencies**: Zero npm packages, no build step, no framework. One HTML file + one JSON file.
- **Georgia serif** for verse text (readability across scripts), **system-ui sans-serif** for UI chrome.
- **Print stylesheet** hides all UI elements and formats the handout for US Letter paper with appropriate margins.
