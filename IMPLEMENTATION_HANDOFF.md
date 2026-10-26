# LineageOS implementation guide

## Current release

LineageOS is a single-file, local-first ancestry research workspace. The public seed is synthetic. The code supports private atlas imports without sending those files to a server.

## Data integration

Use **Import tree** to load a supported atlas HTML (`script#family-data`), JSON backup, or basic GEDCOM file. Export a backup before replacing an existing workspace. The importer preserves the source object, including dossiers, IDs, source links, original text, relationship metadata and any embedded archive. Imports and edits do not change the original source file.

Canonical fields: `persons`, `connections`, optional `presentation.relationships`, `researchLog`, `archiveHtml`, and `originalData`. Only typed parent–child links are traversed. Review classifications are reused only when exact evidence text matches. New links remain unverified. Candidate identities stay separate until reviewed connecting evidence exists.

## Prototype behavior replaced

- Removed the invented named-user family and hardcoded celebrity connection.
- Implemented breadth-first graph traversal and shared-ancestor lookup.
- Replaced fixed confidence scores and fabricated documents with saved evidence.
- Kept unavailable DNA and unstructured migrations unknown in real workspaces.
- Added dossier editing, parent claims, local persistence and portable backups.

## Production boundary

This is a personal browser application, not a public collaborative genealogy service. Browser storage is not authentication, access control, multiuser sync or permanent cloud backup. Publishing this code does not publish imported trees.

Before shared accounts launch, implement and verify:
1. Authentication and server-side authorization for every workspace operation.
2. Consent and privacy controls for living people, invitation acceptance and revocation.
3. Durable claims, immutable provenance, review history and optimistic concurrency.
4. Managed backups, deletion/export, retention and recovery tests.
5. Conflict-safe collaboration and human-reviewed identity resolution.
6. Licensed, approved integrations; do not imply provider access that is not connected.
7. Abuse controls, load tests, accessibility review and security assessment.

`CHATGPT_HANDOFF.md` is historical prototype context. Its invented names, percentages, DNA and roadmap claims are superseded by this implementation. They must not be treated as family evidence or current product capabilities.
