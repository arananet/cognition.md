# ADR 0002: Consolidation Must Pass Through Episodic Memory

**Date:** 2026-06-25
**Status:** Accepted

---

## Context

COGNITION.md v0.1 §1 defines four memory strata (working, episodic,
semantic, procedural). A natural simplification would be three strata —
collapse episodic into semantic, since "facts the agent knows" feels like
one category. Several production memory systems effectively do this: an
extraction step reads the conversation and writes generalized facts
straight into a vector store.

§2.1 of the spec deliberately forbids this. It requires that **no memory
reaches semantic or procedural memory without first existing as an
episodic record**. Working memory cannot write directly to semantic
memory. This ADR records why that ordering is mandatory rather than
optional, and why four strata beat three.

The forces at play:

- **Provenance.** A semantic fact ("user prefers email") is only
  trustworthy if you can say *where it came from*. §1.3 requires
  provenance on every semantic write.
- **Confirmation.** Promoting a single off-hand statement to a durable
  "fact" is how agents acquire confident-but-wrong beliefs. §2.1 requires
  corroboration (2+ episodes) or an explicit authoritative source.
- **Conflict resolution.** When two facts disagree (§4.2), you need the
  originating events to decide which is current. If episodes were never
  stored, that evidence is gone.
- **Simplicity pressure.** Three strata are less to implement. The cost of
  the fourth must justify itself.

## Decision

Consolidation is strictly ordered: **working → episodic → {semantic,
procedural}**. Direct working-to-semantic writes are prohibited.

Episodic memory is retained as a distinct stratum — not merged into
semantic — because it serves a role semantic memory cannot: it is the
time-stamped, anchored evidence trail from which semantic facts are
derived and against which they are later verified (§5.3) and adjudicated
on conflict (§4.2).

A semantic or procedural memory is therefore always a *claim with a
citation*: it points back to the episode(s) that justify it.

## Consequences

**Easier:**

- Every durable fact is auditable — you can trace it to the events that
  produced it.
- Conflicts are resolvable by recency and source, because the sources
  still exist as episodes.
- Verification (§5.3) has ground truth to check against: recent episodes.
- Hallucinated "facts" with no episodic basis are structurally
  impossible — a semantic write without provenance is rejected.

**Harder:**

- More storage and an extra hop. A fact must be written as an episode
  first, then promoted, rather than extracted in one shot.
- Implementations must run a promotion step (§2.1) rather than writing
  facts inline.

**Trade-off accepted:** We pay the cost of a fourth stratum and a
two-step write path to buy auditability and conflict-resolvability. For a
memory system whose core failure mode is *confidently retaining outdated
or unfounded beliefs*, that evidence trail is the point — collapsing it
away to save a hop reintroduces exactly the failure the spec exists to
prevent.

This mirrors the human dissociation Tulving (1972) documented: episodic
and semantic memory are separate systems, and semantic knowledge is
abstracted *from* episodic experience — not stored in its place. See
[`docs/RATIONALE.md`](../RATIONALE.md) §1.
