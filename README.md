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

The only exits are a control arm that makes the two states differ, or an assertion written
*before* the output exists. Both are ways of manufacturing a difference where observation
offers none.

### Recent

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
