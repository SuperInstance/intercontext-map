# CLAIMS — `algebra.md` against the corpus

**The charter:** `SuperInstance/AI-Writings/algebra.md`, 5,294 B.
*(The brief calls this repo `AI-Writings`; the GitHub org `AI-Writings` is 404.
The repo is `SuperInstance/AI-Writings`.)*

**Its history: 2 commits.**
* `3807557abd` — 2026-09-21T14:46:45Z — created
* `1da6180bb7` — 2026-10-18T07:46:49Z — *"Make the witness carry the answer"*, +13 lines

The brief describes it as "ONE commit, ever". It had one until **two hours
before this map was measured.** Every row below that touches the witness was
measured after that change and is annotated.

"Implemented value" means a **dispatch site**, `case`/method/`const` that computes
the thing — not a mention in a comment or a string.

| # | claim | documented | implemented | where measured | source of the discrepancy |
|---|---|---|---|---|---|
| 1 | **11 opcodes** | 11 (5 + 6 adopted) `algebra.md:6,13` | **there are three different elevens** | see below | see below |
| 2 | **26 kinds** | 26 `algebra.md:45` | **25 / 25 / 24** | `quilt-claw/src/cells-*/` = 25 dirs · `quilt-claw-cells-game/index.html:156` array = 25 ids · `_worker.js:21` `KIND_WHITELIST` = 24 | the doc is the only source for 26; the three implementations each lost a kind, and **the validator lost a different one** |
| 3 | **`Address = (x, y, t, sig)`** | 4D navigation without a map `algebra.md:129` | **ZERO** | `grep -rInE` over `.ts .js .py .rs .c .mjs` in 8 repos: no hits | never implemented. The three nearest things are three *different* tuples |
| 4 | **4D lattice, three projections** | TOP x,y / FRONT signal / SIDE time `algebra.md:64-70` | **ZERO** | no FRONT/SIDE/TOP triple anywhere; `examples/04_full_lattice_demo.ts` is 73 lines with none | never implemented |
| 5 | **Cell ID = hash of (kind, payload, parents)** | no separate ID service `algebra.md:128` | **one model, three fields** | `cargo-line-tycoon/substrate/ts/src/index.js:69` hashes `{s,t,b}` = state, type, behaviour — **`parents` is never hashed** | the only model with an ID hashes a different triple, and stores it in a field named `address` |
| 6 | **Polyformalism = same model, 12 languages** | 12 `algebra.md:142` | **5 media measured, 4 of them partial** | TS 6/11 · C99 5/11 · Python 3/11 · Rust **9/11** · SUBLEQ 4 real / 5 stubbed | no implementation or document in the corpus ever enumerates 12; `algebra.md:143` and `_worker.js:16` both assert it |
| 7 | **Don't store cells in a DB** | witness log + derived state `algebra.md:133` | **violated in the sketchbook** | `quilt-claw/src/store.ts:19` `KnowledgeStore`, `:20` `entries = new Map`, `:21` `topicIndex` | the prohibition was written in the charter and broken in the repo that carries the charter's kinds |
| 8 | **5 laws, proved in `quilt-substrate-meta`** | 5, proved `algebra.md:26` | **2 of 5 machine-checked — in other repos** | `cargo-line-tycoon/substrate/c/cell_algebra.c:26` `law_L1_bind_idempotence`, `:35` `law_L3_view_purity`; `quilt-rust/crates/quilt-polyformalism/src/lib.rs:212` `law_bind_idempotent` | **`quilt-substrate-meta` was not cloned and not measured — UNVERIFIED.** The laws that are checked live in a tycoon and a Rust crate. `:35` is labelled **L3**; the charter numbers VIEW-pure as **law 4** |
| 9 | **The witness log IS the substrate of truth** | append-only, chained `algebra.md:60` | **the live canon contradicts it** | `_worker.js:192` `WITNESS_LOG.push(witness)` — an in-memory array; `grep -c prev_hash _worker.js` = **0** | the canon's witness has no chain field and dies with the isolate |
| 10 | **the measurement ladder** `step → preds → labels` | strongest to weakest, `PRICED-STANDARD.md:17` | **the code exists; the cited document never did** | `situations/arch/PRICED-STANDARD.md:100` cites `PRICED-TRAINING.md` — **0 commits on that path, ever**; the code is `labs/priced-training/priced_training.py` | a dangling citation. The ladder is real and load-bearing; the file it is quoted from has never existed in the repository |
| 11 | **a witness must carry the answer** *(new, `algebra.md:88`)* | `result?: any` | **2 of 4 dialects, 2 h old** | `_worker.js:447` (TICK only; CREATE witnesses at `:181` have none) · `recorder.py:104` carries `response` · `openjev/cell.py:51` carries `value` but unchained | the field was added to the charter at `1da6180` on 2026-10-18; the canon adopted it for tick witnesses only |

---

## Row 1 in full: three different elevens

| # | where | the eleven | written |
|---|---|---|---|
| A | `algebra.md:6,13` | BIND LINK EFFECT VIEW TICK **FORGET PROOF ROUTE CRDT WORLD TIME** | `3807557abd` — 2026-09-21T**14:46:45**Z |
| B | `cellular-first-design/reports/wr-obs13-missing-opcodes.md:9,11,13,15,17` (3,583 B) | **ATTEST CONTEST REVOKE WITHDRAW MERGE DELEGATE** | `46766b5605` — 2026-09-21T**22:01:18**Z |
| C | `cargo-line-tycoon/tests/sim/massive_simulation.js:16` | ATTEST CONTEST **MERGER** REVOKE WITHDRAW DELEGATE BIND LINK EFFECT VIEW TICK | `595f6f32` — 2026-10-03 |

A and B are **7 h 14 m 33 s apart** on the same day. C is a **third** list,
three weeks later, and it is not B: B says **`MERGE`**, C says **`MERGER`** — one
letter, two vocabularies. C also merges B's six into A's five, so the fleet's
simulation dispatches an eleven that is neither A nor B.

`wr-obs13:17` says *"Five candidates"* and then names six.

### Corrections to the brief, measured

| brief says | measured | evidence |
|---|---|---|
| "implemented 0 Rust" | **9 of 11**, `COUNT = 9`, with an `Engine` and law checks | `quilt-rust/crates/quilt-polyformalism/src/lib.rs:86`, `:195` `ALL_OPS: [Op; 9]`, `:212` `law_bind_idempotent` |
| "`CONTEST MERGER REVOKE DELEGATE WITHDRAW` never implemented anywhere" | **implemented** — `ATTEST:229`, `CONTEST:242`, `WITHDRAW:251` | `cargo-line-tycoon/substrate-bundle/substrate-sdk.js` (11,375 B) |
| "`algebra.md` ONE commit, ever" | **2 commits** | `1da6180bb7`, 2026-10-18T07:46:49Z |
| "charter witness has **no result**" | has `result?: any` — added ~2 h before this map | `algebra.md:88`, from `1da6180bb7` |
| "`PRICED-TRAINING.md`" | **zero commits**; the file is `PRICED-STANDARD.md` | `situations/arch/PRICED-TRAINING.md` — 404 on the commits API |
| "`AI-Writings/…`" | the org is 404; the repo is `SuperInstance/AI-Writings` | `api.github.com/orgs/AI-Writings` |
| "there are **five** witness dialects' worth of disagreements — three" | **four** dialects; openjev's was not listed | `cellular-first-design/code/openjev/cell.py:46-53` |
| "`parse_address` in `openjev/cell.py`" | it is in `code/spreadsheet/spreadsheet_plugin.py:23` | openjev/cell.py has the *identity*, the plugin has the *address* |

---

## Unimplemented claims, drawn as nodes with no edge

| claim | why it is empty |
|---|---|
| **`cl.lattice`** — the 4D lattice, three projections | **zero implementations in 8 repos.** Drawn isolated in `intercontext.dot`: dashed, double-periphery, grey, labelled `>>> NO IMPLEMENTATION ANYWHERE <<<`. It is the only node in the graph with no edge at all. |
| **`cl.address`** — `Address = (x, y, t, sig)` | **zero implementations.** It has three edges, every one dashed and labelled `ADJACENT ONLY — NOT IMPLEMENTED`: to the spreadsheet cell (`types.ts:43`, a chosen string), to qthe (`qthe.mjs:102`, `(slot, x, y, resonance)`), to subleq (`block.rs:24`, `Port{url, sig, encoding}`). Three different tuples, none of them the charter's, and the one that owns the word `sig` is a network port. |

## Unverified

| node | what is missing |
|---|---|
| `cl.laws` | **`quilt-substrate-meta` was not cloned.** `algebra.md:26` says the 5 laws are proved there. Marked `"verified": false` in `intercontext.json`, listed under `.unverified`. `quilt-c` is likewise named at `algebra.md:143` and never opened. |

---

*A claim with no edge is not a defect. It is a sketch of a claim. Both are drawn.*
