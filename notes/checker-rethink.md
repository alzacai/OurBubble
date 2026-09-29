> **Provenance note (2026-09-29).** The owner, reading PR #120 (the `beside` binding): *"I feel like
> we could reorganize this into something more modular and data vs logic. why do we even have this
> check_edition.py"* — and then: *"check_edition.py is the wrong way to solve this. it's not modular
> and logical but reactive and mixes simple loops with complex comments about edge cases. the
> content isn't wrong, but the delivery is. this entire method needs to be rethought."* This note
> is the rethink, written for the owner to judge before any code moves. Nothing here is built.

# The checker, rethought — the reader is the guard, and the code checks that the reader has read

Lane note, not chapter text. Delivery: this note; the work it proposes is a Repository-lane
tranche once the owner has ruled on §4.

FIREWALL: this note is about how a book about a toy checks itself. Nothing in it is a claim about
nature, and the one guarantee it must not weaken is the firewall's own: *the book faithfully
reports what the experiment recorded.*

---

## 1 · What the checker is, read whole

`check_edition.py` is 2,431 lines, 53 functions, 22 compiled regexes, and 35 mentions of a
self-test. Ten of its comments cite a proof-read round by number. It runs as step 2 of tier 0
(`tools/check.sh`), beside `tools/beat_coverage.py` (578 lines), `tools/attacks_beats.py` (404),
`tools/figures.mjs` (519) and the napkin's own refusals in `tools/napkin.py` (1,254). Read as a
whole it is four different things in one file:

| # | kind | what it holds | nature |
|---|---|---|---|
| A | **evidence integrity** | the `record/` snapshot equals the pinned commit byte for byte; the vendored engine hashes match `engine.lock`; every declared quotation is verbatim in its source; every cited entry, gate and figure path exists; `record.lock`'s path list equals what `edition.json` depends on; the appendix is in step; no built page loads anything from another host; every link resolves; every reader-note link parses back to its heading | deterministic, over data |
| B | **structural contract** | a Scope block that says *toy*; a `Next` on every chapter but the last; the closing pointer by slug; beats `slug.n` ascending, at most twelve, matching `OUTLINE.md`; 350–1800 words a chapter, 150 a paragraph, 24 a sentence on average, 100–220 a section; captions under their ceilings; every napkin token resolved | deterministic; a schema |
| C | **prose guards** | the nature-claims denylist — 9 phrases, 25 patterns, a toy exemption with a 40-character reach, a nature-subject veto, an abbreviation list for the segmenter; 16 retired phrasings; broken joins; emphasised numbers anchored and, since #120, bound `beside` their noun | heuristic regexes over natural language |
| D | **self-proof** | 49 probes the denylist must refuse; the exemption's triples; the retired phrasings' own probes; the `beside` triples; the beat and demo mutation suites; the status-discipline rule — a check cannot print its own pass line | tests of the tests |

A, B and D are sound, and only badly packaged. C is the method problem.

## 2 · The diagnosis — reactive delivery

**The treadmill.** Every proof-read round that found a nature claim in new words ended the same
way: a regex grew a clause, a veto or an exemption was added beside it, and a paragraph was
written into the code explaining what went wrong in round *N*. Tranche F did this eight times on
one chapter — sentences instead of full stops (round 4), exclusivity without a modal (5), the
segmenter and the toy exemption (6), the exemption re-scoped to the claim's subject (7), the
nature-subject veto (8). Each was correct. Together they are a regex that has learned the last
eight readers and will meet a ninth.

**The admitted limit.** The file says it plainly, in `flattened()`: *"the same sentence reworded
passes, and always will: this refuses the sentences somebody wrote down, in any punctuation and
any markup, not the idea behind them."* That sentence is the whole finding. A denylist can hold
the nine literal phrases of the excluded programme; it cannot hold *the claim*. The code is
standing in for a judgment it cannot make, and the war stories in the docstrings are what that
costs.

**The mixing.** Because each guard carries its own history, a ten-line loop sits under a
sixty-line essay about edge cases, and the reader of the code cannot tell the rule from the
incident. The numbers the writing contract sets (350, 1800, 150, 24, 100–220, 12, 300) are
literals in three files, so `EDITION_STANDARD.md` and the checker can disagree without either
noticing.

**The same shape, three more times.** The fresh reader on #121 attacked the guards and found:
a removed section returns green under a borrowed beat number (a "split"); "a fiftieth of a full
turn" becomes "a fortieth" green, on the chapter and the appendix note both; the *toy* test greps
the file, so *toy* removed from the Scope block and "a toy stick" written into beat 2 builds
green. Each is a guard that checks a file for a word instead of a block for its claim. They are
not three bugs; they are the delivery.

## 3 · What must not change

Three things the rethink keeps exactly, because they are what the firewall's guarantee rests on:

1. **Evidence integrity (A) stays byte-for-byte and verbatim.** A quotation is checked against
   the pinned record, and the snapshot against the engine, on every build. This is the guarantee;
   everything else is housekeeping around it.
2. **Refusing, not checking** (`EDITION_STANDARD.md` rule 4). A guard asks whether the wrong thing
   is absent, never only whether the right thing is present, and it lands with the mutation that
   proves it bites.
3. **The check does not hold the pen on its own verdict.** `status()` and the status-discipline
   audit stay verbatim: a failing check cannot report a clean one, and a builder that quietly guts
   a guard fails its own self-test (proved on #120 by mutation D).

## 4 · The rethink

### 4a · For prose, the reader is the guard — and the code checks that the reader has read

The repository already has the right instinct everywhere but here: a claim is *registered before
the run*; a copy is *verified against the original*. Apply it to prose.

A chapter's text gets a **reading**: the proof-reader's structured verdict, made by a fresh reader
with the firewall's own table and the toy exemption in front of it in *prose*, sentence by
sentence, for the two things a regex cannot judge — a claim about nature (mode 7) and a thing
described by its negatives (#99) — and for the two things a regex could judge but nobody has
written (counts of the book's own parts, mode 13; a registered guess in the first person, #105).
The reading is committed beside the chapter with the hash of the words it read. Tier 0 then asks
one deterministic question:

> *Is every chapter's current text the text that was read and cleared?*

Any edit to the words invalidates the reading and demands a fresh one. A HOLD reading refuses the
build the way a missing quotation does. Rounds stop growing the code; they grow the ledger, and
the ledger is the audit trail the docstrings were trying to be.

What this changes in C: the denylist shrinks to the **nine literal phrases** and their probes —
the part that was never reactive, because the excluded programme's own words do not reword
themselves. The 25 patterns, the toy exemption, the nature-subject veto, the abbreviation list and
their triples go, because the thing they approximated is now done by a reader who can tell *of the
triangle* from *of the universe* without a forty-character reach. Retired phrasings stay as data:
they are literal too.

**The reading's form**, one file per chapter, `readings/<slug>.json`:

```json
{ "slug": "the-shadow",
  "words_sha256": "…",           /* sha256 of flattened(chapter): words only, so a wrap or a
                                    markup change does not stale it and a word change does */
  "rubric": "readings/RUBRIC.md@<sha>",
  "read_at": "2026-09-29", "reader": "fresh agent, model …, worktree at <commit>",
  "verdict": "CLEAR",
  "findings": [ { "beat": "the-shadow.6", "anchor": "…", "mode": 7, "severity": "bump",
                  "direction": "…" } ] }
```

`readings/RUBRIC.md` is the firewall's table, the toy-exemption rule and #99 as a reader's
checklist — the prose that the regexes were a lossy compression of. The reading is produced by the
proof-reader skill's run and committed on the PR; the owner's merge is its acceptance. A reading
is data: it can be re-read, disputed on the PR, and superseded, and none of that touches code.

### 4b · Everything else is declarations plus small pure rules

- **One manifest.** `edition.json` already holds the phrases, patterns, probes, retired phrasings
  and bindings. It gains a `contract` block — the bands, ceilings, limits and the beat cap — so the
  writing contract and the checker read one source, and `EDITION_STANDARD.md` quotes it rather
  than restating it. The manifest is **schema-validated on load**: a stale key, a quotation
  without a source, a `beside` on a quotation the chapter never emphasises, is refused before
  anything is checked.
- **One signature.** Every rule is a pure function `(text, declarations) → findings`, ten to
  thirty lines, tested by a table of cases it must pass and must refuse. No rule reads the
  repository; a thin layer reads files and hands rules their text.
- **One finding record.** `{file, location, rule, severity, what, direction}` — the same shape the
  proof-reader's table already uses, so a reading, a rule's refusal and a mutation report are one
  kind of thing and render the same way on a PR.
- **History out of the code.** A rule's docstring says what it refuses and its limit, in two
  lines. What went wrong in which round moves to `LESSONS.md`, keyed by rule id, the way the lab
  keeps its own.

### 4c · The shape

```
edition/
  manifest.py      load + validate edition.json (schema, cross-references, the contract block)
  text.py          the segmenter: sentences(), flattened(), words() — the one NL primitive
  findings.py      the finding record, and its renderers (terminal, PR table)
  rules/
    evidence.py    A — snapshot, engine, quotations verbatim, paths, lock, appendix in step
    contract.py    B — scope, next, pointer, beats, bands, tokens, captions
    readings.py    C — each chapter's words match a CLEAR reading; the nine phrases; retired phrasings
    anchoring.py   C — emphasised numbers anchored and bound beside their noun
    rendered.py    A/B on built pages — external assets, links, reader-note links
  cli.py           `python -m edition check [--rendered]`; the status discipline lives here
tests/             one table-driven file per rules module; the probes and triples become rows
readings/          RUBRIC.md and one reading per chapter
```

`check_edition.py` becomes a hundred-line runner, or disappears into `cli.py`. `make check`, CI
and `tools/check.sh` do not change their contract: five steps, the same pass lines, the same
refusals by name.

## 5 · Migration, without a day when the guarantee is off

1. **Freeze the oracle.** The current `check_edition.py` is renamed, not edited, and keeps running
   in tier 0 until step 5.
2. **Build the corpus.** Every probe, every triple, every mutation the attack scripts perform, and
   the four swaps proved on #120 become one directory of *must refuse* cases with the file and the
   message each must produce; the current book is the *must pass* case.
3. **Extract A and B first**, rule by rule, each with its table; the new runner's findings must be
   a superset of the oracle's on the corpus before a rule is switched over. These are the
   mechanical ones and most of the lines.
4. **Introduce readings** with the rubric, read every chapter fresh (nineteen readings — one
   round, the way the whole-book read #103 was one round), and switch C to *words match a CLEAR
   reading* plus the nine phrases. The patterns are deleted the day the readings exist, not before.
5. **Delete the oracle** when the new runner refuses everything the oracle refuses on the corpus
   and passes the book, and the mutation suites run against the new runner. `LESSONS.md` receives
   the docstrings' history in the same commit.

No dates. The order is the promise: at every step, tier 0 refuses at least what it refuses today.

## 6 · The first cases the new shape must make impossible

| hole | where found | which part closes it |
|---|---|---|
| two anchored values swapped between their nouns | #88, #103, #118, #119 | `anchoring.py` — `beside` (landed on #120), carried over as a rule with its table |
| a removed section returns green under a borrowed beat number | #121 attack (a) | `contract.py` — a split is declared in the outline, or it is a hole |
| a history figure is free prose on two surfaces and they can disagree | #121 attack (b) | `manifest.py` — a figure stated twice is declared once and both surfaces render it, or the appendix says *copy* |
| *toy* greps the file, not the Scope block | #121 attack (c) | `contract.py` — the Scope block is parsed and its own text is tested |
| a registered guess in the first person is unguarded | #105 | a reading finds it (mode 7); a quotation token from the spec closes it |
| relative counts of the book's parts | #83, mode 13 | a reading finds it; `contract.py` refuses a count it can compute |
| a nature claim in words no pattern has met | every tranche | `readings.py` — the words have not been read |

## 7 · What this note does not decide

Three calls are the owner's, and the tranche waits on them:

1. **Review-as-record for prose (§4a), yes or no.** It changes what tier 0 means: the build refuses
   on *this text has not been read*, not on *a regex disliked this sentence*. This note's
   recommendation is yes — it is the honest form of what the repository already does with every
   number — but it puts a reader in the critical path of every prose edit, and that is a cost to
   weigh against the treadmill.
2. **Where readings live and who signs them** — `readings/` in this repository, produced by the
   skill's run and accepted by the owner's merge, is the proposal; a reading in the PR only, not
   committed, is the lighter alternative and loses the ledger.
3. **Whether the nine phrases stay code or data.** Data, in the manifest, with their probes beside
   them, is the proposal; it is the last thing in the checker that is a denylist, and it should be
   visible as one.
