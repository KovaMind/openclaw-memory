# Changelog

All notable changes to `@kovamind/openclaw-memory` are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-06-02

First stable release. The connector contract (tools, endpoints, request shapes)
is now considered stable for integrators.

### Security
- Upgraded `vitest` dev-dependency from `^3.0.0` to `^4.1.8`, resolving the
  critical advisory [GHSA-5xrq-8626-4rwp](https://github.com/advisories/GHSA-5xrq-8626-4rwp)
  (arbitrary file read/execute when the Vitest UI server is listening).
- The vitest 4 upgrade transitively pulls in patched `vite`, `picomatch`, and
  `postcss`, clearing the remaining high/moderate dev-dependency advisories.
  `npm audit` now reports **0 vulnerabilities**.

### Added
- `CHANGELOG.md` (this file).
- README **Errors** section documenting the Phase-2 bound-key `403` failure
  mode (`API key is bound to a different agent identity`).

### Notes
- No runtime/`src` behavior changed in this release. All 136 tests pass
  unchanged against vitest 4.1.8.

## [0.4.x] - 2026-05-28 → 2026-06-01 (unpublished)

Contract-correctness fixes shipped to the live bots ahead of the public release.

### Fixed
- **Manifest tool registration** (#5): declare all 12 tools in
  `contracts.tools` so they register on OpenClaw runtime `>=2026.x`, which
  rejects any `registerTool` name not listed in the manifest.
- **Recall field mapping** (#4): map the Kova API response shape
  (`pattern_id`/`content`/`pattern_type`) to the canonical
  (`id`/`pattern`/`category`) shape via `toPattern()`, fixing silently-disabled
  memory injection. Both naming schemes are accepted for compatibility.
- **API base URL** (#3): corrected the domain (`kovamind.ai` → `kovamind.io`)
  and added the missing `/api` path prefix.

## [0.3.0] - 2026-03-19

### Changed
- Removed the original vault tools pending a redesign around opaque handles.

## [0.2.0] - 2026-03-19

### Added
- Initial vault tools release (later superseded by vault v2).

## [0.1.0] - 2026-03-19

### Added
- Initial release: OpenClaw memory plugin backed by the Kova Mind cloud memory API.
- Memory tools: `memory_recall`, `memory_store`, `memory_forget`,
  `memory_surprise`, `memory_reinforce`.
- Lifecycle hooks: auto-recall before each agent turn, auto-capture after.
- Prompt-injection guard, HTML/XML escaping, and untrusted-memory wrapping.
- `kovamind` CLI subcommands (`status`, `search`, `surprise`).

[1.0.0]: https://github.com/KovaMind/openclaw-memory/releases/tag/v1.0.0
[0.3.0]: https://github.com/KovaMind/openclaw-memory/releases/tag/v0.3.0
[0.2.0]: https://github.com/KovaMind/openclaw-memory/releases/tag/v0.2.0
[0.1.0]: https://github.com/KovaMind/openclaw-memory/releases/tag/v0.1.0
