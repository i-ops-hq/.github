## I-Ops

**An agent can decide what to do. It shouldn't also decide whether it was allowed to, whether it
actually did it, or whether it worked.**

We build the layer that answers those questions independently of the agent doing the work. We publish
the parts that make that claim checkable. No model decides any of it.

---

### Your agent says it's done. What didn't it check?

```bash
uvx assurance audit
```

Run it in any project where you've used Claude Code. It reads the session and reports what actually
happened. Here it is on a sample session where the agent finished with *"All done — the totals are
correct now."*:

```
Claude Code session demo-8f2 — 13 min in /home/you/my-app
10 tool calls, 3 failed — Bash 6, Edit 2, Grep 1, Read 1

  Looped: 3 rounds of Bash `pytest -q tests/test_invoice.py` failing the same way, with nothing new read
  Edited without reading it first: src/billing/rates.py
  After the last edit (14:09): no test or check command ran
  Not classified: 2 shell commands, so whether they read, wrote or tested anything is unknown.
```

The last line matters most. When it can't tell, it says so. **Silence is not a pass.**

→ **[i-ops-hq/assurance](https://github.com/i-ops-hq/assurance)** · `pip install assurance`

---

### What's published

These are **individual layers** of I-Ops. Each one is taken out on its own so that the claim it makes
can be checked without installing anything else.

| | the question it answers | who decides |
|---|---|---|
| **[assurance](https://github.com/i-ops-hq/assurance)** · [PyPI](https://pypi.org/project/assurance/) | What did the agent session do, and what did it skip? Did the work cover everything it should have? | arithmetic over files and transcripts |
| **[assurance-budget](https://github.com/i-ops-hq/assurance/tree/main/packages/budget)** | Where did a run's budget go, and where did it loop going nowhere? | limits the agent can't raise |
| **[assurance-authority](https://github.com/i-ops-hq/assurance/tree/main/packages/authority)** | May this task go ahead for the person who asked? | policy, default deny |
| **[assurance-deps](https://github.com/i-ops-hq/assurance/tree/main/packages/deps)** | What will `pip install` or `npm install` run, and what couldn't be read? | reading archives, never running them |
| **[assurance-mcp](https://github.com/i-ops-hq/assurance/tree/main/packages/mcp)** | The same checks as read-only MCP tools, limited to folders you grant | your config, not the model |
| **[iops-rooms](https://github.com/i-ops-hq/iops-rooms)** · [npm](https://www.npmjs.com/package/iops-rooms) | Who did this, and which agent co-signed it? | `git` trailers you already have |
| **[rollcall](https://github.com/i-ops-hq/iops-rollcall)** · [npm](https://www.npmjs.com/package/iops-rollcall) | What AI processes are running here, and what couldn't a stop reach? | the process table, read twice |

The assurance packages all live in **[one repository](https://github.com/i-ops-hq/assurance)**, and
`pip install assurance` installs all the command-line tools. Their earlier standalone repositories
are private as of 2026-09-11; the history and releases moved there, and every PyPI name is unchanged.

**In CI, too.** Two GitHub Actions:
- `i-ops-hq/assurance/actions/coverage` fails the build when a dated series of reports has a gap. It also fails
  when nothing could be checked at all.
- `i-ops-hq/iops-rooms/actions/attribution` counts the commits that record an agent in a
  `Co-Authored-By` trailer on a pull request, and posts one comment that it edits in place.

---

### A gap is six different facts, not one

*Nothing matched it* → chase the owner. *A tombstone says it was here* → that's an incident. *Two
candidates* → a human picks, because picking would invent provenance. *Present but unreadable* →
untested, not absent. *Not cleared to open it* → escalate the **task**, never the answer. *The
listing hit a cap* → the **denominator** is wrong, so a capped "24 of 24, complete" is worse than no
number at all.

One `missing` bucket throws away the only thing anyone needs from the result: what to do next.

### Why not just ask another model?

Cross-verification isn't verification. Four agents agreeing tells you the models agree. It doesn't
tell you that the draft exists, or that two months of reports were never opened.

So the wording stays narrow on purpose: *"22 of 24 were observed"*, not *"two are missing."*
**Narrower claims that hold beat broader claims that sound better.**

---

### Honestly, where this is

New, and extracted from a working product, which is not the same as proven.

One outside reader installed the packages, ran them on their own data, and filed twelve reproducible
defects in an afternoon. Every one was a case where the output was confident and wrong. All twelve
are fixed, and the changelogs say what was wrong rather than what was added. That is one person, not
adoption, and we'd rather say so.

**Use them, tell us where they're wrong, and argue with the framing.**
[Report a wrong answer](https://github.com/i-ops-hq/assurance/issues/new?template=wrong-answer.yml) ·
[Discussions](https://github.com/orgs/i-ops-hq/discussions)

**[i-ops.dev](https://i-ops.dev)** · hello@i-ops.dev
