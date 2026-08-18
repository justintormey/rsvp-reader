# Flash Reader — Project Notes

Single-file RSVP speed reader. See README.md for the feature rundown and
deploy instructions.

## Constraints

- Keep it a single self-contained `index.html` — no build step, no
  dependencies beyond the two Google Fonts already linked. That's the whole
  point of the project.
- `thumbnail.png` (if present) is the demo-home card image, hosted at
  `demo.justintormey.com/rsvp-reader/thumbnail.png` — it deploys via the same
  `scripts/deploy` sync, no manual upload step.
- Deploy is additive (no `--delete`) because the S3 bucket is shared across
  sibling demo-subpath projects. Never add `--delete` to `scripts/deploy`.

---

## Versioning — Semantic Versioning (mandatory)

This project follows [Semantic Versioning 2.0.0](https://semver.org/): `MAJOR.MINOR.PATCH`. Any agent/LLM making changes here MUST bump the version automatically as part of the change — never wait to be asked.

- **MAJOR** — breaking change: removed/renamed capability, incompatible API/CLI/schema/data-format/UX change
- **MINOR** — new backward-compatible functionality
- **PATCH** — backward-compatible bug fix, perf tweak, copy correction
- Docs-only or internal-refactor changes with no behavior change: no bump
- Pre-1.0 (`0.y.z`): breaking → MINOR, everything else → PATCH; new projects start at `0.1.0`

In the SAME commit as the change, update the version everywhere it appears:
1. **Source of truth** — whatever this repo uses (`package.json`, `VERSION`, `Info.plist`/`project.yml` `MARKETING_VERSION`, `pyproject.toml`, site footer constant). If none exists yet, create a root `VERSION` file at `0.1.0`.
2. **Documentation** — add a `CHANGELOG.md` entry (create the file if missing); update README/docs anywhere a version is stated.
3. **User interface** — not every UI displays a version and that's fine; never add one where none exists. Any surface that already shows a version (About screen, footer, settings, CLI `--version`) must be correct — reading from the single source of truth, never a second hardcoded copy.
4. **GitHub** — tag the release commit `vX.Y.Z` and push the tag with the branch (GitHub Releases for MAJOR/MINOR on repos that use them).
