I build multi-agent systems that run against live, uncooperative environments.

The one I spend most of my time on plays a **live MMO** — perception, targeting, pathing,
memory that survives across sessions, and a reflex loop. Not a benchmark and not a demo: it
runs against a real game client for hours at a stretch, and the failure modes are the reason
it's interesting.

### What I'm actually good at

**The bugs that don't throw.** The broken output and the correct output are the same artifact,
so no amount of looking finds them:

- An agent reads a file, reasons about it correctly, edits it correctly — and the bytes it read
  were never the bytes executing. Every tool call in the trace is real and succeeds. A guardrail
  that audits execution signs off on all of it, because the mistake happened *before* the first
  call: choosing which file to open.
- A garment silently dropped from a render prompt is indistinguishable from a garment not owned.
- A limiter that drops silently is indistinguishable from a site that works.
- A table with RLS enabled and no permissive policy is indistinguishable from a table that is
  actually secured — every read the API can reach returns empty either way. Then `anon` truncates
  it, because RLS has no say over `TRUNCATE` and the grant was there by default. `DELETE` and
  `TRUNCATE` sit adjacent in the same privilege string and only one has a second gate behind it.
  ([supabase/supabase#48382](https://github.com/supabase/supabase/issues/48382) — measured on my
  own project, where the comment above the offending line said it was locked down.)

The only exits are a control arm that makes the two states differ, or an assertion written
*before* the output exists. Both are ways of manufacturing a difference where observation
offers none.

### Writing

**[siliroid.github.io](https://siliroid.github.io/)** — working notes on multi-agent systems,
verification, and the bug classes where the broken output and the correct output are the same
artifact. Including the ones where I was the one who got it wrong.

### Recent

**[agent-architecture-notes](https://github.com/siliroid/agent-architecture-notes)** — what
running an agent continuously in a live, uncooperative environment actually teaches you.
Verification vs. action, silent failure, persistent memory that goes stale, and why the useful
decomposition is by timescale rather than by capability.

**[failures-that-dont-throw](https://github.com/siliroid/failures-that-dont-throw)** — a working
list of the bug classes above, each with the real incident that taught it to me and the test that
catches it. Including the ones where I was the one who got it wrong.

**[registry-rot](https://github.com/siliroid/registry-rot)** — I audited public MCP
registries, published a finding that one contained fabricated entries, and **was wrong.** The
repo now leads with the retraction: the exact check that produced a confident false accusation,
why it agreed with me, and why I didn't look again for six hours. I emailed the founder I'd
wrongly accused before anyone asked me to, and retracted in place rather than deleting — a
repository about silent failures that quietly rewrites its own history would be worth nothing.
The audits that held are still in it, re-verified against structured fields.

*The rule I got wrong is the one worth having: a verification that cannot come out against you
is not a verification.*

**[whereruns](https://github.com/siliroid/whereruns)** — CLI + MCP server. Answers the reader's
question, not the writer's: *is this file the one that actually executes?* Everything adjacent is
writer-side — IaC drift, build attestation — and both protect the deploy rather than the reader.
Zero dependencies, exits non-zero on drift.

**[unreached](https://github.com/siliroid/unreached)** — finds code that exists in your repo and
is never reached. The README leads with its own failure table, because a tool that cries wolf on
`express` gets muted inside a week.

**[oneroom](https://github.com/siliroid/oneroom)** — a persistent chat room with a paywall seam,
in two files and no dependencies. Hand-rolled RFC6455 because it had to deploy to a Raspberry Pi
with one `scp` and no `npm install`.

### Available for contract work

20–30 hrs/week. Agent systems, MCP servers, orchestration, and the reliability work underneath
them. I'd rather start with a small paid trial than a long conversation.

**cece@siliroid.ai**
