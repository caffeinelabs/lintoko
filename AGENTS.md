# AGENTS.md

lintoko is a Rust CLI that lints Motoko source using tree-sitter queries defined in TOML rule files.

## Build, test, lint, format

The `justfile` defines the common tasks (requires the [`just`](https://just.systems/) runner):

- `just build` — `cargo build`
- `just build-release` — `cargo build --release`
- `just test` — `cargo test`
- `just test-accept` — accept insta snapshot changes (`INSTA_UPDATE=always cargo test`)
- `just fmt` — `cargo fmt --all`
- `just lint` — `cargo clippy --all-targets -- -D warnings` then `cargo fmt --all --check`
- `just` (default) — runs `lint` then `test`

These also work directly with `cargo` if `just` is unavailable.

## CI

`.github/workflows/ci.yml` runs on pushes and PRs to `main` and will fail unless all pass:

- `cargo build --verbose`
- `cargo test --verbose`
- `cargo clippy --all-targets -- -D warnings` (clippy warnings are hard errors)
- `cargo fmt --all --check` (unformatted code fails CI)

Run `just lint && just test` before pushing.

## Layout

- `src/` — Rust crate (`lib.rs`, `main.rs`, custom tree-sitter predicates in `custom_predicates.rs`).
- `src/snapshots/` — insta snapshot fixtures for tests; regenerate with `just test-accept`, never hand-edit.
- `example-rules/` — example TOML lint rules; also used as test input (see snapshot tests).
- `test-data.mo` — Motoko fixture compiled into snapshot tests via `include_str!`.

## Conventions and gotchas

- Rust edition is 2024; install the toolchain via `rustup`.
- The `tree-sitter-motoko` grammar is a git dependency pinned to a specific `rev` in `Cargo.toml`; snapshot output can change when it is bumped, requiring `just test-accept`.
- Snapshot tests set `NO_COLOR=1`; keep output deterministic.
- `deny.toml` restricts allowed dependency licenses.
- Releases are automated by `cargo-dist` (config in `dist-workspace.toml`); see `DEVELOPMENT.md` for the tag/release flow. Bump the version in `Cargo.toml` and rebuild to update `Cargo.lock`.
- `/target` is the only gitignored path.
