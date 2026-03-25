---
name: scripture-alphabet-translations
description: Produces a multilingual Scripture Alphabet translation page for a given letter (A–Z) from the church-based ESL ministry PDF. Use this skill whenever the user says "make letter X", "do letter X", "proceed with letter X", or any similar phrasing referencing a letter of the Scripture Alphabet. Also use when the user says "next letter" or references the ESL Scripture Alphabet project. Always look up actual Bible translations from verified sources — never self-translate.
---

# Scripture Alphabet Translation Skill

Generates a verified, multilingual translation page for a single letter from the Scripture Alphabet guide used in church-based ESL ministries. Each page contains the verse for that letter translated into 7 languages, sourced from actual Bible translation databases.

---

## Source Document

The verses come from the **Scripture Alphabet PDF** (`ScriptureAlphabet.pdf`) uploaded to this project. Each letter maps to a specific verse and Bible version (usually GNT). Always check the PDF first to confirm the correct verse for the requested letter.

### Full Verse Index (from PDF)
| Letter | Verse | Reference | Version |
|--------|-------|-----------|---------|
| A | Ask and you will receive | Matthew 7:7 | GNT |
| B | Be kind and tenderhearted to one another | Ephesians 4:32 | GNT |
| C | Come to me … and I will give you rest | Matthew 11:28 | GNT |
| D | Do for others what you want them to do for you | Matthew 7:12 | GNT |
| E | Every good gift … comes from heaven | James 1:17 | GNT |
| F | Forgive us our sins, for we forgive everyone who does us wrong | Luke 11:4 | GNT |
| G | God is love | 1 John 4:8 | GNT |
| H | Happy is the one who reads this book | Revelation 1:3 | GNT |
| I | If we confess our sins to God … he will forgive us our sins | 1 John 1:9 | GNT |
| J | Jesus Christ is the same yesterday, today, and forever | Hebrews 13:8 | GNT |
| K | Keeping God's commands is what counts | 1 Corinthians 7:19 | NIV |
| L | Love your neighbor as you love yourself | Mark 12:31 | GNT |
| M | My help will come from the Lord | Psalm 121:2 | GNT |
| N | Not my will … but your will be done | Luke 22:42 | GNT |
| O | Obey your leaders and follow their orders | Hebrews 13:17 | GNT |
| P | Pray for one another | James 5:16 | GNT |
| Q | Quietly trust in me | Isaiah 30:15 | GNT |
| R | Remember your Creator while you are still young | Ecclesiastes 12:1 | GNT |
| S | Speak, Lord, your servant is listening | 1 Samuel 3:9 | GNT |
| T | Teach children how they should live, and they will remember it all their life | Proverbs 22:6 | GNT |
| U | Unless he is born again no one can see the kingdom of God | John 3:3 | NIV |
| W | Worship no god but me | Exodus 20:3 | GNT |
| X | Examine me and test me, Lord | Psalm 26:2 | GNT |
| Y | You are like salt for the whole human race | Matthew 5:13 | GNT |
| Z | Zacchaeus … welcomed him with great joy | Luke 19:6 | GNT |

---

## Output Format

Create a `.md` file saved to `/mnt/user-data/outputs/Scripture_Letter_[X]_Translations.md`.

### Template

```
# Scripture Alphabet - Letter [X]
## [Book Chapter:Verse]

### English - Good News Translation (GNT)
[verse text]
**Reference:** [Book Chapter:Verse], GNT

---

### Español (Spanish) - Nueva Versión Internacional (NVI)
[verse text]
**Referencia:** [Book Chapter:Verse], NVI

---

### 繁體中文 (Traditional Chinese) - 中文和合本 (CUV)
[verse text]
**經文出處：** [Book Chapter:Verse]，和合本

---

### Português (Portuguese) - Nova Versão Internacional (NVI)
[verse text]
**Referência:** [Book Chapter:Verse], NVI

---

### Italiano (Italian) - Nuova Riveduta (NR)
[verse text]
**Riferimento:** [Book Chapter:Verse], NR

---

### Français (French) - Louis Segond (LSG)
[verse text]
**Référence:** [Book Chapter:Verse], LSG

---

### አማርኛ (Amharic) - Amharic Bible
[verse text]
**ማጣቀሻ:** [Book Chapter:Verse], Amharic Bible
```

---

## Language & Version Reference

| Language | Version | Abbreviation |
|----------|---------|--------------|
| English | Good News Translation | GNT |
| Spanish | Nueva Versión Internacional | NVI |
| Traditional Chinese | Chinese Union Version (中文和合本) | CUV |
| Portuguese | Nova Versão Internacional | NVI |
| Italian | Nuova Riveduta | NR |
| French | Louis Segond | LSG |
| Amharic | Amharic Bible (Bible Society of Ethiopia) | — |

---

## Step-by-Step Process

### 1. Identify the verse
- Check the verse index table above for the letter requested.
- If the PDF is available in uploads (`/mnt/user-data/uploads/ScriptureAlphabet.pdf`), confirm by reading it.

### 2. Look up each translation — NEVER self-translate
Search for each language version using web search. Use targeted queries like:
- `[Book Chapter:Verse] NVI Spanish`
- `[Book Chapter:Verse] Nova Versão Internacional Portuguese`
- `[Book Chapter:Verse] Chinese Traditional CUV 和合本`
- `[Book Chapter:Verse] Nuova Riveduta Italian`
- `[Book Chapter:Verse] Louis Segond French`
- `[Book Chapter:Verse] Amharic Bible`

Reliable sources:
- **biblestudytools.com** — supports NVI, CUV, LSG, RIV
- **bibleserver.com** — supports NVI, NR, LSG
- **bibliatodo.com** — Portuguese NVI
- **bo.net.br** — Portuguese NVI
- **saintebible.com** — French LSG
- **wordproject.org** / **bibleonline.geezexperience.com** — Amharic

### 3. Verify before using
- Confirm the source URL clearly attributes the correct version.
- If search results conflict, prefer the most-cited source.
- If a translation cannot be verified, note it and try an alternative source before giving up.

### 4. Create the file
- Use the template above exactly.
- Save to `/mnt/user-data/outputs/Scripture_Letter_[X]_Translations.md`.
- Present the file with `present_files`.

---

## Important Rules

- **Never translate text yourself.** Always find the actual published Bible translation.
- **No "full verse" / "shortened version" labels** — just the verse text and reference.
- The verse index table above is the authoritative source. Do not substitute a different verse for a letter.
- Chinese characters require a CJK-capable font to print. If the user reports printing issues with Chinese, offer to recreate the document as `.docx` or `.html` for better font support.
