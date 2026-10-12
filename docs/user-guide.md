# User Guide

## Who This Is For

This guide is for engineers and agent authors evaluating the proposed
engineering-memory capability. There is no end-user application or client
workflow in the current repository.

## Before You Begin

Read [README.md](../README.md) and [PLAN.md](../PLAN.md). The project is
documentation-only today, so no account, installation, database, or remote
service is required.

## Sign In and First Use

Not applicable. Authentication and first-use screens do not exist. A future
implementation must document identity, project scope, and authorization before
it exposes memory to an agent or user.

## Main Workflows

The planned workflows are:

- retrieve a related decision with supporting citations;
- avoid a previously failed approach;
- resolve a recurring error using an applicable, verified lesson;
- capture a session outcome without ingesting an entire transcript;
- review, amend, reject, or defer a proposed lesson; and
- inspect provenance, applicability, expiry, correction, or revocation.

These workflows are not implemented and must not be presented as available
product behavior.

## Offline and Reconnect Behavior

No client, local database, or network protocol exists yet. The design direction
favors local operation without an account or remote service. Any future
connected publication workflow must define retries, conflicts, freshness,
revocation, deletion, and disconnected-consumer behavior.

## Statuses and Notifications

The roadmap proposes separate verification and publication states, including
observed, proposed, verified, rejected, superseded, and revoked. No UI,
notification channel, or status transition is implemented.

## Attachments or Other Data

No data upload or attachment workflow exists. Future ingestion is expected to
operate on explicitly included repository documents, preserve source revisions
and locations, and report unsupported formats instead of guessing.

## Accessibility and Mobile Use

No user interface exists, so accessibility and mobile behavior are not
applicable yet. A future client should document keyboard, screen-reader, mobile,
and reduced-motion behavior before claiming support.

## Privacy and Data Handling

Do not add private repository content, credentials, raw transcripts, or
sensitive lesson data to this repository. The planned system requires scope
boundaries, redaction and review before publication, provenance, correction,
revocation, and deletion semantics.

## Troubleshooting

If a command or integration is described as available but cannot be found in
the repository, treat the documentation as stale: there is currently no
executable implementation. Check [PLAN.md](../PLAN.md) for whether the
behavior is planned, deferred, or explicitly out of scope.

## Getting Help

Use GitHub issues in the repository for public questions and proposed changes.
Include the relevant roadmap phase, expected evidence, and the exact
documentation or command involved. Do not include secrets or private source
content in a public issue.
