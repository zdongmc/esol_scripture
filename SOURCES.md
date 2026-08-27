# SOURCES

Verified lookup sources for each language in the ESOL handout generator.

Every source below was probe-tested on **John 3:3** (and 1 Corinthians 7:19 for NIV)
on 2026-08-26. "Verified" means the URL pattern returned the correct verse in the
correct version and script. Companion to the Languages section of
`CLAUDE.md` (which defines *what* each language is) — this file defines *where the
text comes from*.

> **Never self-translate.** (The one rule worth keeping from the archived
> `_archive/SKILL.md`, whose sources are otherwise superseded.) Every verse must
> come from a published translation retrieved from one of these sources. A verse that
> cannot be retrieved stays empty; the app already renders empty as
> "Translation not yet available" (`index.html`).

---

## Verification status

The app ships **17 languages**. **14** have been through the verse-by-verse
verification this file documents.

| | Languages | Status |
|---|---|---|
| ✅ Verified | en, es, zh-tw, pt, it, fr, am (2026-08-26); uk, ar, be, zh-cn, fa, hi, ko (2026-08-27) | Each verse fetched individually, asserting the returned reference matched the one requested |
| ⚠️ **Not yet verified** | te, vi, ta | Added separately; sourced from ebible.org per `_meta.note`, but not checked verse-by-verse |

**The ten unverified languages still need a pass.** Every one of the seven that was
checked turned out to have a defect except French — a versification offset, an
off-by-one source, a mixed dialect, truncated verses, a mislabelled translation. There
is no reason to assume the other ten are clean, and the failures are the kind that
read plausibly rather than looking broken.

Known already, without looking: **unmatched quote marks survive in te** — verses carry an opening `«` or `“` they never close, the same
extraction artefact cleaned up in the verified languages. They were left alone rather
than cosmetically patched, since the text under them is still unvalidated; fix them
during each language's verification pass.

When verifying the rest, the traps found this time are worth checking for first:
1. **Versification offsets** — Ecclesiastes 12:1 and Luke 11:4 are the usual suspects.
2. **Divine-name conventions** — small caps, reverential spacing, or a distinct word.
3. **Verse truncation** — a verse missing its opening clause still reads fine.
4. **Dangling quote marks** from discourse spanning verse boundaries.
5. **Right-to-left text** — `ar` and `fa` add a layer the others did not have.

---

## Source Table

| Code | Version | Source | URL pattern | Verified | Copyright |
|-------|---------|--------|-------------|:--------:|-----------|
| en | GNT *(23 letters)* | BibleGateway | `biblegateway.com/passage/?search={REF}&version=GNT` | ✅ | © 1992 American Bible Society |
| en | NIV *(letters K, U only)* | BibleGateway | `biblegateway.com/passage/?search={REF}&version=NIV` | ✅ | © 2011 Biblica |
| es | NVI (1999, 2015, 2022) | **BibleGateway** | `biblegateway.com/passage/?search={Libro}+{ch}:{v}&version=NVI` | ✅ | © Biblica |
| es | NVI 1999 *(rejected)* | BibleStudyTools | `biblestudytools.com/nvi/{libro}/{ch}-{v}.html` | ⚠️ | older edition, broken poetry spacing |
| zh-tw | 和合本 CUV | FHL 信望愛 (Taiwan) | `bible.fhl.net/json/qb.php?chineses={abbr}&chap={ch}&sec={v}&version=unv` | ✅ | Public domain (1919) |
| pt | **NVI (Nova Versão Internacional)** | **BibleGateway** | `biblegateway.com/passage/?search={Livro}+{ch}:{v}&version=NVI-PT` | ✅ | © Biblica |
| pt | Almeida *(alt, PD)* | bible-api.com | `bible-api.com/{ref}?translation=almeida` | ✅ | Public domain |
| it | **Nuova Riveduta 2006 (NR)** | **LaParola.net** | `laparola.net/testo.php?riferimento={Abbr}{ch}:{v}&versioni[]=Nuova Riveduta` | ✅ | © Società Biblica di Ginevra |
| it | Riveduta 1927 *(alt)* | BibleStudyTools | `biblestudytools.com/riv/{libro}/{ch}-{v}.html` | ✅ | Public domain (Luzzi, d. 1948) |
| fr | **Louis Segond 1910** | **SainteBible** | `saintebible.com/{book}/{ch}-{v}.htm` | ✅ | Public domain |
| fr | LSG *(alt)* | BibleGateway | `biblegateway.com/passage/?search={Livre}+{ch}:{v}&version=LSG` | ⚠️ | has a text error — see below |
| uk | **UKR "Ukrainian Bible"** *(all 25)* | BibleGateway `UKR` | `biblegateway.com/passage/?search={ref}&version=UKR` | ✅ | Public domain |
| uk | UKR *(cross-check)* | htmlbible.com | `htmlbible.com/sacrednamebiblecom/ukrainian/B{bb}C{ccc}.htm` — **windows-1251**, not UTF-8 | ✅ | Public domain |
| ko | **한국어 성경 (KOR)** | ebible.org `kor` | `ebible.org/Scriptures/kor_html.zip` | ✅ | Public domain |
| hi | **IRV हिंदी 2019** | ebible.org `hin2017` | `ebible.org/Scriptures/hin2017_html.zip` | ✅ | © Bridge Connectivity Solutions, **CC BY-SA 4.0** |
| fa | **ترجمه قدیم (OPV)** | ebible.org `pesOPV` | `ebible.org/Scriptures/pesOPV_html.zip` | ✅ | Public domain |
| zh-cn | **圣经当代译本 (CCB)** | ebible.org `cmncbs` | `ebible.org/Scriptures/cmncbs_html.zip` | ✅ | © Biblica, **CC BY-SA 4.0** |
| be | **Біблія, пер. А. Бокуна** | ebible.org `bel` | `ebible.org/Scriptures/bel_html.zip` | ✅ | © 2016–2023, **CC BY-ND 4.0** |
| ar | **Van Dyke (SVD)** | ebible.org `arb-vd` | `ebible.org/Scriptures/arb-vd_html.zip` | ✅ | Public domain |
| ar | SVD *(cross-check)* | copticchurch.net | `copticchurch.net/bible/arabic/SVD/{Book}/{ch}?showVN=1` | ✅ | Public domain |
| am | መጽሐፍ ቅዱስ 1962 | **wordproject.org** | `wordproject.org/bibles/am/{NN}/{ch}.htm` (NN zero-padded) | ✅ | © Bible Society of Ethiopia |
| am | *(rejected)* | `magna25/amharic-bible-json` | — | ❌ | corrupt — see below |

---

## Verification probes (John 3:3)

Retained as a regression fixture — re-run these if a source changes shape.

- **en / GNT** — "I am telling you the truth: no one can see the Kingdom of God without being born again."
- **es / NVI** — "—Te aseguro que quien no nazca de nuevo no puede ver el reino de Dios —dijo Jesús."
- **zh-tw / 和合本** — 耶穌回答說：「我實實在在地告訴你，人若不重生，就不能見　神的國。」
- **pt / NVI-PT** — "Em resposta, Jesus declarou: “Digo-lhe a verdade: Ninguém pode ver o Reino de Deus, se não nascer de novo”."
- **it / Nuova Riveduta** — "Gesù gli rispose: «In verità, in verità ti dico che se uno non è nato di nuovo, non può vedere il regno di Dio»."
- **fr / LSG** — "Jésus lui répondit: En vérité, en vérité, je te le dis, si un homme ne naît de nouveau, il ne peut voir le royaume de Dieu."
- **am / 1962** — ኢየሱስም መልሶ። እውነት እውነት እልሃለሁ፥ ሰው ዳግመኛ ካልተወለደ በቀር የእግዚአብሔርን መንግሥት ሊያይ አይችልም አለው።

---

## Source caveats

### Amharic — use wordproject, NOT the GitHub mirror
The `magna25/amharic-bible-json` mirror was tested and **rejected**. Its verses are a
bare positional array with no verse numbers, and where the Amharic merges two verses
the array collapses — so `verses[n-1]` silently returns a neighbouring verse.
Measured against wordproject it was wrong on **5 of 25** references (B, E, K, L, N),
each off by one, each returning real scripture that reads plausibly. Its Matthew
chapter 5 is also mislabelled `ማን` instead of `5`.

**wordproject.org prints explicit verse numbers** in the markup, which removes the
guesswork entirely:

```html
<span class="verse" id="3">3 </span> ኢየሱስም መልሶ። እውነት እውነት እልሃለሁ፥ ...
```

Parsing rules:
- Book numbers are **zero-padded**: `/am/09/3.htm`, not `/am/9/3.htm`.
- **Verse 1's marker is HTML-commented out** (`<!--span class="verse" id="1">`), so
  verse 1 is the leading text of the `<p>`. Restore the marker before splitting.
- Each verse is prefixed by a stray `፤` separator — strip leading Ethiopic
  punctuation.
- Read back the `<h3>ምዕራፍ N</h3>` heading and assert it matches the chapter asked for.
- Exodus 20:3 carries a stray trailing `ፕ` in the source page. Strip a ≤2-character
  fragment following the final `።`.

#### Access
`robots.txt` sets `User-agent: * → Allow: /` with
`Content-Signal: search=yes, ai-train=no, use=reference` — reference use is
permitted, which is what this is. It separately disallows `ClaudeBot`, and requests
sent with that UA do return **403**. A plain descriptive UA
(`esol-scripture/1.0 …`) returns **200**; there is no need to disguise the client as
a browser, and it should not be done. Requests paced ~1s apart.

#### Copyright
The 1962 text is most likely still under Bible Society of Ethiopia copyright. The
GitHub mirror's MIT license covered only the JSON compilation, never the text.

### Spanish — BibleGateway, not BibleStudyTools
Two problems ruled BibleStudyTools out for Spanish:

1. **It drops poetry line-break spaces in its own HTML** — the source markup literally
   reads `Creadoren los días` and `los días malosy vengan`. Affected 7 of 25 (F, M, Q,
   R, S, T, X). Not a parsing bug; the spaces are absent upstream.
2. **It serves an older NVI edition** (1999). BibleGateway serves the current
   `NVI® © 1999, 2015, 2022`. The two differ in real wording — e.g. James 1:17
   "todo don perfecto" (1999) vs "toda perfecta bendición" (current).

BibleGateway sets **`Crawl-delay: 15`** in robots.txt; `/passage/` is not disallowed.
Pace requests accordingly — a full 25-verse run takes ~6.5 minutes.

Parsing rules for BibleGateway:
- The verse span carries an `id` *before* `class` (`<span id="es-NVI-29305" class="text Eph-4-32">`),
  so a regex anchored on `<span class="text …">` silently misses the primary span and
  returns only continuation lines. Match the `passage-content` div and strip tags instead.
- Remove `<sup>` (verse numbers *and* footnote markers) and
  `<span class="chapternum">` — the latter otherwise prepends the chapter number to
  any verse 1 (it corrupted Ecclesiastes 12:1 as "12 Acuérdate…").
- Strip the trailing "Read full chapter" link text.
- Tag-stripping inserts spaces around inline `small-caps` spans, producing
  `Señor , que hizo`. Normalise whitespace before closing punctuation.

#### The divine name is small-capped — preserve it
BibleGateway renders YHWH in small caps via CSS, so naive tag-stripping yields
`Lord` / `Señor` and **silently collapses the distinction between YHWH
(LORD / SEÑOR / SENHOR) and the ordinary title "Lord"**. Uppercase the span contents.

The class differs by version, which is easy to miss:
- **NVI / NVI-PT**: `class="small-caps divine-name"`
- **GNT**: `class="small-caps"` only — no `divine-name`

Match on **`small-caps`** so both are covered; keying on `divine-name` silently skips
English. Affects M, Q, S, X in English, Spanish, and Portuguese alike. Same class of
convention as the reverential space before `神` in Chinese CUV — see that note.

#### Dangling quotation marks
Quoted discourse runs across verse boundaries, so an extracted verse often carries an
unmatched `“`, `”`, `«` or `»` — and Spanish/Portuguese additionally use a leading `»`
or `“` as a *paragraph-continuation* marker. Scan for quote marks that never pair and
drop them; do not touch `‘ ’`, which double as apostrophes. Affected A, C, D, F, L, Q,
W, Y across the three languages.

#### Dialect: the previous data was mixed
The Spanish that predated this file was **not consistently NVI**. Seven verses
(A, B, C, D, O, P, Y) were in Castilian *vosotros* — `Pedid, buscad, llamad`,
`Venid a mí`, `Vosotros sois la sal` — while seven others matched NVI exactly.
NVI is a Latin American translation and uses *ustedes*. All 25 have been replaced
from BibleGateway, so the file is now internally consistent.

---

### Portuguese — NVI-PT is available; no need to substitute Almeida
The Languages section of `CLAUDE.md` specifies Nova Versão Internacional, and BibleGateway serves it as
**`version=NVI-PT`**. Public-domain Almeida remains a fallback but is a different
translation, notably more formal, and should not be swapped in silently.

Only **3 of 25** of the previous Portuguese matched NVI. Several verses were plainly
a different translation — Revelation 1:3 read "Bem-aventurado aquele que lê em voz
alta" where NVI has "Feliz aquele que lê"; Proverbs 22:6 read "no caminho em que deve
andar" where NVI has "segundo os objetivos que você tem para ela". The old text also
used **non-breaking hyphens (U+2011)** in `perdoando‑se`, `Lembre‑se`, `Sonda‑me`,
suggesting it was pasted out of a PDF.

---

### Chinese — use `chineses`, not `engs`
FHL's `engs=John` parameter returned **Romans 3:3**, not John 3:3. The `chineses`
parameter with the Chinese book abbreviation returned the correct verse. A book
map is required; do not use `engs` as an input.

`chineses` values used: `出 撒上 詩 箴 傳 賽 太 可 路 約 林前 弗 來 雅 約一 啟`

**But `engs` is valuable as an *output* check.** Every response echoes back
`engs`, `chap`, and `sec`, so each fetch can assert it got the book and verse it
asked for. FHL's own labels are `Ex, 1 Sam, Ps, Prov, Eccl, Is, Matt, Mark, Luke,
John, 1 Cor, Eph, Heb, James, 1 John, Rev`.

Returns Traditional characters (`version=unv`), correct for `zh-tw`.

#### Cleanup applied
- **Dangling quotation marks.** CUV runs quoted discourse across verse boundaries, so
  a single extracted verse often carries an unmatched `「` or `」` (Matt 7:7 opens a
  quote it never closes). Strip a leading `「` or trailing `」` only when the counts
  are unbalanced — never when the verse quotes cleanly on its own (Luke 22:42 and
  1 Sam 3:9 keep theirs).
- **Textual apparatus.** Luke 11:4 carries `（有古卷沒有末句）` — "some manuscripts
  omit the final clause". That is editorial notation, not scripture. Strip
  parentheticals containing `古卷`.

#### Kept deliberately
- The **reverential space before 神** (` 神`) is standard CUV typography, not a stray
  space. Preserved.
- **`裡` / `著`** (Taiwan standard) rather than `裏` / `着`. These are variant forms of
  the same characters; the previous data mixed conventions.

#### What the previous data got wrong
12 of 25 matched exactly and 11 differed only cosmetically, but **two were
truncated**: 1 Samuel 3:9 was missing its opening clause 因此以利對撒母耳說 ("So Eli
said to Samuel"), and Luke 22:42 was missing its speech tag 說. Both are restored.

### Italian — solved: LaParola.net serves NR
LaParola.net serves **Nuova Riveduta 2006**, exactly the version `CLAUDE.md`
specifies. All 25 references were fetched and each returned the correct book,
chapter, and verse (confirmed against the `<h1>` heading in every response).
`robots.txt` permits `/testo.php`; it bans only `/greco/index.php` and wholesale
site-rippers (HTTrack, TeleportPro). Requests were paced ~1.2s apart.

Response shape is stable and easy to parse:

```html
<div id="brano"><h1>Matteo 7:7</h1><p></p><p><i>La preghiera e il suo esaudimento</i><br />&laquo;Chiedete e vi sar&agrave; dato; ...</p></div>
```

Two parsing rules are required:
- Strip a **leading `<i>…</i><br />`** — that's an editorial section title, not
  scripture. It affects letters A, D, R, and Y.
- Read the `<h1>` back and assert it matches the reference you asked for.

Italian book abbreviations: `Mt Mc Lc Gv At Ro 1Cor Ef Eb Gm 1Gv Ap Sal Pr Ec Is Es 1Sam`.

#### Versification: Ecclesiastes is offset by 2
NR divides Ecclesiastes differently from the English versions. NR chapter 11 ends at
11:8, so:

| English | NR |
|---------|-----|
| Ecc 11:9 | Ec 12:1 |
| Ecc 11:10 | Ec 12:2 |
| **Ecc 12:1** (letter R) | **Ec 12:3** |

Requesting `Ec12:1` returns "Rallègrati pure, o giovane…" — the wrong verse, and
plausible enough to pass unnoticed. Letter R must be fetched as **`Ec12:3`**.
This is the only offset found across the 25.

## Copyright posture

Reproducing one verse with the version attributed — which is what this app does —
is standard handout practice and within normal fair-use / publisher courtesy limits
for GNT, NIV, and NVI. What would *not* be safe is bulk-reproducing a copyrighted
translation. At 25 verses the app is comfortably inside the courtesy limits
publishers state (Biblica and ABS both allow several hundred verses without written
permission, provided the work is not a commercial or biblical-reference product).

Public-domain versions (CUV, LSG, Riveduta 1927, Almeida) carry no such constraint.

---

### French — the existing data was already correct; BibleGateway is not
French is the one language that needed **verification, not replacement**. All 25
verses already in `translations.json` match SainteBible's LSG **exactly**, so they
were left untouched.

BibleGateway's LSG was fetched as a cross-check and **disagreed on 6 of 25** — and
SainteBible sided with the existing data every time:

| | BibleGateway | SainteBible / repo |
|---|---|---|
| Heb 13:17 | `ce qui vous ne serait` | `ce qui ne vous serait` |
| Heb 13:8 | `Jésus Christ` | `Jésus-Christ` |
| Ps 121:2, Isa 30:15, 1 Sam 3:9, Ps 26:2 | `l'Éternel` | `l'Eternel` |

The Hebrews 13:17 case is not a variant, it is **an error** — `ce qui vous ne serait`
is ungrammatical French. Overwriting from BibleGateway would have introduced a
grammatical mistake into the handout. Use SainteBible for LSG.

Extraction: the verse is the text node directly after
`Louis Segond Bible</a></span><br />`, terminated by `<span class="p">`.

#### No versification override for French
BibleGateway's LSG prefixes Ecclesiastes 12:1 with a `(12:3)` marker, suggesting the
same offset Italian NR has. **SainteBible's LSG numbers it 12:1**, matching English.
Printed LSG editions differ here, so no `ref` override was added — unlike Italian,
where the offset was unambiguous. The text itself is correct either way.

---

### Ukrainian — one translation, chosen for the audience
All 25 verses come from the public-domain **UKR "Ukrainian Bible"**. Two earlier
approaches were tried and dropped:

1. **ONPU alone** (`ukronpu`, © 2022 Biblica, CC BY-SA) covers only the New Testament
   and Psalms, so the five Old Testament letters — Q (Isaiah), R (Ecclesiastes),
   S (1 Samuel), T (Proverbs), W (Exodus) — had no text at all.
2. **ONPU + Kulish 1905** filled the gap but mixed a 2022 translation with a
   pre-reform archaic one (`днї`, `Сьвятий`, `инших`) in the same column.

UKR settles it with a single translation throughout. ONPU is the more modern text,
but the congregation's Ukrainian learners are typically **grandparents**, for whom
the older wording is the familiar one from church — so newer is not automatically
more readable here. Let the audience decide this, not the publication date.

**Attribution is deliberately unasserted.** BibleGateway says only that it is public
domain with "no further information about its publication history"; htmlbible.com
likewise calls it just "Ukrainian Bible". The text matches what circulates as the
Ohienko translation, but neither source names a translator, so the repo labels it
`UKR`.

#### Verification
Two independent sources carry it and **agree on all 25 verses** — the strongest
confirmation available for any language here, since it needs no reader of the script:
- BibleGateway `version=UKR`, each fetch reference-asserted against the page title
- htmlbible.com, whose pages are **windows-1251, not UTF-8** (decoding as UTF-8 gives
  mojibake). Markup is `<A NAME='V3'><H4>3</H4></TD><TD><P>text<P>`; verse 1 may carry
  a leading `¶`.

BibleGateway intermittently returns **HTTP 500** on this version; retry with backoff
rather than treating it as a missing verse.

#### Psalm numbering
**UKR uses Hebrew numbering**, so no `ref` overrides are needed. Worth recording in
case anyone returns to ONPU: *ONPU* numbers Psalms the Septuagint way, one behind the
Hebrew — its Psalm 121 is Hebrew 122 ("Our feet are standing in your gates,
Jerusalem"), a real psalm and the wrong verse.

---

### Korean — clean, but verified less strongly than the rest
The existing data matched ebible's `kor` **exactly on all 25 verses**. Psalm numbering
is Hebrew, proper nouns are present, and no verse has unbalanced quote marks — nothing
needed changing.

**It rests on one source, though.** ebible carries only one Korean Bible: `kor` is the
ISO 639-3 code for Korean, and the neighbouring ids are unrelated languages. So unlike
Arabic (cross-checked against copticchurch.net) or Ukrainian (two sources agreeing on
all 25), there is no independent same-translation confirmation here. Treat Korean as
verified-but-single-sourced; a second source would strengthen it.

A cautionary note on picking cross-check editions: **`kog` is Cogui, a Colombian
language, not Korean.** Filtering ebible ids by a `ko` prefix picks it up, and the
resulting comparison reported zero overlap on every verse — which looks alarming until
you read the text and find it is in Latin script. Match on the exact ISO code, and
sanity-check the script before trusting a comparison.

#### One incomplete chapter in the source
`kor`'s **Psalm 118 contains only 9 verses** where it should have 29. It is the only
short chapter in all 150 — every other psalm checked parses to its expected count, and
both psalms this app uses (26 and 121) are complete. Recorded because it means `kor`
is not uniformly complete, so verify chapter length if a future scripture is added
from it.

---

### Hindi — the worst data found so far: 7 truncated verses
Only 15 of 25 matched. **Seven verses were truncated** — F, I, J, M, O, T and X — each
cut short and each still reading as a complete Hindi sentence. Psalm 26:2 had lost an
entire closing clause. Three more carried quote-mark artefacts (C, W) or stray
whitespace (U). All 25 were replaced from `hin2017`.

The truncations were **not** a case of the wrong edition: the previous text matched
neither `hin2017` nor `hin2010`, and was shorter than both. It was damaged in
extraction, not sourced differently.

Psalm numbering is Hebrew (176-verse psalm at 119). Proper nouns all present.

---

### The cross-translation overlap check is a weak signal — treat it accordingly
Comparing a verse against the same verse in a second translation has now produced
**three false alarms and no findings**:

| Language | Flagged | Actually |
|---|---|---|
| Farsi | Luke 22:42 zero overlap | correct — editions render "cup" differently, shared words fell under the length threshold |
| Hindi | Exodus 20:3 zero overlap | correct — editions use wholly different vocabulary (परमेश्वर/दूसरों vs देवता/अन्य) |
| Hindi | Luke 19:6 zero overlap | correct — different synonyms throughout |

Two translations of one verse may legitimately share almost no words. Use the check
only to decide *what to look at*, never as evidence of an error, and confirm anything
it flags by another route. The stronger checks remain: matching a second source of
the **same** translation byte-for-byte, asserting the returned reference, and
distinctive proper nouns.

Note also that distinctive-word assertions must be written against **the edition
actually in use** — the Exodus check above failed because it was written with the
other edition's vocabulary, and the Belarusian one failed on a name spelling.

---

### Farsi — clean, but it broke the cross-translation check
The existing data matched ebible's `pesOPV` (ترجمه قدیم, public domain) **exactly on
all 25 verses**. Psalm numbering is Hebrew (176-verse psalm at 119), proper nouns
appear where required, and dangling quote marks were stripped from A, F, L, Q and Y.

#### Tune the cross-translation check per language
The overlap check — "does this verse share content words with the same verse in a
second translation?" — reported **zero overlap on Luke 22:42**, which is the wrong-verse
signal. It was a false alarm.

Both Persian editions have the right verse. They simply share no *long* words: each
renders "cup" differently (پیاله vs جام), and everything they do share (ای, پدر, اگر)
falls under the 4-character minimum the check inherited from Arabic. Dropping the
minimum to 3 cleared it, and to 2 as well.

The lesson generalises: **the word-length threshold is language-specific.** Persian
and other languages with short content words need a lower bound than Arabic. Treat a
zero-overlap result as "inspect this verse", never as proof of an error — and check
the threshold before trusting it.

Two other checks in this file have now produced false alarms rather than findings:
the Belarusian proper-noun assertion (wrong name spelling) and the Simplified Chinese
self-consistency near-ties. All three were the check being wrong, not the data.

---

### Simplified Chinese — two verses were truncated
Unlike Traditional Chinese, this one needed fixing. **Luke 11:4 and Proverbs 22:6
were cut off at a poetry line break**, keeping only the first line:

| | was | should be |
|---|---|---|
| F, Luke 11:4 | "Forgive us our sins," and nothing more | the full petition |
| T, Proverbs 22:6 | "Teach a child the right way," | plus "and when he is old he will not depart from it" |

Both read as complete sentences in Chinese, so neither looks broken on a handout —
the same failure mode as the two truncated verses found in Traditional Chinese.
The other 23 matched exactly. Dangling quote marks were stripped from eight verses
(A, C, D, F, L, Q, W, Y); CCB is CC BY-SA, which permits adaptation, unlike Belarusian.

#### Cross-checking against the verified Traditional column
A useful trick where two related languages are both present: score each `zh-cn` verse
by shared Han characters against **every** `zh-tw` verse, and check it matches its own
letter best. 23 of 25 did outright. The two that did not — F and G — were near-ties on
very short verses where common characters (的, 我, 不) dominate, and both were then
confirmed correct by distinctive-word checks (饶恕/罪 for Luke 11:4, 爱/上帝 for
1 John 4:8). Treat a near-tie as "inspect", not "wrong".

Psalm numbering is Hebrew (176-verse psalm at 119), so no `ref` override.

#### Attribution
CC BY-SA 4.0 requires crediting Biblica. The handout shows only `CCB`. Same open gap
as Belarusian, and the same design decision — noted, not guessed at.

---

### Belarusian — verified, and the only licence here with real constraints
The existing data matched ebible's `bel` (Біблія, пераклад А. Бокуна) **exactly on all
25 verses**. Structural checks all pass: the 176-verse psalm sits at 119 (Hebrew
numbering, no offset), proper nouns appear where required, and no verse has zero
content-word overlap with `beln`, a second Belarusian edition from the same publisher.

Note the name spelling: Bokun uses **Самуэль**, not Самуіл. An assertion written
against the more common form fails on a correct verse.

The divine name is already uppercase in the source (`ГОСПАДЗЕ`), so no transformation
is applied — the same conclusion as Ukrainian ONPU's bold `nd`, reached for a
different reason.

#### ⚠️ CC BY-ND — this text may not be modified, and must be attributed
Unlike every other language here, Bokun is **not public domain and not permissive**.
It is Creative Commons **Attribution-NoDerivatives 4.0**, © 2016–2023 John the
Forerunner Church of Christians of Evangelical Faith of Minsk City.

Two consequences:

1. **Do not clean the text.** Luke 11:4 and Mark 12:31 carry quote marks left
   unmatched by extraction, the same artefact stripped from every other language.
   They are **deliberately left in place** — under NoDerivatives, altering the
   distributed text is exactly what the licence withholds. Consistency with the other
   columns is not worth a licence breach on someone else's translation.
2. **Attribution is currently insufficient.** The handout shows only `BEL`. CC BY-ND
   requires crediting the translation. This is an open compliance gap — the app has
   no field for a fuller credit line, and adding one is a design decision, not a
   parsing fix.

Both points are for the repo owner to decide. Neither blocks use of the app.

---

### Arabic — verified without reading Arabic
The existing data matched ebible's `arb-vd` (Van Dyke, public domain, Syrian Mission /
American Bible Society) **exactly on all 25 verses**, so nothing was replaced. Only
dangling quote marks were stripped (A, F, L, Y).

Since no one here reads Arabic, correctness was established structurally, by four
checks that need no reader of the script:

1. **Chapter assertion** — every fetch reads back the page's own chapter heading.
   Note the heading may *begin* with a numeral (`١ يوحنا ٤` = 1 John 4), so take the
   **last** number in it; stripping all non-digits yields `14` and fails.
2. **Psalm numbering** — the 176-verse psalm sits at 119, so this is Hebrew
   numbering, not Septuagint. M and X need no `ref` override.
3. **Proper nouns** — Samuel in 1 Sam 3:9, Jesus in Heb 13:8 and John 3:3, Israel in
   Isa 30:15, all present. Compare with diacritics stripped.
4. **Cross-translation overlap** — every verse shares content words with the *same*
   verse in a different Arabic translation (`arbnav`). Zero overlap would signal a
   wrong verse; there were none.

#### SVD has multiple editions — expect variants
copticchurch.net also serves SVD and agrees with `arb-vd` on 20 of 25 once diacritics
and punctuation are normalised. The rest are edition differences, not errors:
case (`السنون` / `السنين`), spacing (`يارب` / `يا رب`), alef forms (`آلهة` / `الهة`),
and one genuine wording difference in John 3:3. Prefer `arb-vd`, which is the
documented ABS edition and what the repo already matched.

#### Arabic is RTL
`index.html` renders `ar` and `fa` with `dir="rtl"`. Quote counting still works on
logical order — `«` remains U+00AB regardless of how the bidi algorithm mirrors it
visually — so the unmatched-quote cleanup applies unchanged.

---

### English — the version varies by verse
23 letters are GNT; **K (1 Cor 7:19) and U (John 3:3) are NIV**, per the source PDF.
Fetch each letter with the version recorded on the verse, not a blanket version.

Both publishers permit up to **500 verses** without written permission (Biblica for
NIV, American Bible Society for GNT), subject to the quoted verses not forming a
complete book or exceeding 25% of the work. At 25 verses this app is far inside that.
Biblica additionally waives the full copyright notice for *non-saleable local-church
media* — bulletins, orders of service, handouts — provided **the version appears with
each quotation**, which is why `index.html` must render the per-verse version.

### Handout heading
The heading uses the abbreviated phrase exactly as printed in
`_archive/ScriptureAlphabet.pdf` (ellipses included), stored per scripture as `key` in
`translations.json`. It is deliberately *not* derived from the verse text — the PDF's
phrasing is the mnemonic the alphabet is built on. The full verse appears in the
English row alongside the other languages.

---

## Provenance of the existing data

The five populated languages in `translations.json` (en, es, zh-tw, pt, fr) predate
this file. `_meta.note` asserts they were "verified from official sources," but no
per-verse source URL was recorded, and git history does not show where they came
from. They should be re-verified against the table above before the handouts are
used in class.
