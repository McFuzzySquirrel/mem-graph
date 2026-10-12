# Engineering memory plugin: implementation plan

## Purpose

Build a portable engineering memory capability that helps agents and people reuse
the reasoning behind software: decisions, failed approaches, resolutions, and
verified lessons. The primary inputs are repository documentation, not code.

Repository documents remain the source of truth. A local SQLite knowledge graph
is a derived, rebuildable index. Cross-project memory is an explicitly curated
publication layer, not automatic replication of everything an agent encounters.

This is a proposed roadmap, not a description of implemented functionality.
The repository currently has no application, build, or test infrastructure.

## How to track this plan

- Work through the phases in order unless a task explicitly has no dependency.
- Turn each task ID into an issue when implementation begins; add its issue or PR
  link beside the checkbox.
- Check a task only when its deliverable and relevant acceptance criteria exist.
- Record architecture choices in ADRs as they are made, including alternatives.
- Record evidence for each phase gate before starting its dependent phase.
- If scope changes, update this plan and document the reason.
- Do not mark implementation tasks complete merely because a design exists.

| Phase | Outcome | Depends on | Status |
| --- | --- | --- | --- |
| 0 | Validated scope and representative inputs | None | Not started |
| 1 | Memory contracts and lifecycle | 0 | Not started |
| 2 | Local SQLite graph foundation | 1 | Not started |
| 3 | Rebuildable document ingestion | 2 | Not started |
| 4 | Useful, evidence-backed retrieval | 3 | Not started |
| 5 | Verified session learning | 4 | Not started |
| 6 | Tool-independent plugin integration | 4; 5 for outcome capture | Not started |
| 7 | Local pilot and usefulness evaluation | 5, 6 | Not started |
| 8 | Controlled cross-project publication | 7 | Not started |
| 9 | Shared-memory evaluation and release | 8 | Not started |

**Initial product boundary:** complete phases 0–7 before investing in a shared
platform. Phase 7 is a go/no-go decision, not an automatic approval for phase 8.

## Product principles and scope

### Required principles

- Store context and applicability, not unsupported universal recommendations.
- Preserve citations, source revisions, and verification evidence.
- Separate temporary observations from durable lessons.
- Return a small, relevant set of lessons rather than injecting all memory.
- Keep conflicting lessons distinguishable when circumstances differ.
- Treat retrieved content as untrusted evidence, never executable instructions.
- Enforce access scope before searching or traversing relationships.
- Keep local use possible without an account or a remote service.
- Support correction, expiry, revocation, and deletion from the beginning.

### Initial non-goals

- Indexing all source code or every raw session transcript.
- Replacing ADRs, document maps, or project documentation.
- Training a model or building an autonomous self-improvement system.
- Automatic cross-project sharing or unrestricted global memory.
- Selecting a dedicated graph database or embedding provider before measuring
  the limitations of SQLite and full-text retrieval.
- Building a general-purpose agent orchestration platform.

## Phase 0 — Validate the problem and document inputs

**Goal:** establish concrete use cases and inspect the formats that the plugin
will actually consume.

### Tasks

- [ ] **P0.1** Review the Engineering Journey System and the `doc-map` and
  `create-project-documentation` skills. Record the exact revisions reviewed,
  document categories, metadata conventions, and licensing constraints.
- [ ] **P0.2** Select one or two documentation-rich pilot repositories with
  permission to index their content.
- [ ] **P0.3** Inventory representative ADRs, document maps, project documents,
  and existing records of failures and resolutions.
- [ ] **P0.4** Define priority user journeys: retrieve a related decision,
  avoid a previously failed approach, resolve a recurring error, and discover
  a lesson from a similar project.
- [ ] **P0.5** Assemble a sanitized evaluation set of real questions and expected
  supporting documents, including questions with no relevant answer.
- [ ] **P0.6** Define access boundaries, excluded content, retention requirements,
  and who can approve durable lessons and shared publications.
- [ ] **P0.7** Establish a baseline using current documentation search and
  document-map navigation. Agree on success thresholds before the pilot.

### Deliverables

- Input-format inventory and representative sanitized fixtures.
- Prioritized use cases, evaluation questions, and baseline results.
- Scope and privacy decisions recorded in an ADR.

### Acceptance gate

- [ ] **G0** At least two repeatable engineering-memory use cases have concrete
  source evidence and an agreed evaluation method.
- [ ] **G0** Actual document formats have been inspected; support is not inferred
  solely from skill names or assumptions about templates.

### Reference inputs to inspect

- https://github.com/McFuzzySquirrel/Engineering-Journey-System
- https://github.com/McFuzzySquirrel/experimental-skills/tree/main/collection/skills/doc-map
- https://github.com/McFuzzySquirrel/experimental-skills/tree/main/collection/skills/create-project-documentation

These are discovery inputs, not verified integration contracts.

## Phase 1 — Define the memory model and trust lifecycle

**Goal:** define what a memory means before choosing implementation details.

### Tasks

- [ ] **P1.1** Define entity types: project, source document, decision, constraint,
  attempt, failure, resolution, lesson, and supporting evidence.
- [ ] **P1.2** Define directional relationship types and valid endpoints:
  motivated-by, constrained-by, failed-because-of, addresses, supported-by,
  applies-under, contradicts, and supersedes.
- [ ] **P1.3** Define a reusable lesson contract: problem, circumstances,
  attempted approach, outcome, resolution, applicability, counterexamples,
  evidence, and verification status.
- [ ] **P1.4** Define provenance fields: project identity, document identity,
  repository revision, section or location, content hash, extraction method,
  capture date, and schema/extractor version.
- [ ] **P1.5** Separate short-term, project long-term, and shared-memory scopes.
  Define expiry for short-term records and explicit promotion rules.
- [ ] **P1.6** Define lifecycle states and permitted transitions: observed,
  proposed, verified, rejected, superseded, and revoked. Keep publication status
  separate from verification status.
- [ ] **P1.7** Define stable identities, versioning, duplicate handling, and how
  contradictory records remain available without becoming duplicate facts.
- [ ] **P1.8** Specify correction and deletion semantics, including how removed
  evidence changes the validity of dependent lessons.
- [ ] **P1.9** Write a threat model covering prompt injection, poisoned lessons,
  sensitive information, unauthorized retrieval, and stale recommendations.

### Deliverables

- Versioned entity, relationship, and lesson contracts.
- Memory lifecycle and trust policy.
- Identity/provenance ADR and threat model.

### Acceptance gate

- [ ] **G1** Representative phase-0 documents can be mapped to the contracts
  without silently inventing missing evidence.
- [ ] **G1** An unverified observation cannot become a verified or shared lesson
  without an explicit, auditable transition.
- [ ] **G1** Conflicting context-dependent lessons and revoked evidence have
  defined behavior.

## Phase 2 — Build the local SQLite foundation

**Goal:** provide a small, reliable storage layer suitable for a plugin.

### Tasks

- [ ] **P2.1** Select the implementation language, package manager, test framework,
  and distribution model in an ADR based on portability and agent integration.
- [ ] **P2.2** Scaffold the selected project using its standard ecosystem tooling;
  establish build, lint, and test commands.
- [ ] **P2.3** Implement schema migrations for entities, relationships, sources,
  revisions, scopes, lifecycle history, and publication metadata.
- [ ] **P2.4** Add SQLite full-text search and indexes for scoped lookups.
- [ ] **P2.5** Implement storage operations with parameterized queries,
  transactions, relationship validation, and foreign-key enforcement.
- [ ] **P2.6** Define database placement outside tracked source files by default,
  connection lifecycle, concurrent access behavior, and safe failure handling.
- [ ] **P2.7** Provide initialization, migration, health inspection, backup, and
  recovery operations.
- [ ] **P2.8** Add tests for migrations, rollback, invalid relationships, scope
  isolation, stable identities, and supported concurrency behavior.

### Deliverables

- Working local storage package and migration strategy.
- Documented installation and database lifecycle.
- Automated storage and migration tests.

### Acceptance gate

- [ ] **G2** A clean installation can initialize and reopen a database.
- [ ] **G2** Interrupted operations do not leave a partially updated graph.
- [ ] **G2** Migration and recovery preserve valid data and report failures.
- [ ] **G2** Local operation requires no remote service.

## Phase 3 — Ingest documents into a rebuildable graph

**Goal:** reliably index existing repository documents without replacing them.

### Tasks

- [ ] **P3.1** Define explicit configuration for included document paths,
  excluded paths, project identity, and supported formats.
- [ ] **P3.2** Implement adapters for the phase-0 ADR, document-map, and project
  documentation formats; report unsupported formats rather than guessing.
- [ ] **P3.3** Extract headings, explicit metadata, links, and typed relationships.
  Keep extracted statements distinguishable from inferred statements.
- [ ] **P3.4** Resolve links within authorized repository boundaries and report
  missing targets. Prevent traversal through unsafe paths or symlinks.
- [ ] **P3.5** Implement content-hash-based incremental indexing and repeatable
  ingestion that does not create duplicates.
- [ ] **P3.6** Handle renamed, modified, and deleted documents, removing stale
  derived relationships while preserving appropriate lifecycle history.
- [ ] **P3.7** Provide dry-run ingestion with source counts, exclusions, parse
  errors, and proposed graph changes.
- [ ] **P3.8** Apply sensitive-content exclusions and scanning before durable
  storage; document scanning limitations and manual review requirements.
- [ ] **P3.9** Implement clean rebuild of the document-derived index. Keep
  authored lesson approvals and session records in a separately recoverable
  store or export so a rebuild cannot silently erase them.
- [ ] **P3.10** Test malformed documents, ambiguous metadata, broken links,
  duplicates, revisions, deletion, and unsafe input paths.

### Deliverables

- Document adapters and indexing interface.
- Incremental ingestion and rebuild support.
- Sanitized ingestion fixtures and regression tests.

### Acceptance gate

- [ ] **G3** Re-indexing unchanged documents produces no duplicate entities.
- [ ] **G3** Citations resolve to the indexed source revision and location.
- [ ] **G3** Changed or deleted documents cannot leave apparently current lessons
  supported solely by stale evidence.
- [ ] **G3** A rebuild reproduces the document-derived graph while preserving
  separately recorded approvals and session knowledge.

## Phase 4 — Retrieve lessons with evidence and applicability

**Goal:** prove the graph improves access to reasoning, not just storage.

### Tasks

- [ ] **P4.1** Define queries for related decisions, failure resolutions,
  applicable lessons, and supporting source context.
- [ ] **P4.2** Implement scoped full-text search plus structured filters for
  project, document type, lifecycle state, and relevant constraints.
- [ ] **P4.3** Add bounded graph traversal to connect attempts, failures,
  resolutions, decisions, and evidence.
- [ ] **P4.4** Rank results using relevance, applicability, verification,
  freshness, and evidence quality; define deterministic tie-breaking.
- [ ] **P4.5** Return compact results with source citations, why each result
  matched, applicability limits, and contradictory evidence.
- [ ] **P4.6** Enforce scope filtering before retrieval and traversal, including
  counts and metadata that could otherwise leak inaccessible content.
- [ ] **P4.7** Support an explicit no-useful-result response rather than forcing
  a recommendation.
- [ ] **P4.8** Evaluate against the phase-0 questions and documentation-search
  baseline; identify gaps before considering semantic search.
- [ ] **P4.9** Add tests for irrelevant matches, stale evidence, conflicting
  lessons, scope leaks, and bounded result sizes.

### Deliverables

- Retrieval interface and documented ranking behavior.
- Citation-bearing responses and evaluation results.

### Acceptance gate

- [ ] **G4** Agreed relevance and citation-accuracy thresholds are met on the
  evaluation set.
- [ ] **G4** Unverified or revoked lessons are not presented as established facts.
- [ ] **G4** Queries with no supporting evidence can return no result.
- [ ] **G4** Access boundaries hold across both search and graph traversal.

## Phase 5 — Capture session outcomes and promote lessons

**Goal:** learn from agent sessions without preserving every transient statement.

### Tasks

- [ ] **P5.1** Define a concise session outcome record covering task context,
  attempted approaches, failures, resolutions, and verification evidence.
- [ ] **P5.2** Provide explicit outcome capture, with previews and redaction;
  do not require raw transcript ingestion.
- [ ] **P5.3** Store temporary observations with expiry and clear separation
  from project long-term lessons.
- [ ] **P5.4** Create a review workflow to accept, amend, reject, or defer
  proposed lessons and attach evidence.
- [ ] **P5.5** Preserve failed attempts, counterexamples, and unresolved outcomes
  without mislabeling an untested resolution as successful.
- [ ] **P5.6** Implement duplicate detection that proposes merges while retaining
  distinct contexts and provenance.
- [ ] **P5.7** Add correction, supersession, revocation, and expiry operations.
- [ ] **P5.8** Define durable recovery/export for authored records and approval
  history; verify it alongside document-index rebuild.
- [ ] **P5.9** Test promotion authorization, false success claims, sensitive
  content, expiry, duplicate handling, and corrections.

### Deliverables

- Session outcome capture and lesson review workflow.
- Durable lesson records and recoverable lifecycle history.

### Acceptance gate

- [ ] **G5** A session can record an unresolved failure without asserting a fix.
- [ ] **G5** A verified lesson identifies both its evidence and approving action.
- [ ] **G5** Temporary memory expires without deleting approved durable lessons.
- [ ] **G5** Incorrect lessons can be corrected or revoked with visible history.

## Phase 6 — Expose the plugin to agent tools

**Goal:** make the capability reusable without coupling it to one agent product.

### Tasks

- [ ] **P6.1** Define a versioned interface for ingestion, retrieval, outcome
  capture, review, and memory inspection.
- [ ] **P6.2** Provide a CLI for direct use, troubleshooting, and automation.
- [ ] **P6.3** Implement an MCP adapter over the same core interface, keeping
  transport-specific concerns out of storage and retrieval.
- [ ] **P6.4** Separate read-only access from ingestion, lesson approval, and
  publication privileges.
- [ ] **P6.5** Make authorized roots and project scopes explicit configuration;
  do not derive authority from an agent-supplied request.
- [ ] **P6.6** Add request limits, structured errors, bounded response sizes,
  and logs that avoid sensitive document content.
- [ ] **P6.7** Document setup and integration guidance: consult memory at task
  start, inspect citations, and propose outcomes at task completion.
- [ ] **P6.8** Add compatibility and integration tests for supported clients,
  cancellation, unavailable databases, and invalid requests.

### Deliverables

- CLI, MCP adapter, and versioned tool contracts.
- Agent integration documentation and least-privilege configuration.

### Acceptance gate

- [ ] **G6** At least one supported agent client can retrieve cited lessons and
  submit a proposed outcome without direct database access.
- [ ] **G6** Read-only clients cannot mutate, approve, or publish memory.
- [ ] **G6** Retrieved documents cannot change tool permissions or execute
  embedded instructions.

## Phase 7 — Run the local pilot and decide whether to expand

**Goal:** determine whether engineering memory is worth its maintenance cost.

### Tasks

- [ ] **P7.1** Index the selected pilot repositories and review ingestion quality.
- [ ] **P7.2** Run representative tasks with existing documentation search and
  with memory retrieval; record citations and decision outcomes.
- [ ] **P7.3** Review relevance, applicability, citation correctness, misleading
  recommendations, and repeated failures avoided.
- [ ] **P7.4** Measure operational burden: review effort, indexing failures,
  storage growth, retrieval latency, and stale records.
- [ ] **P7.5** Collect agent and human feedback on result clarity and workflow fit.
- [ ] **P7.6** Fix observed correctness, usability, and safety issues; add
  regression cases for each material failure.
- [ ] **P7.7** Compare results with the thresholds agreed in phase 0 and record
  a go/no-go ADR for cross-project sharing.

### Deliverables

- Pilot report with baseline comparisons and failure analysis.
- Prioritized improvement backlog and expansion decision.

### Acceptance gate

- [ ] **G7** Demonstrated usefulness is supported by task evidence, not the
  number of stored records.
- [ ] **G7** Privacy and access-control failures discovered in the pilot are
  resolved before expansion.
- [ ] **G7** If usefulness is insufficient, pause sharing work and improve or
  narrow local retrieval rather than scaling poor-quality memory.

## Phase 8 — Publish curated cross-project memory

**Goal:** share transferable lessons while retaining provenance and control.

### Tasks

- [ ] **P8.1** Choose the initial publication mechanism in an ADR: portable
  reviewed bundles or an authenticated collection service. Do not assume a
  central service is necessary.
- [ ] **P8.2** Define a versioned publication format with stable lesson IDs,
  source attribution, verification status, applicability, and revision history.
- [ ] **P8.3** Require explicit approval and a redacted preview before publication;
  inspect text and metadata for sensitive project information.
- [ ] **P8.4** Enforce collection membership and read/write authorization in
  any hosted deployment.
- [ ] **P8.5** Define update, conflict, retry, duplicate, and interrupted-transfer
  behavior; retain publisher identity and lineage.
- [ ] **P8.6** Implement revocation and deletion propagation using appropriate
  tombstones or equivalent records, including offline consumer behavior.
- [ ] **P8.7** Define freshness and expiry rules for imported lessons so a consumer
  cannot indefinitely trust a disconnected, revoked publication.
- [ ] **P8.8** Preserve useful non-sensitive evidence when original sources are
  private; clearly label inaccessible or independently unverifiable citations.
- [ ] **P8.9** Add cross-project filters and ranking so broad lessons do not
  override local decisions or constraints.
- [ ] **P8.10** Test publication authorization, malicious imports, metadata leaks,
  retries, conflicts, revocation, deletion, and offline freshness.

### Deliverables

- Reviewed publication/export and import workflow.
- Collection authorization where applicable.
- Update, revocation, deletion, and freshness contracts.

### Acceptance gate

- [ ] **G8** Only approved lessons cross the project boundary.
- [ ] **G8** A published lesson can be updated or revoked without becoming a
  conflicting duplicate.
- [ ] **G8** Consumers can identify publisher, evidence availability, and
  freshness before trusting an imported lesson.
- [ ] **G8** Deletion and revocation behavior is tested, including disconnected
  consumers and the limits of controlling external copies.

## Phase 9 — Evaluate shared memory and release

**Goal:** deliver a supportable plugin and prove transfer between projects.

### Tasks

- [ ] **P9.1** Run a cross-project pilot using explicitly approved lessons and
  evaluate whether they help under genuinely similar circumstances.
- [ ] **P9.2** Measure harmful transfer: irrelevant patterns, conflicting
  constraints, inaccessible evidence, and stale recommendations.
- [ ] **P9.3** Complete security review, dependency checks, and secret scanning;
  resolve findings affecting the release.
- [ ] **P9.4** Validate installation, supported platforms, upgrade, rollback,
  backup, recovery, and schema compatibility.
- [ ] **P9.5** Publish user guidance for capture, review, retrieval, sharing,
  corrections, and deletion.
- [ ] **P9.6** Establish release/versioning practices, support boundaries, and
  contribution guidelines.
- [ ] **P9.7** Review measured bottlenecks before considering semantic retrieval,
  additional adapters, or a different graph backend.

### Deliverables

- Shared-memory evaluation report.
- Installable release, operational documentation, and known limitations.
- Evidence-based roadmap for subsequent improvements.

### Acceptance gate

- [ ] **G9** Shared lessons improve selected tasks without bypassing local
  constraints or authorization.
- [ ] **G9** Release-critical correctness and security findings are resolved.
- [ ] **G9** An operator can install, upgrade, recover, and remove the system
  using the published instructions.

## Cross-cutting validation

Apply these requirements throughout implementation, not only at release:

- [ ] Each behavior-changing task includes relevant automated tests.
- [ ] Database changes include migration and rollback/recovery verification.
- [ ] Ingestion changes include provenance, deletion, and rebuild checks.
- [ ] Retrieval changes include irrelevant-query and unauthorized-scope cases.
- [ ] Capture and sharing changes include sensitive-content and prompt-injection
  cases; scanning alone is not treated as a confidentiality guarantee.
- [ ] New dependencies are justified and checked for known vulnerabilities.
- [ ] Logs, fixtures, exported bundles, and commits are checked for secrets.
- [ ] Documentation states supported formats, limitations, and failure behavior.

## Principal risks and responses

| Risk | Response | Primary phase |
| --- | --- | --- |
| Documents do not contain enough reasoning | Validate real examples before building; add concise outcome capture | 0, 5 |
| Agent guesses become durable facts | Require evidence, review, and explicit lifecycle transitions | 1, 5 |
| A lesson is applied in the wrong context | Record constraints, counterexamples, and applicability | 1, 4 |
| Index drifts from source documents | Track revisions, hashes, removals, and reproducible rebuilds | 3 |
| Rebuild destroys approved session knowledge | Separate derived data from recoverable authored records | 3, 5 |
| Retrieved content manipulates an agent | Treat content as untrusted evidence; separate authority from text | 1, 6 |
| Shared memory exposes private information | Explicit publication, redaction review, and scoped access | 0, 8 |
| Revoked lessons persist in disconnected consumers | Define freshness limits and revocation propagation | 8 |
| Knowledge graph adds complexity without benefit | Compare retrieval with the documentation-search baseline | 4, 7 |
| Shared patterns conflict with project decisions | Preserve provenance and prioritize applicable local constraints | 8, 9 |

## Decisions deliberately deferred

- Implementation language and packaging: decide in P2.1.
- Exact adapters and metadata conventions: decide after phase-0 inspection.
- Semantic search and embeddings: consider only after measured phase-4 gaps.
- Hosted collection and deployment stack: decide in P8.1 after the local pilot.
- Dedicated graph backend: consider only if measured SQLite limitations justify
  the additional operational complexity.

## Definition of completion

**Local MVP:** phases 0–7 pass their gates, and a supported agent can retrieve
verified, cited lessons from repository documents and reviewed session outcomes.
The index is rebuildable, authored records are recoverable, and local usefulness
has been measured.

**Shared-memory release:** phases 8–9 also pass their gates, and approved lessons
can move between projects with authorization, provenance, correction, revocation,
and freshness controls.
