## I-Ops

**An agent can decide what to do. It should not also decide whether it was allowed to, whether it
actually did it, or whether it succeeded.**

We build the layer that answers those three questions independently of the worker — and we publish
the parts that make the claim checkable, so you do not have to take our word for any of it.

### Every tool call returned 200. The answer was still built on two thirds of the data.

Groundedness checks the **output against the input**. Almost nothing checks the **input against the
question**. So a retriever returns the top 4 of the 9 documents a question spans, the model writes a
fluent and fully grounded answer, every span is green, and the answer is wrong.

```bash
pip install assurance-cli
```

```bash
# Did the retriever see the whole question?
assurance diff --expected corpus-for-this-question.txt --found retrieved.json \
  --scope "documents the question spans" --where "the retrieved set" --fail-on-gap
```
```
2 of 5 documents the question spans — not in the retrieved set: doc-2, doc-3, doc-5
also present and not expected: doc-9
```

Keys are anything you can name, so the same command gates a code review against
`git diff --name-only`, an ETL run against the partitions it declared, an eval against its declared
cases, or a compliance pull against the controls in scope.

```bash
# Or point it at a folder and let it derive the expected set from the filenames
assurance check ~/reports
```
```
22 of 24 months from 2024-01 to 2025-12 in reports — not in this folder: March 2025, July 2025
— Range inferred from filenames: earliest 2024-01, latest 2025-12. Override with --from / --to.
```

No account, no key, no network call, and no model decides any of it.

| | |
|---|---|
| **[assurance-cli](https://github.com/i-ops-hq/assurance-cli)** · [PyPI](https://pypi.org/project/assurance-cli/) | The checks as a command. `diff` for any two sets of keys; `check` for a dated or numbered series on disk; a baseline for what has silently changed since you last looked. Exit codes for CI. |
| **[assurance-core](https://github.com/i-ops-hq/assurance-core)** · [PyPI](https://pypi.org/project/assurance-core/) | The decision modules underneath. Did the worker read everything the task required? Does this document still match its source? Who may produce which effect? **Pure Python, zero dependencies, and no model decides any of it** — enforced by tests that walk the source and fail on a model import. |
| **[assurance-mcp](https://github.com/i-ops-hq/assurance-mcp)** · [PyPI](https://pypi.org/project/assurance-mcp/) | The same checks as MCP tools, for agents that speak the protocol. An agent cannot audit its own reading; this answers it from outside. Read-only by construction, proven by a test rather than promised in a README. |

### Six ways an expectation fails to be evidence, and they are not the same fact

Most tools give you one `missing` bucket, which destroys the only thing anyone needs from the
result — **what to do next**.

*Nothing matched it* sends you to the owner. *A tombstone says it was here* is an incident. *Two
candidates* means a human names the real one, because picking invents provenance. *Present and
unreadable* means untested, not absent. *You are not cleared to open it* escalates the **task**,
never the answer. And *the listing hit a cap* means the **denominator** is wrong, so every ratio
above it is a guess — which is why a capped "24 of 24, complete" is worse than no number at all.

### The principle

Most agent tooling asks *"is the answer good?"* and answers it with another model. Cross-verification
is not verification: four agents agreeing tells you the models agree, not that the draft exists or
that two months were never read.

So the vocabulary is deliberately narrow. `complete_unverified` rather than `verified_complete` when
no verifier exists. *"22 of 24 were observed"* rather than *"two are missing."* *"I could not check
it"* rather than a pass. **Narrower claims that hold beat broader claims that sound better.**

### Honestly, where this is

Days old, extracted from a working product, which is not the same as proven. The library is a
decision layer, not a runtime — you still build the machinery that feeds it facts. **Coverage over
the wrong scope is still coverage over the wrong scope**, which is why every result states how it
reached its denominator, so you can disagree with it.

It will not invent your expected set. That is on purpose: a denominator a tool chooses for you is a
denominator nobody can argue with.

Nobody outside this company has used any of it yet. We would rather say so here than have you find
out after installing.

**[i-ops.dev](https://i-ops.dev)** · hello@i-ops.dev
