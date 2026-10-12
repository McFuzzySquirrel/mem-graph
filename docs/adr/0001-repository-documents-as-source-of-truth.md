# ADR-0001: Keep Repository Documents as the Source of Truth

- **Status:** Proposed
- **Deciders:** Project maintainers
- **Date:** 2026-10-12

**Technical Story:** [PLAN.md](../../PLAN.md), product principles and phases 0–3

## Context and Problem Statement

The proposed memory system needs a searchable representation of engineering
reasoning without replacing the documents that people already review and
maintain. How should the system relate its local knowledge graph to repository
documents and authored session knowledge?

## Decision Drivers

- Preserve human-readable source documents as authoritative.
- Make derived indexing repeatable and rebuildable.
- Retain provenance, source revisions, and citations.
- Avoid silently deleting approved session knowledge during rebuilds.
- Keep local use possible without a remote service.

## Considered Options

- Treat repository documents as the source of truth and build a derived local
  graph.
- Treat the graph as the primary store and generate or synchronize documents.
- Store all documents and session transcripts as an undifferentiated graph.

## Decision Outcome

Chosen option: “Repository documents as the source of truth with a
rebuildable derived graph,” because it preserves reviewable project authority
while allowing local retrieval and controlled future publication. This record
remains **Proposed** until phase-0 evidence and phase-1 contracts validate the
boundary.

### Positive Consequences

- Re-indexing can reproduce document-derived data from source revisions.
- Citations can point back to a document and location.
- Documents remain usable without the memory system.
- Authored approvals and session knowledge can be recovered independently.

### Negative Consequences

- The system must track revisions, hashes, deletion, and stale evidence.
- Derived data can lag behind source documents until ingestion runs.
- The implementation needs separate lifecycle handling for derived and authored
  records.

## Pros and Cons of the Options

### Repository documents as source of truth

- Good, because review and correction remain in familiar project documents.
- Good, because a rebuild can repair derived-index corruption.
- Bad, because ingestion and provenance are additional implementation work.

### Graph as primary store

- Good, because retrieval-oriented data is centralized.
- Bad, because document workflows could drift from graph state.
- Bad, because rebuilding or migrating the graph risks losing authoritative
  context.

### Undifferentiated graph for documents and transcripts

- Good, because ingestion appears simpler initially.
- Bad, because transient observations could be mistaken for durable lessons.
- Bad, because retention, privacy, approval, and deletion boundaries become
  ambiguous.

## Links

- [Implementation roadmap](../../PLAN.md)
- [ADR index](README.md)
- Related future decision: implementation ecosystem and packaging in phase 2.1
