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

### These are layers, not products

I-Ops is one system. What is published here are **individual layers of it**, taken out one at a time
so the claim each one makes can be checked without installing anything else — and so people who have
no interest in the rest can still use the part that answers their question.

Most of the system is not here and may never be. What is here is what could stand alone and be
argued with.

| layer | the question it answers | who decides |
|---|---|---|
| **[assurance](https://github.com/i-ops-hq/assurance)** · [PyPI](https://pypi.org/project/assurance-cli/) | What was supposed to be there, and what silently changed? | arithmetic over files |
| **[assurance-budget](https://github.com/i-ops-hq/assurance/tree/main/packages/budget)** · [PyPI](https://pypi.org/project/assurance-budget/) | Where did a run's budget go, and where did the loop go nowhere? | ceilings a caller cannot raise |
| **[assurance-authority](https://github.com/i-ops-hq/assurance/tree/main/packages/authority)** · [PyPI](https://pypi.org/project/assurance-authority/) | May this task proceed, for the person who asked? | policy, default deny |
| **[iops-rooms](https://github.com/i-ops-hq/iops-rooms)** · [npm](https://www.npmjs.com/package/iops-rooms) | Who did this, and which agent co-signed it? | `git` trailers you already have |

All four live in **[assurance](https://github.com/i-ops-hq/assurance)** — one repository, five
packages. `assurance-core`, `assurance-mcp`, `assurance-budget` and `assurance-authority` were
published from repositories of their own first; those are archived rather than deleted, so their
URLs still resolve, and every PyPI package name is unchanged.

**No model decides any of it.** That is the property the four have in common and the reason they are
worth publishing separately: each is a fact you can recompute yourself.

---

### Why any of this is public

Not as a funnel. These are early, and the fastest way to find out where a claim is thin is to let
people who did not write it try to break it.

So: **use them, tell us where they are wrong, and argue with the framing.** Issues are open on every
repository. A criticism that lands changes the product — several already have, and the packages
carry the corrections in their changelogs rather than quietly in a later version.

That is how the work gets better, and it is how the field gets better. Nobody is served by a
category everyone describes and nobody checks.

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
