# distil-research

> A Claude Code skill that turns messy AI research folders into a clean, compounding knowledge base.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Built for: Claude Code](https://img.shields.io/badge/Built%20for-Claude%20Code-blue.svg)](https://claude.ai/code)
[![Status: Active](https://img.shields.io/badge/Status-Active-green.svg)]()

You run deep research on Perplexity, ChatGPT, or xAI. The files pile up in `~/Research/<topic>/`. After three months, the folder is a graveyard you never reopen.

**`distil-research` turns the graveyard into a re-readable asset that compounds with every new file you add.**

---

## What it does, visually

```
   BEFORE                                AFTER
   ────────                              ────────

   ~/Research/<topic>/                   ~/Research/<topic>/
   ├─ I came accross this post...md      ├─ _sources/      ← canonical kept files
   ├─ How to do X for Y...md             ├─ _distilled/
   ├─ Common errors when doing...md      │  ├─ playbook.md      ← operator reference
   ├─ Examples of the best Z...md        │  └─ manifest.json    ← audit trail
   ├─ Patterns for building W...md       ├─ _archive/      ← low-signal / superseded
   ├─ ... 34 more files                  └─ _quarantine/   ← ambiguous (needs review)

   (you never re-open this)              (you re-open the playbook weekly)
```

The top level after a run holds **only the four `_` folders**. New research drops into the top level and gets routed on the next run.

---

## Who this is for

Anyone whose AI-research output ends up as a folder of files they never re-open.

You run deep research on Perplexity, ChatGPT, xAI, or any other deep-research tool. The exports pile up. Naming is whatever prompt you typed. Information overlaps across files. You can't tell what you've already covered, so you re-research things you already researched.

`distil-research` is for turning that into a **lean playbook that stays lean** — every new file you add updates the playbook with whatever new signal it carries. The playbook can grow as you accumulate genuinely new tactics, but it never bloats: every line still earns its place.

---

## What it's useful for

Six concrete situations. None depend on what you're researching — only on the shape of the problem.

### 1. Daily research → daily clean

You ran a research session this morning. Five new exports landed in `~/Research/<topic>/`. By tonight they'll feel like noise.

Run `/distil research` after the session. The signal goes into the playbook, the chaff goes to `_archive/`. Tomorrow's research drops into a folder that's already organised.

### 2. Stale-folder rescue

That folder you haven't opened in four months — 30+ files, half are duplicates, you can't remember what's in any of them.

One distil pass turns it from "graveyard" to "operator's reference". The playbook is the new entry point. The archive preserves everything in case you need to dig.

### 3. Pre-action distillation

You're about to act on a body of research — write content, build a product, make a decision, draft a brief. You need the *useful* parts, not the full dump.

Distil produces a playbook structured around action: **Rule → Execution → Example → Edge cases**. Open it, scan it in 60 seconds, act. The manifest lets you audit the decision later.

### 4. Compounding knowledge base

You research the same theme over weeks or months. New files keep dropping in. Without intervention, the folder grows linearly while useful signal stays roughly constant — each new file makes the folder *less* navigable, not more.

```
   WITHOUT distil                        WITH distil
   ─────────────                         ─────────────

   files ──▶ files ──▶ files             files ──▶ playbook ──▶ playbook
                                                  (lean)        (still lean,
                                                                 updated with
                                                                 new signal)

   accumulates → unusable                playbook updates; the rest goes to
                                          _archive/ — folder stays navigable
```

Each `/distil research` run is incremental. New files get classified; the playbook gets updated where there's genuinely new signal, and untouched where there isn't. Duplicates get archived. The playbook grows when there's new content to add and stays the same when there isn't — never just because more files showed up.

### 5. Cross-topic hygiene

`~/Research/` has accumulated dozens of topic folders. You don't know which are still relevant, which overlap, which need rework.

Run housekeeping mode (`/distil research` on the parent folder). You get:

- A grade per topic with a confidence label
- Cross-topic overlap detection (real content overlap vs lookalike filenames)
- Filename normalisation suggestions
- A prioritised list of next actions

No files moved without your explicit confirmation. The default is *propose, don't action*.

### 6. Hallucination filtering for AI research

Any research where claimed technical specifics matter — API names, internal mechanisms, repo URLs, version numbers, named algorithms — is at risk of containing AI-fabricated detail that *looks* real.

Distil flags uncross-verifiable technical claims, keeps only the operator-tactical content (signal weights, procedures, formulas) in the playbook, and records the filtering decision in the manifest's `hallucination_warnings`. You get a clear audit trail of what was filtered and why.

---

## The compounding part, made explicit

This is the value most people miss until they've used the skill for a month. The playbook updates as new research lands. It grows when there's genuinely new signal. It stays put when new files are mostly restatements of what you already have.

```
   week 1                week 2                week 3                week 4
   ─────                 ─────                 ─────                 ─────

   5 new files     →     +4 new files    →     +6 new files    →     +3 new files
   playbook 7 KB         playbook 9 KB         playbook 10 KB        playbook 10 KB
   _sources/  4          _sources/  6          _sources/  9          _sources/ 10
   _archive/  1          _archive/  3          _archive/  6          _archive/  7

                         (2 new tactics         (1 new tactic         (this week's
                          merged in)             merged in; the         research
                                                  rest duplicated)       duplicated
                                                                          existing)
```

The playbook only grows when new files contribute new signal. Duplicates and restatements go to `_archive/` — they're preserved, just not in the way of the playbook. Every line in the playbook still earns its place after every run.

After two months you have a tight reference you re-read weekly, plus a fat archive you rarely touch — instead of a fat folder you ignore.

---

## How it works

```
                    ┌──────────────────────┐
   user drops new   │   ~/Research/        │   ← /distil research
   research files   │   <topic>/           │
                    └──────────┬───────────┘
                               │
                               ▼
         ┌─────────────────────────────────────────────┐
         │ 1. Tier filter — section by section, not    │
         │    file by file                              │
         │      T1  specific tactics with numbers       │
         │      T2  non-obvious frameworks              │
         │      T3  counterintuitive findings           │
         │      T4  tools / resources (low priority)    │
         └─────────────────────┬───────────────────────┘
                               │
                               ▼
         ┌─────────────────────────────────────────────┐
         │ 2. Strict keep gate                          │
         │      Unique tactic?            ─┐            │
         │      Non-overlapping framework? ├─→ keep     │
         │      Materially better version? ─┘            │
         │      (else → archive)                         │
         └─────────────────────┬───────────────────────┘
                               │
                               ▼
         ┌─────────────────────────────────────────────┐
         │ 3. In-playbook dedup                         │
         │      Repeated ideas → merged                  │
         │      One canonical version per concept        │
         └─────────────────────┬───────────────────────┘
                               │
                               ▼
         ┌─────────────────────────────────────────────┐
         │ 4. Two-job test                              │
         │      Does the playbook cover >1 distinct      │
         │      operational job?                         │
         │         No  → write single playbook           │
         │         Yes → propose split, ask user         │
         └─────────────────────┬───────────────────────┘
                               │
                               ▼
         ┌─────────────────────────────────────────────┐
         │ 5. Write the playbook                        │
         │      Operator-first: Rule / Execution /       │
         │      Example / Edge cases                     │
         │      ASCII diagrams where they help           │
         │      No markdown tables                       │
         └─────────────────────┬───────────────────────┘
                               │
                               ▼
         ┌─────────────────────────────────────────────┐
         │ 6. Route every file to its final home        │
         │      kept       → _sources/                   │
         │      partial    → _sources/ or _archive/      │
         │      archived   → _archive/                   │
         │      quarantined→ _quarantine/                │
         └─────────────────────────────────────────────┘
```

---

## Two modes

```
   TOPIC MODE (frequent)              HOUSEKEEPING MODE (occasional)
   ─────────────────                  ─────────────────────────

   Target: one topic folder           Target: ~/Research/ (parent)
   ~/Research/<topic>/                ~/Research/
                                      ├── <topic-a>/
   Produces:                          ├── <topic-b>/
   • playbook.md                      ├── <topic-c>/
   • manifest.json                    └── ... (N topics)
   • _sources/ _archive/
     _quarantine/                     Produces:
                                      • _overlap-report.md
                                      • Topic grades + confidence labels
   Run on every new batch             • Filename normalisation suggestions
   of research                        • Cross-topic overlap detection
                                      • Recommendations (no auto-moves)

                                      Run monthly-ish for hygiene
```

---

## Install

This is a Claude Code skill. Clone into your skills folder:

```bash
cd ~/.claude/skills
git clone https://github.com/noodlebindev/distil-research.git
```

Then in any Claude Code session, say:

```
distil my research on ~/Research/<topic>
```

…or any of:

- `/distil research`
- `clean up my research folder`
- `organise my research`
- `build a playbook from my research`

---

## What the playbook looks like

Every section follows a scannable shape. A reader should know what to do in 15 seconds:

```
## <Section name>

**Rule:** <one-line action principle — verb-led, no hedging>
**Execution:** <concrete how-to — bullets or numbered steps>
**Example:** <a real example with values, names, or numbers>
**Edge cases:** <when this breaks or doesn't apply> (optional)
```

ASCII diagrams replace markdown tables (which don't render reliably across viewers). Example showing how relative impact is communicated in a playbook section:

```
Signal A (high-leverage interaction)    ████████████████████████  ~24× baseline
Signal B                                ██████████████████        ~15×
Signal C (gatekeeper)                   ███████████               high
Signal D                                ████████                  significant
Signal E                                ██                        marginal
Signal F                                █                         baseline
```

Strategic / reflective sections don't force the Rule / Execution / Example shape — they use bulleted observations to avoid feeling mechanical.

---

## Manifest schema

Every run writes `_distilled/manifest.json`. Per-file entries:

```
verdict              ─┬─ kept              (full canonical reference)
                      ├─ partial           (some sections extracted)
                      ├─ archived          (low signal, superseded)
                      └─ quarantined       (ambiguous, needs review)

contribution_type    ─┬─ canonical_source     (primary source — rare)
                      ├─ supporting_signal    (reinforces other files)
                      ├─ partial_extract      (gold sections, rest noise)
                      ├─ superseded_duplicate (covered by another file)
                      ├─ low_signal           (generic / consensus)
                      └─ ambiguous            (mixed quality)

confidence           ─ high / medium / low
reason               ─ free-text explanation
sections_kept        ─ which sections from this file survived
sections_influenced_in_playbook
                     ─ which playbook sections this file fed
```

Top-level fields include `playbook_size_bytes` (informational), `read_coverage` (when cluster sampling was used on >20-file folders), and `hallucination_warnings` (when sources contained uncross-verifiable technical claims that were filtered).

Full schema and decision rules in [SKILL.md](SKILL.md).

---

## Good at / not yet good at

**Good at:**

- Markdown exports from Perplexity, ChatGPT deep research, xAI
- Mixed-format folders (markdown + RTF + small PDFs)
- Topics with operator framing — workflows, tactics, build wedges, content strategies
- Any folder size — cluster sampling kicks in for >20-file folders

**Not yet good at:**

- Large PDF-only folders (text extraction is best-effort)
- Audio / video transcripts (treated as one section)
- Non-English content (untested)
- Pure narrative topics (history, ethnography) — operator-first format is built for tactical content

---

## Known limitations

- **Two-job test is judgement-based.** Small chance of false positives ("this should be one playbook") or false negatives ("split me!"). User confirmation required before any split.
- **Cluster sampling trades coverage for speed.** Recorded in `read_coverage`; outliers promoted to direct-read but the heuristic is imperfect.
- **Filename normalisation is propose-only.** Never auto-renames — protects against breaking links, scripts, manifests.
- **Tested primarily on solo-creator workflows** (one topic per folder, 5–50 files per topic). Team / shared folder behaviour untested.

---

## Contributing

Open an [issue](https://github.com/noodlebindev/distil-research/issues) with:

- The folder you ran the skill on (file count, formats, rough topic)
- What you expected vs what happened
- Anything from the manifest or chat summary that reveals where the skill misjudged

If you're proposing a change to the skill itself, open a PR against [SKILL.md](SKILL.md) with rationale.

---

## Cadence

The author distils their own research weekly. Expect roughly-monthly updates. Major changes get a release entry in [CHANGELOG.md](CHANGELOG.md).

---

## License

MIT — see [LICENSE](LICENSE).
