## I-Ops

**An agent can decide what to do. It shouldn't also decide whether it was allowed to, whether it
actually did it, or whether it worked.**

We build the layer that answers those independently of the worker, and we publish the parts that
make the claim checkable.

---

### Every tool call returned 200. The answer was still built on two thirds of the data.

Groundedness checks the **output against the input**. Almost nothing checks the **input against the
question** — so a retriever returns 4 of the 9 documents a question spans, the model writes a fluent,
fully grounded answer, every span is green, and the answer is wrong.

```bash
pip install assurance-cli

assurance diff --expected corpus.txt --found retrieved.json \
  --scope "documents the question spans" --where "the retrieved set" --fail-on-gap
```
```
2 of 5 documents the question spans — not in the retrieved set: doc-2, doc-3, doc-5
also present and not expected: doc-9
```

Keys are anything you can name, so the same command gates a **code review** against
`git diff --name-only`, an **ETL run** against its declared partitions, an **eval** against its
declared cases, or a **compliance pull** against the controls in scope.

No account, no key, no network call, and no model decides any of it.

---

### The three packages

| | |
|---|---|
| **[assurance-cli](https://github.com/i-ops-hq/assurance-cli)** · [PyPI](https://pypi.org/project/assurance-cli/) | The checks as a command. `diff` for any two sets, `check` for a series on disk, a baseline for what changed while you weren't looking. Exit codes for CI. |
| **[assurance-core](https://github.com/i-ops-hq/assurance-core)** · [PyPI](https://pypi.org/project/assurance-core/) | The arithmetic underneath. Pure Python, zero dependencies, and **no model decides any of it** — enforced by tests that walk the source and fail on a model import. |
| **[assurance-mcp](https://github.com/i-ops-hq/assurance-mcp)** · [PyPI](https://pypi.org/project/assurance-mcp/) | The same checks as MCP tools. An agent can't audit its own reading; this answers it from outside. Read-only by construction, proven by a test. |

---

### A gap is six different facts, not one

*Nothing matched it* → chase the owner. *A tombstone says it was here* → that's an incident. *Two
candidates* → a human picks, because picking invents provenance. *Present and unreadable* → untested,
not absent. *Not cleared to open it* → escalate the **task**, never the answer. *The listing hit a
cap* → the **denominator** is wrong, so a capped "24 of 24, complete" is worse than no number at all.

One `missing` bucket throws away the only thing anyone needs from the result: what to do next.

---

### Why not just ask another model

Cross-verification isn't verification. Four agents agreeing tells you the models agree — not that
the draft exists, or that two months were never opened.

So the vocabulary stays narrow on purpose. `complete_unverified` rather than `verified_complete`
when no verifier exists. *"22 of 24 were observed"* rather than *"two are missing."* **Narrower
claims that hold beat broader claims that sound better.**

---

### Honestly, where this is

Days old, extracted from a working product, which is not the same as proven. It's a decision layer,
not a runtime — you still build the machinery that feeds it facts.

**It will not invent your expected set.** That's deliberate: a denominator a tool chooses for you is
a denominator nobody can argue with. And coverage over the wrong scope is still coverage over the
wrong scope, which is why every result states how it reached its denominator.

**Nobody outside this company has used any of it yet.** We'd rather say so here than have you find
out after installing.

**[i-ops.dev](https://i-ops.dev)** · hello@i-ops.dev
