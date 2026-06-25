# COGNITION.md

![Markdown](https://img.shields.io/badge/Markdown-gray)
![OpenSpec](https://img.shields.io/badge/OpenSpec-enforced-blueviolet)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

> Cognitive contract spec for agentic memory.

Your AI agent has dementia. The industry treats this as an engineering problem. It is a cognitive architecture problem. The science to solve it has been published for 140 years. Nobody reads it.

COGNITION.md is a declarative, framework-agnostic specification that defines how an agent should manage its memory. Not a storage format. Not an SDK. Not a runtime. A contract.

---

## What this is

A `.md` spec file that any agentic system can adopt. It declares the cognitive policies an agent must follow for memory management: what to remember, how to encode it, when to consolidate, how to retrieve, when to forget, and how to verify integrity.

It follows the same pattern as `CLAUDE.md`, `SKILL.md`, and `auth.md`. Machine-readable. Human-auditable. Implementation-agnostic.

## What this is not

This is not a memory store like OpenClaw's `MEMORY.md`. That file holds facts. This file defines how facts should be governed.

This is not a memory SDK like Mem0. Mem0 is infrastructure. COGNITION.md is the contract that infrastructure should implement.

This is not a runtime like Letta. Letta is a platform you adopt. COGNITION.md is a spec you can apply inside any platform.

## Why it exists

Every agentic memory system today solves the problem from the engineering side. Vector stores, embeddings, retrieval pipelines. These are necessary. They are not sufficient.

The failure modes in agentic memory are structurally identical to the failure modes in human memory disorders. Context loss, stale recall, inability to distinguish current knowledge from outdated knowledge, uncontrolled accumulation of noise. Cognitive science has studied these failures for over a century. The solutions are documented, peer-reviewed, and experimentally validated.

COGNITION.md translates five foundational principles from cognitive science into engineering constraints for agentic systems.

## Scientific foundations

| Principle | Source | Agentic translation |
|---|---|---|
| Memory taxonomy | Tulving (1972) | Separate stores for episodic, semantic, procedural, and working memory. Each with its own encoding, retention, and retrieval policy. |
| Depth of encoding | Craik & Lockhart (1972) | Memories encoded with intent, causality, and relational context survive longer than raw logs. Shallow encoding degrades fast. |
| Active retrieval | Roediger & Karpicke (2006) | Retrieve-then-verify before generation. The act of recall strengthens the trace. Passive injection does not. |
| Controlled forgetting | Ebbinghaus (1885) | TTLs, conflict-driven pruning, relevance decay. A system that never forgets is as broken as one that forgets everything. |
| Cognitive reserve | Stern (2002) | Multi-anchor indexing. More retrieval paths per memory means more resilience when any single path degrades. |

## Spec structure

A `COGNITION.md` file declares five sections:

```
# COGNITION.md — Cognitive Contract v0.1

## Taxonomy
Declare memory strata: working, episodic, semantic, procedural.
Each stratum defines encoding depth, retention policy, retrieval protocol.

## Consolidation
When and how memories promote between strata.
Depth-of-processing requirements per level (shallow, structural, deep).
Spaced repetition schedule for re-evaluation.

## Retrieval
Retrieve-before-generate for stateful decisions.
Active recall before passive injection.
Minimum contextual anchors: who, what, when, why.

## Degradation
TTL per stratum.
Pruning triggers: conflict, obsolescence, redundancy.
Graceful degradation when a memory is uncertain.

## Health
Periodic coherence check across strata.
Redundancy index per critical memory.
Consistency verification against recent evidence.
```

## Landscape

| Project | What it is | What it is not |
|---|---|---|
| OpenClaw MEMORY.md | Persistent fact store in Markdown | Cognitive policy spec |
| Mem0 | Universal memory SDK with vector + graph retrieval | Architecture contract |
| Letta (MemGPT) | Agent runtime with OS-inspired memory tiers | Framework-agnostic spec |
| Zep | Temporal knowledge graph for conversational AI | Cognitive lifecycle model |
| AgentMemory | Session capture for coding agents | Cross-domain standard |
| **COGNITION.md** | **Cognitive contract any implementation can adopt** | **Implementation** |

COGNITION.md does not compete with these projects. It is the layer above them. Any of them could adopt a `COGNITION.md` file to declare their cognitive policies.

## Usage

The v0.1 contract is published at [`COGNITION.md`](COGNITION.md) in this
repo. Copy it (or fork it) to the root of your agent project — it declares
the cognitive contract your agent follows. Your memory infrastructure
(Mem0, Letta, custom RAG, plain files) implements the contract.

```
my-agent/
├── COGNITION.md    # Cognitive contract
├── CLAUDE.md       # Agent operating contract
├── SKILL.md        # Capability definitions
└── src/
```

## Quick start

```bash
# 1. Clone
git clone https://github.com/arananet/cognition.md.git
cd cognition.md
bash setup.sh

# 2. Validate spec coverage
scripts/openspec check --strict
```

## Roadmap

- [ ] v0.1 spec draft with full section definitions
- [ ] Reference implementation examples (Mem0, Letta, plain Markdown)
- [ ] Validation schema for COGNITION.md files
- [ ] Benchmark: memory coherence with and without cognitive contract
- [ ] Integration guides for major agent frameworks

## Contributing

This is an open spec. If you work on agentic memory, cognitive science, or enterprise AI architecture, contributions are welcome. Open an issue or submit a PR.

This project uses **OpenSpec** for spec-driven development — every feature
or bugfix starts with a spec file under `.openspec/specs/`. Each spec
includes a `roles` block to assign responsibility (`implementer`,
`reviewer`, `qa`, `product_owner`). See
[`docs/OPENSPEC.md`](docs/OPENSPEC.md) for the full workflow, or
[`CONTRIBUTING.md`](CONTRIBUTING.md) for the contributor checklist.

---

## Documentation

| Topic | Where |
|---|---|
| Spec-driven workflow | [`docs/OPENSPEC.md`](docs/OPENSPEC.md) |
| Branch protection setup | [`docs/BRANCH_PROTECTION.md`](docs/BRANCH_PROTECTION.md) |
| Architecture decisions | [`docs/adr/`](docs/adr/) |
| Security policy | [`SECURITY.md`](SECURITY.md) |
| Support channels | [`SUPPORT.md`](SUPPORT.md) |
| Release history | [`CHANGELOG.md`](CHANGELOG.md) |

## References

- Ebbinghaus, H. (1885). *Uber das Gedachtnis*. Leipzig: Duncker & Humblot.
- Tulving, E. (1972). Episodic and semantic memory. In *Organization of Memory* (pp. 381-403). Academic Press.
- Craik, F.I.M., & Lockhart, R.S. (1972). Levels of processing. *Journal of Verbal Learning and Verbal Behavior*, 11(6), 671-684.
- Roediger, H.L., & Karpicke, J.D. (2006). Test-enhanced learning. *Psychological Science*, 17(3), 249-255.
- Stern, Y. (2002). What is cognitive reserve? *Journal of the International Neuropsychological Society*, 8(3), 448-460.
- Packer, C., et al. (2023). MemGPT: Towards LLMs as Operating Systems. *arXiv:2310.08560*.
- Chhikara, P., et al. (2025). Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory. *arXiv:2504.19413*.

## License

[MIT](LICENSE)

---

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2H51MPWG)

---

*Created by [Eduardo Luis Arana Giaccio](https://www.linkedin.com/in/eduardoluisarana). Founding article: TODO — add the published article link here.*
