# Plain Spec Governor

Plain Spec Governor is a draft OpenCLAW/OmegaCLAW skill and design note for semi-formal, Plain-spec-driven development.

The core workflow is:

1. Write or update a Plain specification before implementation.
2. Classify the task by risk tier.
3. Run a local Plain lint pass.
4. Ask a persistent reviewer subagent, `PlainSpecReviewer`, for adversarial spec review.
5. Gate implementation on the review verdict: `GO`, `REVISE`, or `STOP`.
6. Implement against the final Plain spec, not against earlier chat fragments.
7. Reconcile implementation and tests back to the Plain spec.

This is intended as a pragmatic intermediate layer between informal chat instructions and later formal verification. It aims to catch ambiguity, missing tests, unsafe operational behavior, misleading research design, and brittle architecture before they become code.

## Files

- [`openclaw_plain_spec_governor_skill.md`](openclaw_plain_spec_governor_skill.md) — draft skill file.
- [`openclaw_plain_spec_governor_explainer.tex`](openclaw_plain_spec_governor_explainer.tex) — TeX source for the design note.
- [`openclaw_plain_spec_governor_explainer.pdf`](openclaw_plain_spec_governor_explainer.pdf) — compiled design note PDF.

## Status

Draft for human review, July 2026.

## License

MIT License. See [`LICENSE`](LICENSE).
