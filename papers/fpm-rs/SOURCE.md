# Source: fpm-rs

- Docs site (human-facing): https://hgrecco.github.io/fpm-rs/
- Repo: https://github.com/hgrecco/fpm-rs
- Pinned commit: `6edad45926d84281943b8084cdc44141aedb3248` (2026-08-05, branch `main`)
- License: MIT
- Languages: Rust (core, ~1.5 MB), Python bindings via PyO3 (~237 KB), plus
  example notebooks

Rust library + Python bindings for **image-plane Fourier ptychographic
microscopy**: compiling illumination geometry, simulating acquisitions, and
reconstructing the complex object (AP, FPIE, EPRY, ADMM, gradient descent
algorithms). Diffraction-plane ptychography, multislice propagation, and GPU
execution are explicitly out of scope (stated in the repo's own README and
`AGENTS.md`) — not a gap to report.

Clone the pinned commit into the job's own sandbox to work with it; don't
read a working copy checked out anywhere else on this machine:

```sh
git clone https://github.com/hgrecco/fpm-rs.git
cd fpm-rs && git checkout 6edad45926d84281943b8084cdc44141aedb3248
```

The repo ships its own `AGENTS.md` with working rules for exactly this kind
of task (module map, documentation policy, canonical commands). Read it
before cataloguing anything — `OBJECTIVE.md` builds directly on it.
