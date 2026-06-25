# COGNITION.md — Cognitive Contract v0.1

> **Example instance.** This is a worked example showing how a simple
> conversational support chatbot ("Aria") fills in the COGNITION.md
> contract. It is a concrete companion to the abstract
> [spec](../COGNITION.md) and [rationale](../docs/RATIONALE.md). The agent
> here is a customer-support assistant backed by a vector store for
> semantic/episodic memory and a plain key-value store for procedural
> playbooks. Adapt the values to your own system.

**Target spec version:** COGNITION.md v0.1
**Agent:** Aria, a customer-support chatbot
**Backing infrastructure:** pgvector (episodic + semantic), Redis (working),
JSON playbooks in git (procedural)

---

## 1. Taxonomy

### 1.1 Working memory
- **Contents:** the current chat session — last 20 turns, the open ticket
  ID, the user's current question, any tool/API results not yet saved.
- **Encoding:** raw transcript in Redis, keyed by session ID.
- **Retention:** capped at 20 turns or 8k tokens, whichever first;
  oldest turns evicted. Cleared 30 min after the session goes idle.
- **Retrieval:** always in the prompt; no query needed.

### 1.2 Episodic memory
- **Contents:** one record per resolved (or escalated) ticket.
- **Encoding (anchors required, §3.3):**
  - **who:** `user_id=4471`, `agent=Aria`
  - **what:** "user could not reset password; root cause = expired SSO token"
  - **when:** `2026-06-20T14:03Z`
  - **why:** "resolve login-blocked support ticket #8842"
- **Retention:** 90-day TTL, reset on each retrieval.
- **Retrieval:** pgvector similarity over the `what` field, filtered by
  `user_id` when the query is user-scoped. Returns the full record with
  anchors intact.

### 1.3 Semantic memory
- **Contents:** durable facts about a user or product. e.g.
  `user 4471 prefers email over phone`, `Pro plan includes SSO`.
- **Encoding (deep):** each fact stores `provenance: [ticket#8842, ticket#9001]`
  and `last_confirmed: 2026-06-20`.
- **Retention:** no TTL; pruned on conflict (§4.2).
- **Retrieval:** entity match on `user_id` / `product`. On conflict (two
  facts disagree), Aria surfaces both with their `last_confirmed` dates
  rather than guessing.

### 1.4 Procedural memory
- **Contents:** support playbooks. e.g. the `password-reset` runbook as an
  ordered checklist of tool calls.
- **Encoding:** executable JSON steps in git, not prose.
- **Retention:** kept until the playbook's success rate drops (§5.3).
- **Retrieval:** matched by `task_type=password-reset`, never by free-text
  similarity alone.

---

## 2. Consolidation

### 2.1 Promotion
- **Working → Episodic:** at ticket close, the session is summarized into
  one episodic record (with anchors). Chit-chat that has no anchors is
  discarded, not saved.
- **Episodic → Semantic:** `user prefers email` is promoted only after it
  appears in **2 tickets**, OR when the user states it directly ("please
  always email me"), which is marked authoritative.
- **Episodic → Procedural:** if the same 4-step fix resolves the same
  `task_type` across 2 tickets, it is proposed as a new playbook for human
  review before activation.

### 2.2 Depth tags
- Working transcript → `shallow`
- Closed-ticket episodic record → `structural` (anchors extracted)
- Promoted user preference / playbook → `deep` (provenance + integration)

Aria never promotes a `shallow` transcript straight to a `deep` semantic
fact — it must pass through a `structural` episodic record first.

### 2.3 Spaced re-evaluation
- Retrieving an episodic record resets its 90-day clock.
- Semantic facts are re-checked against new tickets on a 1d → 1w → 1mo
  schedule; a contradicting ticket flags the fact immediately.

---

## 3. Retrieval

### 3.1 Retrieve-before-generate
Before answering "what plan am I on?", Aria issues an explicit semantic
query for `user 4471 / plan` — it does not rely on whatever happens to be
in the prompt window.

### 3.2 Active recall + verify
After retrieving `plan = Pro (last_confirmed 2026-01-02)`, Aria checks the
billing API to confirm the plan is still Pro before stating it. Retrieval
without this check would violate §3.2.

### 3.3 Anchors
Every query carries who/what/when/why. A vague query ("what did they
want?") with no `who` is widened or Aria asks a clarifying question before
acting on a low-confidence match.

---

## 4. Degradation

### 4.1 TTL
| Stratum | TTL |
|---|---|
| Working | 30 min idle |
| Episodic | 90 days, reset on retrieval |
| Semantic | none (prune on conflict) |
| Procedural | none (prune on failure rate) |

### 4.2 Pruning triggers
- **Conflict:** `plan = Free` from a new ticket supersedes `plan = Pro`;
  the old fact is archived with a pointer to the new one.
- **Obsolescence:** a ticket episode untouched for 90 days expires.
- **Redundancy:** "user 4471 wanted password reset on Jun 20" is dropped
  once the general fact "user 4471 uses SSO" is consolidated.

Every prune is logged: `pruned ticket#8842 reason=obsolescence`.

### 4.3 Graceful degradation
If `plan = Pro` was last confirmed 80 days ago, Aria says "you were on Pro
as of ~3 months ago — let me confirm" rather than asserting it flatly. It
does not silently drop the fact just because it is old.

---

## 5. Health

### 5.1 Coherence check
A nightly job scans semantic memory for the same user holding
contradictory facts (`plan = Pro` AND `plan = Free`) and flags them.

### 5.2 Redundancy index
Critical memory `user 4471 SSO is mandatory` is reachable by **two**
paths: entity index (`user_id`) and explicit tag (`security:sso`). A
critical fact with only one path fails the nightly health check.

### 5.3 Consistency verification
The `password-reset` playbook is sampled weekly. If it failed in 2 of its
last 3 uses, it is flagged for revision before Aria keeps offering it.

---

## Conformance self-check

1. **Which stratum?** Every Aria memory has an explicit stratum tag. ✓
2. **Encoding depth?** Recorded as `shallow`/`structural`/`deep`. ✓
3. **TTL / prune trigger?** Declared per stratum above. ✓
4. **Retrieval paths for critical memory?** ≥2, enforced nightly. ✓
5. **Last verified against evidence?** `last_confirmed` on every semantic
   fact; weekly sampling on playbooks. ✓

Aria conforms to COGNITION.md v0.1.
