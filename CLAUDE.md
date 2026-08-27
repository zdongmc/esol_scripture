# ESOL Multi-language Bible Scripture Handout Generator

A static single-page web app for ESOL ministry that generates printable multilingual Bible scripture handouts. Deployable on GitHub Pages.

See DESIGN.md

## How it works

- `index.html` — single-file SPA (HTML + CSS + JS, no build step, no dependencies)
- `translations.json` — 25 scriptures (A–Z minus V) x 17 languages
- User picks a letter and languages, previews the handout, prints or saves as PDF via the browser

## Key files

- `DESIGN.md` — full architecture and translation sourcing process

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
| W      | Exodus 20:3      | GNT     | Worship no god but me                                                              |
| X      | Psalm 26:2       | GNT     | Examine me and test me, Lord                                                       |
| Y      | Matthew 5:13     | GNT     | You are like salt for the whole human race                                         |
| Z      | Luke 19:6        | GNT     | Zacchaeus … welcomed him with great joy                                            |

- V is intentionally absent from the original Scripture Alphabet source document.
- Key Text is the English prompt phrase, not necessarily the full verse. Full verse text lives in `translations.json`.
- Version listed is the English source version. Each language uses its own canonical version (see Languages section below).

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
- **Verification status:** en, es, zh-tw, pt, it, fr, am, uk, ar, be verified verse-by-verse. The other seven (hi, te, fa, zh-cn, ko, vi, ta) have **not** been — see `SOURCES.md`.
- **Licensing:** Belarusian (Bokun) is CC BY-ND 4.0 — it may not be modified, and needs fuller attribution than the `BEL` label currently gives. Every other language is public domain or permissively licensed. See `SOURCES.md`.

### Lookup Sources by Language

All translations in `translations.json` are copied verbatim from official published Bible versions — never AI-generated or machine-translated.

**See [`SOURCES.md`](SOURCES.md)** for the verified per-language URL pattern, parsing rules, access terms, and the sources tested and rejected. It also records which languages have been verified verse-by-verse and which still need a pass.

`DESIGN.md` describes the overall verification process.
