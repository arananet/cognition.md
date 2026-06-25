# Rationale — Scientific Grounding of COGNITION.md

Every normative rule in [`COGNITION.md`](../COGNITION.md) traces to a
published, peer-reviewed result in cognitive science. This document maps
each spec section to its source and explains *why* the science implies the
engineering constraint. If you only read the spec, you see the rules. If
you read this, you see why breaking them reproduces a known failure of
human memory.

---

## §1 Taxonomy ← Tulving (1972)

**Source.** Tulving, E. (1972). Episodic and semantic memory. In
*Organization of Memory* (pp. 381–403). Academic Press.

**Finding.** Human long-term memory is not one store. Tulving showed
episodic memory (time-stamped personal events — "I saw X at time T") and
semantic memory (context-free facts — "X is true") are functionally
dissociable: they fail independently, encode differently, and are
retrieved by different cues. Later work extended the taxonomy with
procedural memory (skills) and working memory (the active scratchpad).

**Why the rule follows.** An agent that stores every memory in one
undifferentiated vector pool cannot apply a different retention or
retrieval policy to a fleeting tool output versus a durable user
preference — because it has thrown away the distinction at write time.
§1 therefore requires four strata with a stratum assigned **at creation**,
not inferred later. §1.2's demand that an episodic write carry its anchors
is the engineering form of "episodic memory is defined by its
spatio-temporal context."

**Failure mode if ignored.** Stale facts and live events compete on equal
footing in retrieval. This is the agentic analogue of source-monitoring
failure: knowing *that* something is true but not *when* or *whether it
still is*.

---

## §2.2 Depth of processing ← Craik & Lockhart (1972)

**Source.** Craik, F.I.M., & Lockhart, R.S. (1972). Levels of processing:
A framework for memory research. *Journal of Verbal Learning and Verbal
Behavior*, 11(6), 671–684.

**Finding.** Retention is a function of *depth* of encoding, not duration
of exposure. Material processed semantically (for meaning, relation, and
intent) is retained far longer than material processed shallowly (for
surface form). Rote repetition without elaboration ("maintenance
rehearsal") barely improves long-term retention.

**Why the rule follows.** §2.2 makes processing depth an explicit,
recorded property (`shallow` / `structural` / `deep`) and forbids
promoting a memory to a stratum that demands a depth it was never given.
A raw log dumped into a vector store is shallow encoding — it will degrade
into noise. To survive, a memory must be encoded *with* its intent,
causality, and relations, which is exactly what `structural` and `deep`
require.

**Failure mode if ignored.** Memory bloat. The store fills with
shallow-encoded fragments that retrieve poorly and crowd out the few
deeply-encoded memories that matter.

---

## §3 Retrieval ← Roediger & Karpicke (2006)

**Source.** Roediger, H.L., & Karpicke, J.D. (2006). Test-enhanced
learning: Taking memory tests improves long-term retention.
*Psychological Science*, 17(3), 249–255.

**Finding.** The *act of retrieval* strengthens a memory more than
re-studying it does (the "testing effect"). Actively recalling — and
checking — information produces durable retention; passively re-reading
it does not.

**Why the rule follows.** §3.1 requires an explicit retrieval query before
any stateful generation and forbids treating a passive context-dump as a
substitute. §3.2 requires a verification step *after* retrieval — recall
then confirm. This is test-enhanced learning translated directly: the
agent must actively pull and check a memory, not have it passively shoved
into the prompt. §2.3 then resets an episodic memory's TTL on retrieval,
so the testing effect compounds — used memories survive.

**Failure mode if ignored.** Passive injection without verification acts
on memories that may be expired or superseded, and never strengthens the
traces that are actually load-bearing. Recall accuracy decays even as the
store grows.

---

## §4 Degradation ← Ebbinghaus (1885)

**Source.** Ebbinghaus, H. (1885). *Über das Gedächtnis*. Leipzig: Duncker
& Humblot.

**Finding.** Ebbinghaus measured the forgetting curve: retention decays
roughly exponentially with time unless the memory is reinforced, and
spaced reinforcement flattens the curve. Forgetting is not a bug — it is
how a memory system stays relevant by shedding what is no longer
reinforced.

**Why the rule follows.** §4.1 mandates a **finite** episodic TTL and
explicitly rejects "never expires" as a valid episodic policy — a system
that cannot forget cannot stay current. §4.2's pruning triggers (conflict,
obsolescence, redundancy) are controlled forgetting. §2.3's expanding
re-evaluation schedule (1 day → 1 week → 1 month) is spaced repetition —
the documented way to flatten the forgetting curve for memories worth
keeping.

**Failure mode if ignored.** Unbounded accumulation. A store that never
forgets retrieves an ever-larger fraction of irrelevant and contradictory
memories — the agentic analogue of an inability to distinguish current
from outdated knowledge.

---

## §5.2 Cognitive reserve ← Stern (2002)

**Source.** Stern, Y. (2002). What is cognitive reserve? Theory and
research application of the reserve concept. *Journal of the International
Neuropsychological Society*, 8(3), 448–460.

**Finding.** Brains with greater *reserve* — more redundant pathways and
richer connectivity — sustain function despite damage. The same pathology
produces less impairment when more independent routes to the same
capability exist.

**Why the rule follows.** §5.2 requires every **critical** memory to be
reachable through at least two independent retrieval paths and treats a
single-path critical memory as a health-check failure. More anchors per
memory means more routes survive when any single index degrades, embeds
poorly, or drifts. Redundancy is resilience.

**Failure mode if ignored.** A single point of failure in retrieval: when
the one embedding or tag that reaches a critical memory degrades, the
memory becomes effectively lost even though it is still stored.

---

## Summary map

| Spec section | Principle | Source |
|---|---|---|
| §1 Taxonomy | Multiple dissociable memory stores | Tulving (1972) |
| §2.2 Depth-of-processing | Deep encoding outlasts shallow | Craik & Lockhart (1972) |
| §2.3 Spaced re-evaluation | Spacing flattens forgetting | Ebbinghaus (1885) |
| §3 Retrieval | Active recall strengthens traces | Roediger & Karpicke (2006) |
| §4 Degradation | Controlled forgetting keeps memory current | Ebbinghaus (1885) |
| §5.2 Redundancy index | Reserve = redundant pathways | Stern (2002) |

The full reference list lives in the [README](../README.md#references).
