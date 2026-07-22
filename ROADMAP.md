# ILS — Italian course roadmap

Build plan for **Italiano come Lingua Straniera** (`ils`), the boulingua Italian
course, from the current empty scaffold (LICENSE + README + brand icons) to a
live, standards-conforming Hugo site with content, materials, native-voice audio
and VG Wort counting marks on every editorial page. Signature accent `#8023C7`
(light) / `#B97EE7` (dark), pentagon mark, already registered in
`pagegen/data/accents.yaml`.

---

## 1. Overview

**What it is.** A free, openly-licensed Italian course for the German
*Gesamtschule* system and independent learners, following the boulingua
five-step unit model (Activate → Input → Practise → Apply → Reflect), built by
instantiating `pagegen` and conforming to the `curriculum` descriptor framework.
Target learners: German-L1 secondary pupils and adult self-learners starting
from zero.

**CEFR scope (realistic).** Italian is a high-resource Latin-script Romance
language with abundant openly-licensed source material, so **full A1–C1** is
achievable. A short pre-A1 **"Suoni e ortografia"** onboarding precedes A1. C2 is
declared out of committed scope initially (freely-available C2 material is thin;
declare the gap honestly rather than fake it).

**Why Italian.** Completes the Romance triad alongside `fle`/`ele`; German
learners have strong transfer from cognates and shared Latinate vocabulary;
excellent openly-licensed corpus (Wikisource, LiberLiber public-domain
literature, Wikimedia audio) and a usable Piper TTS voice.

**Defining challenges (from the language context):**

1. **Latin script, but Italian orthography-phonology.** No new alphabet, yet
   real decoding load: `c/g` soft vs hard (*cena* vs *casa*, *gelato* vs *gatto*),
   digraphs/trigraphs `ch gh gli gn sc(i)`, and — the hard one for German L1 —
   **gemination** (double consonants: *nonno* ≠ *nono*, *pena* ≠ *penna*), which
   German does not phonemically contrast.
2. **Written accents and stress.** Learners must handle `à è é ì ò ó ù`, the
   open/closed `e`/`o` distinction, elision/apostrophe (*l'amico*, *un'amica*),
   and unpredictable stress (*àncora* vs *ancòra*).
3. **Native audio quality gate.** A good Piper `it_IT` voice exists, but
   gemination and lexical stress must be spot-checked — a TTS that flattens
   *nonno*→*nono* would teach the wrong contrast.
4. **Register and variant.** `tu`/`Lei`, and the productive-vs-receptive split of
   *passato prossimo* (produce) vs *passato remoto* (recognise, for literature).
5. **Full-scope ambition.** A1–C1 across all in-scope CEFR scales is a large,
   multi-phase authoring effort; sequencing must ship value early.

---

## 2. Language & localisation decisions

- **Script / writing system — Latin, no transliteration.** UTF-8 throughout;
  precomposed accented vowels (`à è é ì ò ó ù`) are all Latin-1 and render in the
  hugo-coder default stack. **No RTL, no complex-shaping, no custom web font**
  needed — keep the template fonts unchanged. *Recommendation: change nothing in
  the type stack; only guarantee `UTF-8` sources and lint for correct accents
  (no bare-apostrophe `e'` for `è`).*

- **Level 0 onboarding — yes, a short pre-A1 "Suoni e ortografia".** Not a full
  CEFR level (many scales have no pre-A1 descriptor), but a **3–4 page
  pedagogical onboarding**: the 7-vowel system, soft/hard `c`/`g`, digraphs
  `gli gn sc ch gh`, **gemination**, written accents, elision. *Recommendation:
  ship it as `content/level-pre-a1/` with `page_type: unit`; mint pre-A1
  descriptor IDs only where the CV actually has one (e.g. `preA1.LING.*`),
  otherwise record `no-official-descriptor`.*

- **Variant / register — Standard Italian (neo-standard).** Tuscan-based national
  norm; regional dialects out of scope. Teach `tu`/`Lei` from A1; produce
  *passato prossimo*, treat *passato remoto* as B1+ **recognition** for narrative
  literature. *Recommendation: one norm, standard, explicitly stated in the About
  page.*

- **Native voice / Piper TTS — available, add the voices.** `rhasspy/piper-voices`
  ships `it_IT` voices (e.g. `it_IT-paola-medium`, `it_IT-riccardo-x_low`); they
  are **not yet** in `audiogen/get_voices.sh` (currently fr/de/en only).
  *Recommendation: add `it_IT-paola-medium` (primary) and `it_IT-riccardo-x_low`
  (secondary, for alternating dialogue speakers); **gate on a gemination/stress
  QA pass** (minimal-pair listen-test: *nonno/nono*, *pala/palla*, *ancora*
  stress) before committing any audio; fall back to per-segment respelling
  overrides where the voice mis-renders.*

---

## 3. Instantiation from pagegen

Stand up the site by copying the template into this repo and editing only the
marked values.

1. **Copy the template.** Copy `pagegen`'s tracked tree (everything except
   `public/`, `resources/`, `.hugo_build.lock`) into `ils/`, preserving
   `layouts/`, `scripts/`, `data/`, `archetypes/`, `.github/workflows/`,
   `i18n/`, `assets/`, `static/`, the three legal pages and `go.mod`.

2. **Edit `hugo.toml`** (only the CHANGE-marked keys):
   - `baseURL = "https://boulingua.github.io/ils/"`
   - `title = "Italiano come Lingua Straniera — S. Le Boulanger"`
   - `languageCode = "it"`, `defaultContentLanguage = "it"`
   - `[params].navTitle = "Italiano"`
   - `description` / `keywords` — Italian course, CEFR A1–C1, OER
   - `[params].code = "ils"`  ← selects the accent from `data/accents.yaml`
   - `[params.plausible].domain = "boulingua.github.io/ils"`
   - `[[params.social]].url = "https://github.com/boulingua/ils"`
   - `[[menu.main]]` — rebuild sections to mirror `content/`: **Livelli**
     (pre-A1, A1, A2, B1, B2, C1) → **Materiali** (Presentazioni, Schede) →
     **Info** → **Legal** (Impressum/Datenschutz/Haftungsausschluss kept in
     German — they are statutory, not content).

3. **Regenerate the brand mark.** Run `python brand/make_icon.py` — it reads
   `code = "ils"` and paints the pentagon + favicons in `#8023C7`. Confirm
   `data/accents.yaml` already carries the `ils` row (accent `#8023C7`, hover
   `#671CA0`, dark `#B97EE7`) — it does; no edit needed.

4. **Fill legal placeholders.** Replace the `⟨…⟩` placeholders in
   `impressum.md` / `datenschutz.md` / `haftungsausschluss.md`; keep the VG Wort
   METIS disclosure paragraph in `datenschutz.md`.

5. **First green build + Pages.** `hugo --gc --minify`, run the local gate battery
   (`scripts/`), then enable the `build-deploy.yml` workflow and GitHub Pages
   (source: Actions). Definition of a green first build: site builds, pentagon is
   purple, one demo unit renders, VG Wort render-verify passes on any registered
   marks.

---

## 4. Curriculum conformance target

- **Committed target: `full` (A1–C1).** Implements every in-scope CEFR CV scale
  that has an official descriptor at A1/A2/B1 (`core`) **plus** B2 and C1. Pre-A1
  is authored pedagogically; C2 is **explicitly declared unmet** (not hidden).

- **Framework choice: CEFR** (like `daf`), not Bildungsplan-BW — Italian is not a
  BW school subject with an official plan, and the audience is mixed
  school/independent. Front matter uses `curriculum.framework: cefr` with
  `cefr_level` + `cefr_can_do`.

- **Descriptor mapping.** Every unit maps its learning outcomes to descriptor IDs
  `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` per `curriculum/docs/id-scheme.md`. Cover the
  activity domains `REC PROD INT MED` plus `LING SOC PRAG PLUR`. Where the CV has
  no descriptor for a (scale, level) cell, record **`no-official-descriptor`** —
  a silently missing scale is a conformance failure; a declared gap is not.

- **Machine-readable scope/coverage manifest.** Publish `conformance.yml` at repo
  root (shape per `curriculum/examples/de-a1/conformance.yml`): `framework`,
  `framework_version`, `language: it`, `declared_conformance: full`, and per-unit
  `realizations` with `implements_id` + the Italian can-do wording. Every
  `implements_id` **must resolve** in `curriculum/levels/*.md`; wire
  `curriculum/scripts/id-audit.sh` into CI as a blocking gate. Add a per-scale
  coverage table (levels covered vs `no-official-descriptor`) so coverage is
  auditable, not asserted.

---

## 5. Content creation plan

Recurring cast and themes give continuity: a small ensemble (e.g. **Giulia**,
**Marco**, **la famiglia Rossi**, an exchange student **Lena**) recurring across
units, city settings (Roma, Bologna, Napoli) rotating by level. Exams are
**first-class sibling bundles** (`…-exam/`) sharing a `unit_nr` with their unit.
Effort tags: **S** ≤2 days · **M** ~1 week · **L** ~2–3 weeks.

| Phase | Level | Units | Exams | Focus | Effort |
|---|---|---|---|---|---|
| 0 | pre-A1 | 3–4 | 0 | Suoni e ortografia, gemination, accents, alphabet | **M** |
| 1 | A1 | 12 | 2 module | Greetings, family, food, present tense, articles/gender | **L** |
| 2 | A2 | 12 | 2 module | Past (*passato prossimo*), travel, daily life, prepositions | **L** |
| 3 | B1 | 12 | 2 module | Opinions, work, imperfetto/futuro, subordination | **L** |
| 4 | B2 | 12 | 2 module | Argument, media, congiuntivo, register | **L** |
| 5 | C1 | 10–12 | 2 module | Nuance, literature (*passato remoto* recognition), idiom | **L** |

**Appendices** (each ≥1800 chars → each a VG Wort work): pronunciation & spelling
guide, verb-conjugation tables, grammar reference, thematic glossary (it↔de),
CEFR self-assessment grid. **Editorial pages:** About / course overview.

**Acceptance criteria per phase:** every unit has (a) all five steps, (b)
differentiated exercises **with answer keys** + teacher notes, (c) resolved
`implements_id`s that pass `id-audit.sh`, (d) committed slide deck + worksheet
(`materials_status: ready`), (e) native-voice audio for vocab/dialogue/text, (f)
a registered VG Wort mark once the page clears 1800 chars, (g) green gate battery.
A phase is "done" when its level `_index.md` landing lists every unit + exam and
the level's coverage row is complete in `conformance.yml`.

---

## 6. Website & materials

- **Section landings via shortcodes.** Each `content/level-*/‌_index.md` is
  `page_type: section` and uses the template's shortcodes (never raw HTML) to
  list its unit/exam bundles. Materials hub (`/materiali/`) is navigation — **no
  VG Wort mark**.

- **Materials pipeline (committed).** Generate locally from the branded LaTeX
  templates: **slidegen** → `.odp`/PDF decks, **sheetgen** → PDF worksheets
  (+ answer keys). Commit under `static/materials/` and `static/downloads/<level>/`;
  CI only *verifies* presence + attribution (no TeX Live in the deploy path).
  Front matter carries `presentation`/`worksheet` `{ file, thumbnail }` and, for
  exams, `exam { file }`.

- **Native-voice audio.** After adding the `it_IT` voices to
  `audiogen/get_voices.sh`, run
  `python build_audio.py /path/to/ils voices/it_IT-paola-medium.onnx it 'content/level-*/units/*/index.md'`.
  Output OGG/Opus, committed; idempotent (re-synth only on text/voice change).
  Use `it_IT-riccardo-x_low` as the second speaker in dialogues. **Decision:
  Italian audio ships, gated on the gemination/stress QA listen-test** (§2).

- **Thumbnails & downloads.** Generate deck/worksheet thumbnails alongside
  materials; the downloads gate (`verify_downloads.py`) checks each linked file
  exists and is attributed.

---

## 7. VG Wort — pixel assignment for ALL content pages

**Non-skippable.** Per `pagegen/docs/vgwort-standard.md`, **every** content page
with ≥1800 chars of original prose — every unit, every exam, every appendix,
every editorial page (About/overview) — carries **exactly one** VG Wort
Zählmarke on exactly one URL. Navigation surfaces (home, `/materiali/`, tag
indexes, `/page/2/`) and the **templated legal pages** never carry a mark.

Procedure per work:
1. Draw a **fresh public code** (32-hex, from the author's T.O.M. account) —
   never invent, never reuse a code assigned elsewhere, never expose the private
   code.
2. Register it in `data/vgwort.yaml`, matched by `url:` or `path:`, with
   `public_id` / `pixel_url` (`https://vg09.met.vgwort.de/na/<code>`),
   `min_chars: 1800`, `author`, `registered_at`.
3. It renders through the **single resolver** `layouts/_partials/vgwort/url.html`
   → `<head>` preload + eager `<body>` `<img>` (no JS, no consent gate,
   `loading="eager"`, hidden via off-screen/`visibility:hidden`).
4. Record the mark in the **private usage registry** (kept outside this repo).
5. CI gates: coverage audit (warns on any ≥1800-char page without a mark),
   render-verify (blocking — each registered pixel appears on exactly one URL),
   hub guard (blocking — `met.vgwort.de` absent from `/materiali/`).
   `vgwort.de` is excluded from the link-checker.

**Estimated total marks needed (full A1–C1 build):**

| Bucket | Count |
|---|---|
| Units (pre-A1 3 + A1–C1 5×12) | ~63 |
| Module exams (2 per A1–C1 level) | ~10 |
| Appendices | ~5 |
| Editorial (About, overview) | ~2 |
| **Total** | **~80 marks** (draw in batches per phase, ~15 per level) |

---

## 8. Milestones & sequencing

- **M0 — Scaffold live (S).** §3 instantiation: template copied, `hugo.toml`
  set, pentagon regenerated, legal placeholders filled, first green build,
  Pages deploying. `conformance.yml` stub with `declared_conformance: full`.
- **M1 — MVP: pre-A1 + A1 live (L).** Onboarding + 12 A1 units + 2 exams, all
  with materials, audio, resolved descriptor IDs, VG Wort marks. This is the
  first publicly useful slice.
- **M2 — A2 (L).** Depends on M1 cast/theme conventions and the audio QA sign-off.
- **M3 — B1 → `core` conformance complete (L).** A1–B1 in-scope scales all
  covered or `no-official-descriptor`; `core` declarable.
- **M4 — B2 (L).**
- **M5 — C1 → `full` conformance complete (L).** All A1–C1 in-scope scales
  covered; appendices finished; coverage tables complete.

**Dependencies:** audio pipeline (add `it_IT` voices + QA) blocks M1; `id-audit.sh`
CI gate blocks every phase sign-off; materials generation blocks each unit's
`ready` status.

**Definition of done — flip from "coming soon" to active on the world map:**
site builds green on every gate (VG Wort coverage/render, downloads, legal,
attribution, `id-audit.sh`); pre-A1 + **A1 fully live** with materials, audio and
registered VG Wort marks; `conformance.yml` publishes a truthful scope with
`declared_conformance` at least `core`; About page and legal pages complete. (Full
`full`-level conformance is the M5 target, not the flip gate.)

---

## 9. Open decisions & risks

- **Gemination in TTS (risk).** If `it_IT-paola` flattens double consonants or
  mis-stresses, the audio would teach a wrong contrast. *Mitigation:* minimal-pair
  QA listen-test before any commit; per-segment respelling overrides; consider a
  higher-quality voice if one lands in `piper-voices`.
- **Open/closed `e`/`o` (decision).** Whether to mark è/é and ò/ó distinctions
  explicitly in A1 or defer. *Lean:* introduce written accents in pre-A1, treat
  the open/closed *sound* distinction as awareness-only until B1.
- **`passato remoto` scope (decision).** Recognition-only, tied to literature at
  B1+; do not require production. Confirm in the B1/B2 skills mapping.
- **Section/menu labels language (decision).** Italian UI labels (Livelli,
  Materiali, Info) with German statutory legal pages — consistent with sister
  sites; confirm i18n strings.
- **pre-A1 descriptor coverage (risk).** Many CV scales lack pre-A1 descriptors;
  the onboarding stage must record `no-official-descriptor` rather than mint
  invented IDs, or `id-audit.sh` fails.
- **C2 gap (accepted).** Declared out of committed scope; revisit only if
  openly-licensed C2 source material becomes available.
- **Content licence (open).** README notes CC BY 4.0 "to be confirmed"; the
  pagegen standard is **CC BY-SA 4.0** — align before first release.
