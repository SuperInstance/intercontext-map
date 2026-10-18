# INTERCONTEXT MAP — the legend

**Doctrine:** the substrate is a graph of cells.
**Cartographer:** Kira. **Measured:** 2026-10-18.
**Files:** `intercontext.dot` (drawable), `intercontext.json` (machine-readable), `CLAIMS.md` (the charter's ledger).
**Shape:** 48 nodes · 70 edges · 15 of them boundary crossings.

Both graph files are generated from one Python object, so `intercontext.dot` and
`intercontext.json` cannot disagree. Verified: 48 node ids identical, 70 `(from,to)`
pairs identical, one unreferenced node — `cl.lattice`, the total gap.

---

## How to read it

Nodes are coloured **by kind**. Edges are coloured and weighted **by which
boundary they cross**. That is the whole design: **the drift is at the seams.**

| Node colour | kind | what it is |
|---|---|---|
| blue `#dbeafe` | `cell_model` | a way to be a cell |
| crimson `#ffe4e6` | `witness_dialect` | a way to record that something happened |
| teal `#ccfbf1` | `chain` | the hash algorithm that makes records tamper-evident |
| indigo `#e0e7ff` | `media` | a language/device a model can be hosted on |
| cyan `#cffafe` | `device_feature` | a capability a device has or lacks |
| amber `#fef3c7` | `charter_claim` | a claim in `algebra.md` |
| grey `#f4f4f5` | `organization` | a GitHub account |
| orange `#ffedd5` | `npm_scope` | an npm namespace |
| red `#fee2e2` | `npm_packages` | packages that want a scope they cannot have |
| **dashed grey, double-periphery** | `charter_claim` | **an unimplemented claim — drawn as a gap** |

| Edge | meaning |
|---|---|
| thin grey solid | inside one boundary — model↔model, claim↔claim |
| **crimson** | witness dialect drift — same word, different fields |
| **teal** | chain family — who shares a hash algorithm |
| indigo | media↔model — which device can host which model |
| cyan | device capability, including `cannot-host` / `cannot-reach` |
| blue-violet | claim↔media, claim↔model, claim↔witness |
| **purple, weight 3** | ladder↔witness — *a rung is checkable only if the thing at that rung is witnessed* |
| **crimson-red, weight 4** | **ORGANIZATION boundary crossing** (sketchbook → org) |
| **orange, weight 4** | **NPM boundary crossing** (npm → npm) |
| **dashed grey** | **ADJACENT ONLY — NOT IMPLEMENTED** |

---

## 1. The cell models — two independent axes

The fleet's central disagreement is not about cells. It is about whether a cell
**has a name** or **has a place**. Those are independent, and no model in the
corpus has both.

| model | repo | ID (content hash)? | ADDRESS (a place)? | how it is addressed |
|---|---|---|---|---|
| the spreadsheet cell | `quilt` `packages/core` | no | **yes** | a chosen string, `"compass.heading"` — `types.ts:43` |
| the agent cell | `quilt-claw` | no | **no** | nothing; outputs carry `witness_hash` — `types.ts:21` |
| the openjev ancestor cell | `AI-Writings` | no | **yes** | `Sheet1!C3` via regex — `spreadsheet_plugin.py:23` |
| the game's substrate | `cargo-line-tycoon` | **yes** | no | FNV-1a-64 over `{s,t,b}` — `index.js:69` |
| qthe's lattice cell | `qthe` | no | **yes** | slot + resonance — `qthe.mjs:102` |
| the receipt cell | `labs/quilt-kernel` | partial | no | `activation` hash — `quilt_kernel.py:259` |
| the cell-contract sketch | `quilt-cells` | no | no | `INPUT → OUTPUT+WHY+COST` — `QUILT.md:48` |
| the demo corpus | `cellular-first-design/` | no | partial | contains openjev + spreadsheet |

**The sharpest node in the map:** the game's `computeAddress()`
(`cargo-line-tycoon/substrate/ts/src/index.js:68`) computes a **content hash** of
`{s,t,b}` and assigns it to a field named `this.address` (`:65`). It is the only
model with an ID — and it calls its ID an *address*. The spreadsheet model does
the opposite: its `id` is a place (a string you choose) and it has no hash. The
same word, opposite facts, one repo apart in intent.

Second: `openjev`'s identity is **random** — `self.id = id or f"cell-{uuid.uuid4().hex[:8]}"`
(`cellular-first-design/code/openjev/cell.py:34`) — while its *address* is `Sheet1!C3`
from a **different file** (`code/spreadsheet/spreadsheet_plugin.py:23`). Two files,
two halves of one model, and the brief's source attribution was wrong.

---

## 2. The witness dialects — four, not three

| dialect | where | chained? | carries the result? | fields |
|---|---|---|---|---|
| the charter's | `algebra.md:72` | **yes** (`prev_hash`) | `result?` — *added 2026-10-18* | `tick, op, cell, params, hash, prev_hash, sig` |
| the live canon's | `_worker.js:181` | **NO** (0 hits for `prev_hash`) | only on TICK (`:447`) | `id, opcode, cell_id, timestamp, payload_summary` |
| the labs' | `recorder.py:102` | **yes** | **yes** | `DRAFT{prompt, response, params}` |
| **openjev's** | `cell.py:46` | **no** | **yes** (`value`) | `ts, opcode:'WITNESS', value, source, jev_confidence` |

**openjev's is a fourth dialect and it was not in the brief.** It emits
`opcode: "WITNESS"` (`:50`) and `opcode: "PROOF"` (`:67`) — neither is a charter
opcode — and has no hash field at all.

And a **fifth** vocabulary: `recorder.py:38` defines nine relations —
`TASK ROUTE DRAFT DRAW FOLD KEEP DROP MARK OUTCOME` — which share **zero names**
with the charter's eleven.

### The chains

| chain | algorithm | note |
|---|---|---|
| `recorder.py:41` | fnv1a-64 | the convention the others copy |
| `quilt-claw/witness/ledger.js:42` | fnv1a-64, `GENESIS 0x0000000000000000` | `:6` — *"byte-compatible with it **on purpose**"* |
| `quilt-rust/packages/core/src/ledger.rs:402` | **sha256** | double-entry `input`/`output`; **carries `expected`** |

`ledger.rs:421` holds `pub expected: Option<Value>` and `:426`
`pub imbalance: Option<f64>` — *the prediction the entry was scored against, and
the error.* It is the **only chain in the entire graph that witnesses a
prediction.** Two chains share an algorithm on purpose; the third shares only a
name.

---

## 3. The measurement ladder, wired to the witnesses

`situations/arch/PRICED-STANDARD.md:17` — `step` → `preds` → `labels`, strongest
to weakest, pre-registered tolerance, and the law at `:105`:
**never price a cheaper different answer.**

`:28` is the sentence that makes this a graph and not a document:

> *"Only a witnessed answer can be compared, and only a compared answer can be a standard."*

So each rung gets an edge to the dialect that can carry it:

* `preds` → **the labs' recorder** — `DRAFT` carries `response`, so the `preds` rung is checkable.
* `preds` → **`quilt-rust`'s `LedgerEntry`** — `expected`/`imbalance` realise the rung in code.
* all rungs → **the live canon** — only 6 tick witnesses per tick carry answers (`:447`); every CREATE witness (`:181`) carries none.
* all rungs → **openjev** — carries the value, but no hash, no chain, no tolerance: **not checkable.**

**And the ladder's own citation is a gap.** `PRICED-STANDARD.md:100` says
*"From `PRICED-TRAINING.md` §3.1 and §3.2."* That path has **zero commits** in
the repository — it has never existed. The code is at
`labs/priced-training/priced_training.py`; the document is not.

---

## 4. The devices, and the one cell that calls out

| media | host | what it really has |
|---|---|---|
| TypeScript | spreadsheet, game, agent | 6 of 11 opcodes dispatched (`index.js:80-93`) — the 6th is **ATTEST**, which the charter does not have |
| C99 | game | 5 opcodes (`cell_algebra.c:5-23`) + **2 of the 5 laws** machine-checked (`:26`, `:35`) |
| Python | openjev | 3 opcodes (BIND `:91`, FORGET `:111`, TICK `:121`) |
| Rust | agent-like | **9 of 11**, `COUNT = 9` (`polyformalism/src/lib.rs:86`), with an `Engine` and law checks; `no_std` at `:37` |
| SUBLEQ | game-like | BIND/LINK/EFFECT = **1 instruction**, VIEW = 2, **5 STUBBED** (`lib.rs:26-27`) |

**The remote call-out exists exactly once, and it is mocked.**
`quilt-subleq/src/block.rs:24`:

```
Port { url: String, sig: String, encoding: Encoding }
```

`:117`: *"For now we model it as a local buffer (the remote fetch is mocked)."*
This is the only cell in the whole corpus that names a service — and note that
`sig` here is *a network port's signature* (`:22`), not a cell address. The
charter's `(x, y, t, sig)` has a namesake in the one place that is not about
location at all.

`quilt-cells/QUILT.md:145` asks for the same thing in prose: *"if a cell cannot
fit on the vessel, it doesn't get a smaller model — it gets a **phone call**."*
That repo has **no code** — `QUILT.md` (11,436 B) and `README.md` (1,802 B) are
its entire contents.

And a device that *cannot* do it: `no_std` Rust. `lib.rs:683` — *"No filesystem
in `no_std`."* A cell that cannot read a file cannot fetch a URL.

---

## 5. The scopes and the destinations

### Organizations

| account | type | measure |
|---|---|---|
| **`SuperInstance`** | **`User`, not an Organization** | **5,289 public repos**, created 2024-12-29 |
| `patchwork-quilt` | Organization | 6 repos, created 2026-10-03 — holds the `git.pp` family |
| `purplepincher` | Organization | 19 repos, created 2026-07-02 |
| `si-ready` | Organization | **1 repo** — `.github`, *"the curated shelf for ready-to-use applications"* |

The sketchbook is a **personal account**. That is the first thing on the ladder
and it is not an org.

The dates run backwards: `purplepincher` (2026-07-02) **predates**
`patchwork-quilt` (2026-10-03). And cocapn's work — `cocapn-com-phase1`,
`cocapn-phase1-state-machine`, `cocapn-phase1-provisioning` — went **straight
from the sketchbook to the hardened org**, never touching `patchwork-quilt`.

**`si-ready` is empty in the strict sense**: one repo, an org-profile README.

### npm scopes

| scope | state | evidence |
|---|---|---|
| `@superinstance/*` | **ours, published** | `@superinstance/quilt-tools` HTTP 200, v0.1.1, maintainer `superinstance` |
| `@quilt/*` | **occupied by a third party** | `@quilt/quilt` HTTP 200 v0.4.2, maintainer `kevinklin` (quiltdata). `@quilt/core`, `@quilt/sdk`, `@quilt/ai`, `@quilt/claw` all **404** |
| `quilt-core` (unscoped) | occupied by a stranger | HTTP 200 v0.11.0, maintainer `niko.dellic` — a pane-layout library |
| `@luciddreamer/*` | npm scope **free** | 404 on npm — but the GitHub **User** `luciddreamer` exists (0 repos), the **org does not** |
| `@cocapn/*` | npm scope **free** | 404 on npm; the GitHub account is **404 on both** `/orgs` and `/users` |

**Nine `@quilt/*` packages want a scope they cannot have**: `ai`, `evolve`, `claw`,
`elf`, `nomad`, `rag`, `k3s`, `fleet`, `pincher` — all declared in
`quilt/packages/*/package.json`, none published. And the scope that *is* ours,
`@superinstance/*`, works fine and **none of the nine use it**.

---

## The gaps, drawn as gaps

1. **`cl.lattice` has zero edges.** Not drawn thin, not drawn dashed —
   **isolated, grey, double-periphery, labelled `>>> NO IMPLEMENTATION ANYWHERE <<<`.**
   `algebra.md:64-70` promises a 4D lattice with three projections (TOP x,y /
   FRONT signal / SIDE time). `grep` across all 8 repos for projection triples
   returns nothing. `quilt-claw/examples/04_full_lattice_demo.ts` is 73 lines and
   contains no projection code.

2. **`cl.address` has three edges and every one is dashed and says `ADJACENT ONLY — NOT IMPLEMENTED`.**
   `algebra.md:129` promises `Address = (x, y, t, sig)`. Zero implementations.
   The three edges point at the three models that come *closest* — the
   spreadsheet's string address, qthe's `(slot, x, y, resonance)`, and subleq's
   `Port{url, sig, encoding}` — and each is a different four-tuple. Adjacency is
   drawn; implementation is not, because there is none.

3. **`cl.ladder` points at a file with zero commits.** The edge to
   `situations/arch/PRICED-TRAINING.md` exists in the graph as evidence and is
   marked `ZERO COMMITS`. The ladder is real; its cited source is not.

4. **No edge joins any cell model to any organization.** Not one cell model has
   graduated. `patchwork-quilt` holds `git.pp`; `purplepincher` holds landing
   pages and `constraint-theory-core`. The ladder of accounts and the graph of
   cells do not currently touch. That absence is drawn.

5. **`@luciddreamer/*` and `@cocapn/*` have no edge to any package.** Three repos
   declare those names and none is in this corpus, so the *wanting* is asserted in
   prior measurements, not re-measured here. **Left undrawn rather than asserted.**

---

## What this map could not verify

* **`quilt-substrate-meta`, where `algebra.md:26` says the 5 laws are proved.**
  Not cloned, not measured. `cl.laws` is marked **UNVERIFIED** in the JSON.
* **`quilt-c`, `quilt-py`, `quilt-ts`** are named in `algebra.md:143` but were not
  cloned. The measured media are the four in this graph plus SUBLEQ.
* **Whether any of the 25 `quilt-claw` kind directories is reachable at runtime.**
  Counted, not executed.
* **The seven other language ports** implied by "12 languages" — never enumerated
  anywhere in the corpus.
* **Whether `@quilt/core`'s 404 means "unpublished" or "taken".** The registry
  returns 404 for every `@quilt/*` except `@quilt/quilt`. Emptiness and occupation
  are indistinguishable from the registry alone; the *scope* is quiltdata's either
  way.
* **The body of `wr-obs13-missing-opcodes.md`'s claim that `MERGE` should follow
  `ATTEST`/`CONTEST`/`REVOKE`/`WITHDRAW`** — the doc says "five candidates" and
  then names six.
* **The `@quilt/*` → `@luciddreamer/*` / `@cocapn/*` renames** asserted by prior
  sessions. Out of corpus; not re-measured.

---

*Doctrine: the substrate is a graph of cells. Where the graph has no edge, the
substrate has no claim. Both facts are drawn.*
