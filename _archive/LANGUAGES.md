# LANGUAGES

Ground truth for languages supported in the ESOL handout generator.
Each entry defines the language code, display name, Bible version, and text direction.

> To add or edit a language, update this file and regenerate `translations.json`.

| Code  | Display Name              | Bible Version                          | Abbreviation | Direction |
|-------|---------------------------|----------------------------------------|--------------|-----------|
| en    | English                   | Good News Translation                  | GNT          | LTR       |
| es    | Español (Spanish)         | Nueva Versión Internacional            | NVI          | LTR       |
| zh-tw | 繁體中文 (Traditional Chinese) | 中文和合本 (Chinese Union Version)     | CUV          | LTR       |
| pt    | Português (Portuguese)    | Nova Versão Internacional              | NVI          | LTR       |
| it    | Italiano (Italian)        | Nuova Riveduta                         | NR           | LTR       |
| fr    | Français (French)         | Louis Segond                           | LSG          | LTR       |
| am    | አማርኛ (Amharic)            | Amharic Bible (Bible Society of Ethiopia) | —         | LTR       |

## Notes
- `code` is the key used in `translations.json` and in the web app.
- All languages currently use LTR layout. RTL languages (Arabic, Hebrew, Urdu) would need `direction: RTL`.
- To add a language: add a row here, add a source URL to the lookup table below, then populate `translations.json`.

## Lookup Sources by Language

| Language | Reliable Sources                                          |
|----------|-----------------------------------------------------------|
| Spanish  | biblestudytools.com, bibleserver.com                      |
| Chinese  | biblestudytools.com (CUV), bible.com                      |
| Portuguese | bibliatodo.com, bo.net.br                               |
| Italian  | bibleserver.com (NR)                                      |
| French   | saintebible.com (LSG), bibleserver.com                    |
| Amharic  | wordproject.org, bibleonline.geezexperience.com           |
