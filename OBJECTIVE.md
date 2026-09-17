GOAL
====
A complete, executed audit of `fpm-rs` (Rust core + Python bindings for
image-plane Fourier ptychographic microscopy) at the pinned commit in
`papers/fpm-rs/SOURCE.md`, deliverable as a report the maintainer (hgrecco)
can act on directly.

Every public API item — every public Rust item under `src/` (the crate
enforces `#![deny(missing_docs)]`, so the rustdoc listing IS the full public
surface) and every public Python item under `python/fpm_rs/` (the repo
states these checked-in stubs are authoritative) — gets:

1. a runnable reproduction that actually exercises it (not a read of the
   source), kept in `out/repros/` so it can be reused afterward;
2. a verified status: works as documented / broken / flaky / behaves
   differently from what the docs say;
3. for anything not simply "works": a concrete, specific improvement
   proposal — not "this seems off" but what's wrong and what a fix would
   look like.

This is original review work on someone else's real, MIT-licensed, public
project — not reproducing a known answer. Nothing here gets sent to the
maintainer automatically; the job produces the report, and a human decides
whether/how to hand it over afterward.


SOURCES
=======
`papers/fpm-rs/SOURCE.md` — the pinned commit, clone command, and scope.

Inside that clone, in priority order:
- `AGENTS.md` — the repo's own map of `src/*` modules, its documentation
  policy, and its working rules. Follow these when writing findings (cite
  file+line, state implementation location/assumptions, don't invent
  citations, report doc/code disagreements rather than silently picking a
  side).
- `README.md` — architecture, the array-layout contract, the canonical
  Python quickstart.
- `ROADMAP.md` — what the maintainer already knows is missing or planned;
  don't re-report these as newly-discovered bugs.
- `docs/` (MkDocs: `docs/spherical-geometries.md`, `dataset_spec.md`,
  `docs/datasets.md`, `docs/benchmarks.md`, `docs/reference/`), `examples/`,
  `tests/` — what the library claims and how it's meant to be used.
- The published site, https://hgrecco.github.io/fpm-rs/, for how a user
  arriving from outside the repo is told to use it — compare that framing
  against what the code actually does.


FENCE -- do not do this
========================
Do NOT open an issue or PR, push a commit, or otherwise write to
`github.com/hgrecco/fpm-rs` from this job. The deliverable is a local
report and a local set of reproductions; sending anything to the maintainer
is a separate, human-approved step outside this job.


WHAT YOU CANNOT GET FROM THE SOURCES
=====================================
- The tolerance that counts as "reconstruction converged correctly" for
  each algorithm (AP, FPIE, EPRY, ADMM, gradient descent) against a
  synthetic object with known ground truth. Pick a stated, reasonable
  metric and threshold (e.g. relative amplitude/phase error, or SSIM) and
  say so explicitly — don't leave it implicit in a script.
- How to prioritize if the round budget doesn't stretch to equal depth on
  every item: prioritize breadth first (every public item gets at least an
  executed smoke test) over exhaustive depth on a few. Say in the report
  which items only got a smoke test.
- The project's own scope fence: image-plane FPM only. Diffraction-plane
  ptychography, multislice propagation, and GPU execution are explicitly
  out of scope per the repo's own README/AGENTS.md — do not file these as
  missing-feature findings.


DISCIPLINE
==========
- Environment: the repo uses `pixi` (`pixi.toml`/`pixi.lock`) plus Cargo.
  Canonical commands, from the repo's own `AGENTS.md`:
  `cargo test --all-targets`, `pixi run -e py312 python-test`,
  `pixi run rust-doc`, `pixi run python-api-docs`, `pixi run ci`
  (the full suite). Run these, don't just read whether they'd probably
  pass.
- Also run the Rust examples themselves (`cargo run --example
  simulate_and_reconstruct`, `cargo run --example benchmark_algorithms`,
  and any others under `examples/`) — they're part of the public-facing
  catalog too.
- No network access beyond the one clone of the pinned commit and
  installing declared dependencies — matches the project's own rule
  ("do not add network access to builds or tests").
- Work only inside this job's own sandbox clone of `fpm-rs`. Never touch
  any other checkout of it on this machine.
- Every "works" / "doesn't work" claim must come from something this job
  actually executed and can point to (a log, a test run, a script output)
  — never from reading the source and inferring behavior.


DEFINITION OF DONE
===================
`out/checks.py` runs and prints, per check:
- **Coverage**: N/M public Rust items catalogued, N/M public Python items
  catalogued (M from an actual rustdoc/stub listing generated in this job,
  not eyeballed) — 100% or the report says explicitly what's missing and
  why.
- **Execution**: every reproduction under `out/repros/` actually runs;
  pass/fail is recorded, nothing is marked "works" on an unexecuted or
  currently-failing repro.
- **Findings paired**: every catalog row marked broken/flaky/discrepant has
  a matching improvement proposal in the report — no orphaned bad status.

Include at least one deliberately-broken case in `checks.py` (e.g. a
reproduction stubbed to fail) so the check suite is shown capable of
catching a real problem, not just rubber-stamping green.

Paste the check output (the coverage numbers) in the final report.


DELIVERABLE
===========
1. `out/notes.tex` — the spine: one entry per catalogued item (module,
   name, kind, status, the improvement proposal if any), each quantitative
   or pass/fail claim carrying a `\src{key}` resolving in
   `out/provenance.json`.

2. `out/report.html` — self-contained, opens from a `file://` URL with NO
   network (no CDN, no external stylesheet, no remote font, no fetch),
   written for hgrecco to read directly: a sortable/filterable catalog
   table (module, item, status, link to its repro, notes) plus a clearly
   separated "Proposed improvements" section, one entry per non-working
   item, concrete enough to act on.

3. `out/repros/` — one runnable reproduction per catalogued item (a Rust
   example or a Python script, matching whichever binding surface the item
   belongs to), each independently runnable without editing.

`out/provenance.json` is the registry both `notes.tex` and `report.html`
resolve against — every claim traces to a specific command or file this job
ran, not to a read of the source.
