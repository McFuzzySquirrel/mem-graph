# Administrator Guide

## Responsibilities and Architecture

There is no deployable service or administrator-operated runtime yet. The
current operator responsibility is to preserve the roadmap and documentation
as the source of project intent. The target architecture is a local,
rebuildable SQLite-derived index over repository documents, with separately
recoverable authored records and an explicitly reviewed publication layer.

## Prerequisites

For the current repository:

- Git access to the repository.
- A Markdown-capable editor.
- No server, database, package manager, runtime, or cloud account.

## Installation

There is no installation procedure because no executable artifact exists.
Clone the repository and read [README.md](../README.md) and [PLAN.md](../PLAN.md).
Future installation instructions must be added only after the package and
distribution model are selected in phase 2.

## Configuration

No configuration names, environment variables, ports, paths, or command-line
flags are implemented. The roadmap calls for explicit configuration of
project identity, authorized roots, included and excluded document paths,
supported formats, scopes, and database placement. These must not be inferred
from an agent-supplied request.

## Identity, Secrets, and TLS

No identity system, secrets, TLS endpoint, or network service exists. Do not
store credentials or private data in Git. A future hosted or connected
implementation must define least-privilege roles for read, ingest, review,
approval, and publication, and document secret storage and certificate
rotation.

## Storage and Backups

There is no database to back up. The planned local graph is derived and
rebuildable; approved lessons, session outcomes, and lifecycle history must
remain separately recoverable so rebuilding an index cannot silently erase
authored knowledge. Backup and recovery commands are future deliverables.

## Health Checks and Monitoring

No process, endpoint, log format, or health check exists. Future operations
should monitor indexing failures, storage growth, retrieval latency, stale
records, scope violations, and publication/revocation events without logging
sensitive document content.

## Upgrades and Rollback

No released schema or versioned artifact exists, and no rollback procedure can
be run. Phase 2 requires migrations and recovery tests; phase 9 requires
installation, upgrade, rollback, backup, recovery, and removal validation
before a release can be claimed.

## Troubleshooting

The most likely current issue is assuming planned behavior is implemented.
Confirm the repository status and consult the relevant roadmap phase. There is
no service log, database, package command, or runtime endpoint to inspect.

## Security and Privacy Checklist

- Keep private repositories and credentials out of commits, fixtures, and
  exported bundles.
- Treat retrieved document content as untrusted evidence, not executable
  instructions.
- Enforce authorization before search and graph traversal, including metadata
  and result counts.
- Require explicit approval and redacted preview before publication.
- Preserve provenance, applicability, correction, revocation, and deletion
  history.
- Do not claim scanning alone guarantees confidentiality.

## Recovery and Support

There is no runtime recovery process yet. For repository recovery, use normal
Git history and preserve `PLAN.md` changes with their rationale. Report public
questions and roadmap issues through GitHub; keep sensitive security details
private.
