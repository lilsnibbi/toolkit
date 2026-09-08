# @lilsnibbi/toolkit

Keep responses minimal: bare essentials, short paragraphs, no padding.
Never commit, push, tag, publish, or run release automation unless explicitly requested.
Preserve existing user changes and public APIs. Prefer focused fixes over broad rewrites.

## Project

Dependency-free helpers for Bun, using strict TypeScript and ESM.
Published source is `src/index.ts`; there is no build step. Use Bun for packages and scripts.

- `src/`: one helper per camelCase file, exported from the root barrel.
- `tests/`: matching `bun:test` files.
- `scripts/`: release metadata, version synchronization, and release automation.

## Conventions

- Use tabs, double quotes, and LF; follow Biome configuration.
- Keep named option types alongside implementations and document public symbols with JSDoc.
- Preserve signatures, root exports, and documented edge-case behaviour.
- Validate numeric bounds; use `RangeError` for out-of-range arguments.
- Keep array helpers non-mutating and test boundaries deterministically.
- `formatSeconds` uses the current date for calendar units; freeze time when testing those cases.
- `truncate` is copied into `../logger/src/core/truncate.ts`; mirror behavioural changes and tests in both repositories.

## Verification

Run `bun run check` (typecheck, tests, release metadata, lint).
Use `bun run audit` and `bun pm pack --dry-run` for dependency and package checks.
CI installs with `bun install --frozen-lockfile`; keep a reproducible `bun.lock`.

## Releases

The single `release.yml` workflow matches an entire `vX.Y.Z` tip commit message on the default branch. It verifies, updates metadata, atomically pushes the version commit and tag, publishes to npm, and generates GitHub release notes. Ordinary commits do not release. See `.github/RELEASE_POLICY.md` for setup and retries.
Release scripts can commit and push. Do not run them during ordinary verification.
Keep duplicated release helpers aligned with sibling `logger` and `discord-kit` repositories.
