> **Implementation update — 30 October 2026.** A runnable development site and
> `curriculum.yml` now exist. The current framework version is 1.2.0. Use the
> shared `kit` commands and `README.md` for setup; `pagegen`, `slidegen`,
> `sheetgen` and `audiogen` are archived predecessors. Older status counts,
> installation commands and `conformance.yml` names below are historical.
> The language-specific teaching plan remains pending; a building shell does
> not satisfy its learner-release milestones. See `REFINEMENT.md`.

<!-- status-audit-2026-09-09 -->
> ## Status audit — 2026-09-09
>
> **This repository is still a scaffold:** 5 tracked files, no Hugo shell yet, no `content/` pages, no build output committed.
> The build plan below stands and is not superseded. This box records what the platform
> now requires that the plan predates, and what the first moves are.
>
> **Declared scope (from this plan):** CEFR A1–C1 (full). Pre-A1 onboarding included. C2 explicitly out of scope.
>
> ### Platform requirements added since this plan was written
>
> - **`conformance.yml` at the repo root is mandatory** to declare any conformance level.
>   The framework (`curriculum` v1.1.0) defines `core` (A1–B1), `full` (A1–C1) and
>   `complete` (Pre-A1–C2). **No course in the organisation has this file yet** — not even
>   `efl`, which has the descriptor IDs in place. Absence of the manifest, not absence of
>   content, is what currently blocks every conformance claim in the suite.
> - **Descriptor IDs take the form `A1.REC.overall-oral-comprehension.01`.** Every
>   `implements_id` must resolve against `curriculum/levels/`; `conformance_audit.py
>   resolve` is the gate.
> - **Shortcodes, CSS tokens and layout chrome come from the shared `kit` Hugo module.**
>   A course repo holding its own copies is drift.
> - **CI is shared** and lives in `boulingua/.github`: `course-build.yml`,
>   `kit-drift.yml`, `link-check.yml`, `materials-latex.yml`, `org-audit.yml`.
> - **`[taxonomies]` is declared in no course `hugo.toml` today** — including the three
>   mature ones. Declare it here from the start rather than inheriting the omission.
>
> ### First moves, in this order
>
> 1. **Instantiate from `pagegen`.** Copy the template's `hugo.toml`, `go.mod` and CI entry point; do not copy layouts, shortcodes or CSS — those arrive from the shared `kit` module, and a local copy is drift that `kit-drift.yml` will flag.
> 2. **Write `conformance.yml` before the first unit.** Declare `framework: boulingua-curriculum`, `framework_version: 1.1.0`, the target from the scope above, and `conformance_status: in-progress`. Authoring units before the manifest exists is how `daf`, `efl` and `fle` each ended up with descriptor IDs in front matter that no machine can audit.
> 3. **Author unit 01 as a complete vertical slice** — page bundle, exam sibling bundle, slide deck, worksheet, audio and descriptor IDs — and only then scale out. One finished unit settles every convention question the remaining units would otherwise re-open.
>
> ### Where this sits in the suite
>
> Sequencing across all 18 courses, and the reasoning behind the order, live in
> [`boulingua/.github/ROADMAP.md`](https://github.com/boulingua/.github/blob/main/ROADMAP.md).
> The reference implementation to copy from is **`efl`** (180 unit bundles + 180 exam
> bundles, `unitNN-slug` naming, `page_type` and a `curriculum:` block on every unit).

---
# ILS — Italian course roadmap

Build plan for **Italiano come Lingua Straniera** (`ils`), the boulingua Italian
course, from the current empty scaffold (LICENSE + README + brand icons) to a
live, standards-conforming Hugo site with content, materials, native-voice audio
and VG Wort counting marks on every editorial page. Signature accent `#8023C7`
(light) / `#B97EE7` (dark), pentagon mark, already registered in the kit's
`data/accents.yaml`.

---

## 1. Overview

**What it is.** A free, openly-licensed Italian course for the German
*Gesamtschule* system and independent learners, following the boulingua
five-step unit model (Activate → Input → Practise → Apply → Reflect), built on
the `kit` platform and conforming to the `curriculum` descriptor framework.
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
literature, Wikimedia audio) and one licence-clear Piper TTS voice.

**Defining challenges (from the language context):**

1. **Latin script, but Italian orthography-phonology.** No new alphabet, yet
   real decoding load: `c/g` soft vs hard (*cena* vs *casa*, *gelato* vs *gatto*),
   digraphs/trigraphs `ch gh gli gn sc(i)`, and — the hard one for German L1 —
   **gemination** (double consonants: *nonno* ≠ *nono*, *pena* ≠ *penna*), which
   German does not phonemically contrast.
2. **Written accents and stress.** Learners must handle `à è é ì ò ó ù`, the
   open/closed `e`/`o` distinction, elision/apostrophe (*l'amico*, *un'amica*),
   and unpredictable stress (*àncora* vs *ancòra*).
3. **Native audio: one usable voice, and a quality gate on it.** Exactly one
   `it_IT` voice in the catalogue carries a licence that can ship (§2), and it is
   single-speaker; gemination and lexical stress must still be spot-checked, because
   a TTS that flattens *nonno*→*nono* would teach the wrong contrast — and there is
   no second Italian voice to fall back to if it does.
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

- **Native voice / Piper TTS — `it_IT-serena-medium` (CC BY 4.0), single-voice.**
  The two voices an earlier draft of this document named, `it_IT-paola-medium`
  (primary) and `it_IT-riccardo-x_low` (second speaker), both state
  `License: See URL` on their model cards with no resolvable statement behind the
  URL. "No statement" is not permission, and audio synthesised from a model
  inherits its training data's terms, so neither can go inside a CC BY-SA 4.0
  course. `it_IT-serena-medium` is the **only** licence-clear Italian voice;
  `it_IT-serena-high` is the same speaker at a higher tier, not a second speaker.
  *Consequences, both binding: (a) the two-speaker dialogue plan is not
  achievable — `ils` ships **single-voice dialogues with typographic speaker
  labels**, the same fallback `ele/ROADMAP.md` specifies for this case, and never
  a Spanish or French voice standing in for the second Italian one; (b) CC BY 4.0
  is an attribution licence, so `serena` needs a row in `NOTICE.md` before its
  audio ships. Still **gate on a gemination/stress QA pass** (minimal-pair
  listen-test: `nonno/nono`, `pala/palla`, `ancora` stress) before committing any
  audio, and fall back to per-segment respelling overrides where the voice
  mis-renders — with no second Italian speaker, respelling is the only remedy
  short of transcript-only.*
- **Voice IDs are never hand-typed.** The ID, tier and licence above are read from
  `kit/audio/voices.yml`, which is the single source of truth, must never be
  hand-copied into prose, and keeps the rejected `paola`/`riccardo` rows with
  their licences so the decision stays auditable. A voice ID retyped from memory is how
  a 404 gets written into a download script.

---

## 3. Instantiation from the kit

Stand up the site by building the repo around the kit and editing only the
marked values. The shared surface is imported, never copied.

1. **Create the repo.** `ils/` holds a short `hugo.toml` — baseURL, title,
   languageCode, `[params]`, menus, and the module import

   ```toml
   [module]
     [[module.imports]]
       path = "github.com/boulingua/kit"
   ```

   — a `go.mod`/`go.sum` requiring `github.com/boulingua/kit` at the pinned tag
   (`v1.0.0`), `boulingua.yml` (the per-course config the gate battery reads),
   the twelve-line `.github/workflows/deploy.yml` that calls the org's reusable
   workflow, an empty `content/` skeleton and the three legal pages. `layouts/`,
   `assets/`, `i18n/`, `archetypes/` and `scripts/` are **not** copied and must
   never appear in this repo: they are the drift surface, and a file that is not
   here cannot fork. Hugo resolves the first four from the module; CI checks the
   kit out at the pinned tag so `scripts/` and the gate battery are on disk
   before Hugo runs. `public/`, `resources/` and `.hugo_build.lock` stay
   untracked.

2. **Vendor `_materials/`.** Run `kit materials sync` — it assembles the kit's
   `latex/`, `fonts/` and `brand/icons/` into the flat `_materials/` tree the
   LaTeX build expects — and commit it. This is the only vendored surface,
   because XeLaTeX cannot read a Hugo module, and it is verified against the
   digests in `kit.lock` on every build, so a hand-edited style file fails
   loudly instead of forking. (Earlier drafts told you to copy `_materials/`
   from the template; `pagegen` never had one.)

3. **Edit `hugo.toml`** (only the CHANGE-marked keys):
   - `baseURL = "https://boulingua.github.io/ils/"`
   - `title = "Italiano come Lingua Straniera — S. Le Boulanger"`
   - `languageCode = "it"`, `defaultContentLanguage = "it"`
   - `[params].navTitle = "Italiano"`
   - `description` / `keywords` — Italian course, CEFR A1–C1, OER
   - `[params].code = "ils"`  ← selects the accent from the kit's
     `data/accents.yaml`. Setting it matters: the kit ships the neutral graphite
     `template` accent by default, so a course that forgets looks obviously
     unconfigured rather than looking like DaF.
   - `[params.plausible].domain = "boulingua.github.io/ils"` — keep this
     sub-table **last** under `[params]`; TOML scopes everything after a header
     to that header, so opening it early would silently move the bare `[params]`
     keys under it and leave the site unstyled.
   - `[[params.social]].url = "https://github.com/boulingua/ils"`
   - `[[menu.main]]` — rebuild sections to mirror `content/`: **Livelli**
     (pre-A1, A1, A2, B1, B2, C1) → **Materiali** (Presentazioni, Schede) →
     **Info** → **Legal** (Impressum/Datenschutz/Haftungsausschluss kept in
     German — they are statutory, not content).

4. **Regenerate the brand mark.** Run the kit's `brand/make_icon.py` — it reads
   `code = "ils"` and paints this repo's pentagon + favicons in `#8023C7`.
   Confirm the kit's `data/accents.yaml` already carries the `ils` row (accent
   `#8023C7`, hover `#671CA0`, dark `#B97EE7`) — it does; it arrives with the
   module, and no copy of it belongs here.

5. **Fill legal placeholders.** Replace the `⟨…⟩` placeholders in
   `impressum.md` / `datenschutz.md` / `haftungsausschluss.md`; keep the VG Wort
   METIS disclosure paragraph in `datenschutz.md`.

6. **First green build + Pages.** `hugo --gc --minify --panicOnWarning`, run the
   gate battery locally through `bin/kit`, then enable the `deploy.yml` workflow
   and GitHub Pages (source: Actions). Definition of a green first build: site
   builds with no warnings, pentagon is purple, one demo unit renders, VG Wort
   render-verify passes on any registered marks.

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
  `implements_id` **must resolve**. Add a per-scale coverage table (levels covered
  vs `no-official-descriptor`) so coverage is auditable, not asserted.

- **Gate (blocking).** The reusable workflow
  `boulingua/.github/.github/workflows/course-build.yml@v1` runs
  `python .curriculum/scripts/conformance_audit.py resolve --manifest
  conformance.yml --content content`, which resolves this repo's manifest against
  the framework's levels and scale registry. `id-audit.sh` audits the framework's
  *own* level files and **cannot** validate this repo. Do not wire it here.

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
`implements_id`s that pass the §4 gate, (d) committed slide deck + worksheet
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
  templates in `_materials/` (from `kit/latex/`): `.odp`/PDF decks, PDF worksheets
  (+ answer keys). Commit under `static/materials/` and `static/downloads/<level>/`;
  CI only *verifies* presence + attribution (no TeX Live in the deploy path).
  Front matter carries `presentation`/`worksheet` `{ file, thumbnail }` and, for
  exams, `exam { file }`.

- **Native-voice audio.** After adding the `ils` row to `kit/audio/voices.yml`,
  run
  `python build_audio.py /path/to/ils <serena .onnx> it 'content/level-*/units/*/index.md'`
  — the `.onnx` path comes from that row and is never typed in by hand. Output
  OGG/Opus, committed; idempotent (re-synth only on text/voice change). There is
  **no second speaker**: `it_IT-serena-medium` is the only licence-clear Italian
  voice (§2), so dialogues are single-voice with typographic speaker labels.
  **Decision: Italian audio ships, gated on the gemination/stress QA listen-test**
  (§2) and on `serena`'s CC BY 4.0 attribution row landing in `NOTICE.md`.

- **Thumbnails & downloads.** Generate deck/worksheet thumbnails alongside
  materials; the downloads gate (`verify_downloads.py`) checks each linked file
  exists and is attributed.

---

## 7. VG Wort — pixel assignment for ALL content pages

**Non-skippable.** Per `kit/docs/vgwort-standard.md`, **every** content page
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

- **M0 — Scaffold live (S).** §3 complete: repo standing on the kit module,
  `_materials/` vendored, `hugo.toml` set, pentagon regenerated, legal placeholders filled, first green build,
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

**Dependencies:** audio pipeline (the `ils` row in `kit/audio/voices.yml`, the
`NOTICE.md` attribution row, and the gemination QA pass) blocks M1; the §4
conformance gate blocks every phase sign-off; materials generation blocks each
unit's `ready` status.

**Definition of done — flip from "coming soon" to active on the world map:**
site builds green on every gate (VG Wort coverage/render, downloads, legal,
attribution, the §4 conformance gate); pre-A1 + **A1 fully live** with materials, audio and
registered VG Wort marks; `conformance.yml` publishes a truthful scope with
`declared_conformance` at least `core`; About page and legal pages complete. (Full
`full`-level conformance is the M5 target, not the flip gate.)

---

## 9. Open decisions & risks

- **Gemination in TTS (risk, now unhedged).** If `it_IT-serena-medium` flattens
  double consonants or mis-stresses, the audio would teach a wrong contrast — and
  because it is the only licence-clear Italian voice (§2), there is no second
  speaker to switch to. *Mitigation:* minimal-pair QA listen-test before any
  commit; per-segment respelling overrides; `it_IT-serena-high` (same speaker,
  same CC BY 4.0) if the tier helps; transcript-only for individual items that
  cannot be fixed. Revisit if an Italian voice lands with an allow-listed licence.
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
  invented IDs, or the §4 gate fails.
- **C2 gap (accepted).** Declared out of committed scope; revisit only if
  openly-licensed C2 source material becomes available.
- **Content licence (open).** README notes CC BY 4.0 "to be confirmed"; the
  platform standard is **CC BY-SA 4.0** — align before first release.
