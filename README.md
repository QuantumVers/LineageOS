# LineageOS

A single-file, local-first ancestry research workspace. Open `lineageos.html` directly in a modern browser. No build, account or server required. `index.html` is the identical hosting entry point.

## Available now

- Search 320 synthetic example people, or start a blank tree.
- Import a Goldman atlas HTML file (`script#family-data`), a LineageOS JSON backup, or basic GEDCOM.
- Explore ancestors, descendants and parent/sibling/child branches; zoom, pan and change depth.
- Inspect exact relationship evidence, explicit reviewed classifications and dossier sources.
- Calculate a real shortest parent–child path or nearest shared recorded ancestor.
- Create/edit dossiers and add unverified parent claims with cycle checks.
- Save to IndexedDB in this browser; export/import portable JSON backups.
- Preserve the original imported research log, archive, photos, source claims, IDs and relationship metadata.

The public seed is entirely synthetic. **No private Goldman research is committed here.** Importing a file does not upload it. Google Fonts is the only external request used by the application. It works with system fonts offline.

## Evidence model

Canonical data uses `persons`, `connections`, and optional `presentation.relationships`. Original fields remain intact. Each connection has `from`, `to`, `forward`, `reverse`, `evidence`, and optional `sources`. The presentation key is `[from,to,forward,reverse].join('|')` and may contain explicit parent/child IDs and an evidence review classification.

Only typed parent–child edges appear in ancestry traversal. Spouses, siblings, associations and candidate identity matches do not become parent links. New or changed relationships remain unverified. Review classifications are preserved only when their exact evidence matches. Numeric confidence percentages are not invented or interpreted as calibrated probabilities.

## Import limits

This release supports up to 10,000 people, 30,000 connections and 65 MB per file. Graph display is capped at 160 people; full imported data remains in the backup and searchable reports. Browser performance varies for large archives.

Basic GEDCOM import reads INDI names, BIRT/DEAT dates and places, simple NOTE text, SOUR pointers and FAM parent assertions. It does not resolve source record pointers, multimedia, advanced adoption/relationship subtypes, alternate names, or all GEDCOM extensions. The original GEDCOM text is preserved for later processing. Inspect the import before relying on it.

## Privacy and durability

Browser-local storage is not encrypted, an access-control system, a cloud backup, or multiuser sync. People using the same browser profile can access the saved workspace. Clearing site data removes it. Export backups routinely. File-origin storage behavior differs between browsers; if storage fails, the app reports session-only operation.

Exports may contain living people's information, private notes and original archives. Share only with permission. The app does not publish a tree automatically.

## What is not connected

No live historical-record API, DNA provider, AI prediction service, account/auth backend, billing, invitation service or shared global graph. DNA percentages and routes are synthetic only in demo mode. Real workspaces keep unavailable DNA and unstructured migrations unknown. No Lincoln connection, medieval pedigree, source or match is manufactured.

## Validation

`tests/smoke.cjs` uses Playwright to exercise the public app and graph primitives. See `IMPLEMENTATION_HANDOFF.md` for delivery validation and remaining production work.
