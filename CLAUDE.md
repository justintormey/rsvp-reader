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
