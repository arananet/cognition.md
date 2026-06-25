# Changelog

All notable changes to `cognition.md` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

<!--
Guidelines:
- Add a new entry under `## [Unreleased]` as you work — no batching up for release day.
- Group entries under: Added, Changed, Deprecated, Removed, Fixed, Security.
- Reference the spec slug and PR number:  "Added dark mode (spec: dark-mode, #42)".
- On release, rename `[Unreleased]` to the new version with the release date,
  and open a fresh `[Unreleased]` section at the top.
- The release-drafter workflow auto-populates draft release notes from PRs —
  keep PR titles tidy so they flow straight into here.
-->

## [Unreleased]

### Added
- `COGNITION.md` — the v0.1 cognitive contract, with all five normative
  sections (Taxonomy, Consolidation, Retrieval, Degradation, Health), a
  conformance checklist, and RFC 2119 keyword usage (spec: cognition-spec-v0-1)
- `docs/RATIONALE.md` — maps each spec section to its peer-reviewed source
  (Tulving, Craik & Lockhart, Roediger & Karpicke, Ebbinghaus, Stern) and the
  failure mode each rule prevents
- `examples/chatbot-cognition.md` — a worked instance of the contract filled in
  for a support chatbot, with a conformance self-check
- `docs/adr/0002-episodic-precedes-semantic.md` — records why consolidation must
  pass through episodic memory (four strata, not three)

### Changed
- `README.md` rewritten to describe the COGNITION.md project, with a
  Documentation table linking the contract, rationale, and worked example

### Removed
- Template-authoring workflows that have no role in this project
  (`template-smoke-test.yml`, `repo-init.yml`, `spec-bootstrap.yml`)

---

## [0.1.0] — YYYY-MM-DD

### Added
- Initial release.

[Unreleased]: https://github.com/arananet/cognition.md/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/arananet/cognition.md/releases/tag/v0.1.0
