# ESOL Multi-language Bible Scripture Handout Generator

A static single-page web app for ESOL ministry that generates printable multilingual Bible scripture handouts. Deployable on GitHub Pages.

## How it works

- `index.html` — single-file SPA (HTML + CSS + JS, no build step, no dependencies)
- `translations.json` — 25 scriptures (A–Z minus V) x 7 languages
- User picks a letter and languages, previews the handout, prints or saves as PDF via the browser

## Key files

- `SCRIPTURES.md` — ground truth for letter-to-verse mapping
- `LANGUAGES.md` — ground truth for supported languages and Bible versions
- `DESIGN.md` — full architecture and translation sourcing process

## Translation sourcing

All translations in `translations.json` are copied verbatim from official published Bible versions — never AI-generated or machine-translated. See `DESIGN.md` for the exact sources and verification process.

| Language   | Version                     | Source                    |
|------------|-----------------------------|---------------------------|
| English    | Good News Translation (GNT) | biblestudytools.com       |
| Spanish    | Nueva Version Int'l (NVI)   | bibleserver.com           |
| Chinese    | Chinese Union Version (CUV) | biblestudytools.com       |
| Portuguese | Nova Versao Int'l (NVI)     | bibliatodo.com            |
| Italian    | Nuova Riveduta 2006 (NR)    | biblegateway.com (NR2006) |
| French     | Louis Segond (LSG)          | saintebible.com           |
| Amharic    | Amharic Bible (ABB)         | wordproject.org           |

