# AGENTS.md

lintoko is an extensible linter for Motoko, written in Rust, that matches Tree-sitter queries against source files.

## Build, test, lint, format

The project uses Cargo. A [`justfile`](./justfile) wraps common tasks; run either the `just` recipe or the underlying `cargo` command.

- Build: `cargo build` (or `just build`)
- Test: `cargo test` (or `just test`)
- Lint: `cargo clippy --all-targets -- -D warnings` (or `just lint`, which also runs the format check)
- Format: `cargo fmt --all` (or `just fmt`)

CI (`.github/workflows/ci.yml`) runs `cargo build`, `cargo test`, `cargo clippy --all-targets -- -D warnings`, and `cargo fmt --all --check`. Clippy warnings and unformatted code fail CI.

## Testing conventions

Tests use snapshot testing via [`insta`](http://insta.rs/). Committed snapshots live in `src/snapshots/*.snap`. When intended output changes, regenerate snapshots with `INSTA_UPDATE=always cargo test` (or `just test-accept`) and commit the updated `.snap` files. Do not hand-edit snapshot files.

## Layout

- `src/` — Rust source (`main.rs` binary, `lib.rs`, `custom_predicates.rs`).
- `example-rules/` — example lint rules; each rule is a TOML file with `name`, `description`, and a Tree-sitter `query`.

## Gotchas

- Rust edition is `2024`; use a recent stable toolchain (install via rustup).
- `tree-sitter-motoko` is pinned to a specific git revision in `Cargo.toml`; changing it requires updating `Cargo.lock`.
- `/target` is git-ignored and must not be committed.
- Releases are tag-driven (`vX.Y.Z`) and built by `cargo-dist`, configured in `dist-workspace.toml`; see [`DEVELOPMENT.md`](./DEVELOPMENT.md) for the release steps.

## Running

Rules are passed with `-r <dir>` (repeatable); `--fix` applies rules that define automatic fixes:

```bash
cargo run -- -r example-rules src
```
