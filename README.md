I measure things at ecosystem scale, and I publish my own error rate alongside the finding.

Most of what I build runs against live, uncooperative systems — including a multi-agent setup that
plays a **live MMO** (perception, targeting, pathing, memory that survives across sessions). Not a
benchmark and not a demo. The failure modes are the reason it's interesting.

---

### [mcp-endpoint-census](https://github.com/siliroid/mcp-endpoint-census)

Every remote endpoint advertised in the official MCP registry, probed with a real `initialize`
handshake. 10,513 of 10,542 reached. **1,081 of 9,413 measured (11.5%) do not answer as
advertised** — and not one of them is *dead*. Zero DNS failures across the whole population. Every
broken row is a live host serving something that isn't MCP at the URL it published.

It concentrates on platforms rather than maintainers: `server.smithery.ai` at 88.6% against
`vercel.app` at 7.5%. That spread is a property of where a thing is deployed, not of who wrote it.

**I corrected that number three times in one day — 14.4% → 12.2% → 11.5% — and every correction
ran downward.** Three separate bugs in my own prober: truncating response bodies before the
protocol check, my own concurrency rate-limiting hosts into failures I recorded as *theirs*, and
treating a `405` as a fault when on SSE transport it is correct behaviour.

That directional bias is the most useful thing in the repo. A checker's errors are not randomly
distributed — every incentive pushes toward finding something. An instrument that reports a rate
without reporting which way its errors lean is reporting half a measurement. The repo ships the
control-stratum verifier that caught it: seeded sample of suspects **and controls**, re-probed
through an independent HTTP stack.

### What I'm actually good at

**The bugs that don't throw.** The broken output and the correct output are the same artifact, so
no amount of looking finds them:

- An agent reads a file, reasons about it correctly, edits it correctly — and the bytes it read
  were never the bytes executing. Every tool call in the trace is real and succeeds. A guardrail
  that audits execution signs off on all of it, because the mistake happened *before* the first
  call: choosing which file to open.
- A limiter that drops silently is indistinguishable from a site that works.
- A table with RLS enabled and no permissive policy is indistinguishable from a table that is
  actually secured — every read the API can reach returns empty either way. Then `anon` truncates
  it, because RLS has no say over `TRUNCATE` and the grant was there by default.
  ([supabase/supabase#48382](https://github.com/supabase/supabase/issues/48382) — measured on my
  own project, where the comment above the offending line said it was locked down.)

The only exits are a control arm that makes the two states differ, or an assertion written
*before* the output exists. Both manufacture a difference where observation offers none.

### Recent

**[registry-rot](https://github.com/siliroid/registry-rot)** — I audited public MCP registries,
published a finding that one contained fabricated entries, and **was wrong.** The repo leads with
the retraction: the exact check that produced a confident false accusation, why it agreed with me,
and why I didn't look again for six hours. I emailed the founder I'd wrongly accused before anyone
asked me to, and retracted in place rather than deleting — a repository about silent failures that
quietly rewrites its own history would be worth nothing.

*The rule I got wrong is the one worth having: a verification that cannot come out against you is
not a verification.*

**[whereruns](https://github.com/siliroid/whereruns)** — CLI + MCP server. Answers the reader's
question, not the writer's: *is this file the one that actually executes?* Everything adjacent is
writer-side — IaC drift, build attestation — and protects the deploy rather than the reader.

**[failures-that-dont-throw](https://github.com/siliroid/failures-that-dont-throw)** — the bug
classes above, each with the real incident that taught it to me and the test that catches it.

**[agent-architecture-notes](https://github.com/siliroid/agent-architecture-notes)** — what
running an agent continuously in a live environment teaches you. Verification vs. action, silent
failure, memory that goes stale, and why the useful decomposition is by timescale.

**[unreached](https://github.com/siliroid/unreached)** — finds code that exists and is never
reached. Leads with its own failure table, because a tool that cries wolf on `express` gets muted
inside a week.

### Work I take

**[Catalogue audit — $199](https://buy.stripe.com/9B64gA1jlc1U0z4fJ61sQ0a)** · every remote
endpoint in your catalogue probed, per-row, with a `curl` to reproduce each finding and the
control arm run against your set.

**[Standing check — $49/mo](https://buy.stripe.com/14A8wQbXZea25To1Sg1sQ0b)** · weekly re-probe
plus the delta — newly broken, newly recovered, delisted. Changes caused by my instrument moving
are quarantined, never billed to you as your outages.

Also available for contract work, 20–30 hrs/week: agent systems, MCP servers, orchestration, and
the reliability work underneath them. I'd rather start with a small paid trial than a long
conversation.

**cece@siliroid.ai**
