## I-Ops

**An agent can decide what to do. It should not also decide whether it was allowed to, whether it
actually did it, or whether it succeeded.**

We build the layer that answers those three questions independently of the worker — and we publish
the parts that make the claim checkable, so you do not have to take our word for any of it.

### What is here

**Start here.** One command, one folder, no account:

```bash
pip install assurance-cli
assurance check ~/reports
```
```
22 of 24 months from 2024-01 to 2025-12 — not in this folder: March 2024, July 2025
Range inferred from filenames: earliest 2024-01, latest 2025-12. Override with --from / --to.
```

| | |
|---|---|
| **[assurance-cli](https://github.com/i-ops-hq/assurance-cli)** · [PyPI](https://pypi.org/project/assurance-cli/) | Point it at a folder. What is missing from a dated or numbered series, and what has silently changed since you last looked. Months, quarters, weeks, days, and numbered runs. Exit codes for CI. |
| **[assurance-core](https://github.com/i-ops-hq/assurance-core)** · [PyPI](https://pypi.org/project/assurance-core/) | The decision modules underneath. Did the worker read everything the task required? Does this document still match its source? Who may produce which effect? **Pure Python, zero dependencies, and no model decides any of it** — enforced by tests that walk the source and fail on a model import. |
| **[assurance-mcp](https://github.com/i-ops-hq/assurance-mcp)** · [PyPI](https://pypi.org/project/assurance-mcp/) | The same checks as MCP tools, for agents that speak the protocol. Read-only by construction, proven by a test rather than promised in a README. |

### The principle

Most agent tooling asks *"is the answer good?"* and answers it with another model. We think the
harder and more useful questions are **was it allowed, did it happen, and was the required evidence
actually read** — and that none of them should be settled by the thing being checked.

So the vocabulary is deliberately narrow. `complete_unverified` rather than `verified_complete` when
no verifier exists. *"22 of 24 were observed"* rather than *"two are missing."* *"I could not check
it"* rather than a pass. **Narrower claims that hold beat broader claims that sound better.**

### Honestly, where this is

These are days old and extracted from a working product, which is not the same as proven. The
library is a decision layer, not a runtime — you still build the machinery that feeds it facts.
**Coverage over the wrong scope is still coverage over the wrong scope**, which is why every result
says how it reached its denominator, so you can disagree with it.

The CLI reads CSV, TSV and XLSX. Staleness needs a baseline you asked it to write. Nobody outside
this company has used any of it yet.

We would rather say so here than have you find out after installing.

**[i-ops.dev](https://i-ops.dev)** · hello@i-ops.dev
