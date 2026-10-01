# 0.12.0
- feat: updates the grammar to [tree-sitter-motoko v0.2.2](https://github.com/caffeinelabs/tree-sitter-motoko/releases/tag/v0.2.2), which parses the moc 2.0 syntax (up to 2.0.0-beta.4) alongside the existing moc 1.x syntax: unparenthesized `if`/`while`/`for`/`switch` heads, lighter `case`s, `and`/`or` in conditions, `do { }` operands, `5.toText()`, `<system, T>`
- fix: unspaced `x-1` / `x+1` now parse as binary expressions instead of calls, so rules matching `bin_exp_*` see them (e.g. the `assign-minus` / `assign-plus` example rules)
- breaking (custom rules): number literals no longer carry a sign (`-1` is `(unop_exp (unop) (lit_exp (int_literal)))`), `unop_pat` sits at the unary pattern level, and `await? e` is its own `awaitquest_exp` node. Rules that match parenthesized `switch` scrutinees or `case (?_)` need extra patterns for the bare forms

# 0.11.0
- feat: updates the grammar to parse `and-patterns`, `system-mixins`, and `null-coalescing`

# 0.10.0
- feat: add per-rule `severity` field (`"warning"` or `"error"`, defaults to `"error"`)
- feat: add `--severity` CLI flag to override severity for all rules
- feat: add per-rule `includes` / `excludes` glob fields for scoping rules to file paths (with `allowed-directories` and `types-only` example rules)

# 0.9.0
- feat: allows matching/linting on nesting depth [#31](https://github.com/caffeinelabs/lintoko/pull/31)

# 0.8.0
- chore: updates grammar version
- chore: produces linux-arm64 binaries on release

# 0.7.0
- feat: allows specifying fixes in rules, and automatically applies with the `--fix` flag

# 0.6.0
- breaking: default rules are no longer a thing. All rules need to be passed via command line flags

# 0.5.1
- chore: updates grammar version

# 0.5.0
- feat: allow passing a single rule to iterate on
- feat: adds rules for binary assignment operators *, /, and #
- chore: updates grammar version

# 0.4.3
- chore: updates tree-sitter version to support mixins and weak references

# 0.4.2
- fix: Don't error on non-persistent actors, the compiler takes care of that

# 0.4.1
- fix: also check casing on classes and type parameters
- feat: adds a textual output format that's easier to consume with AI or screen readers

# 0.4.0
- feat: lint casing for type and function definitions
- fix: print the correct version number when calling `lintoko --version`
- chore: infra/license/etc changes to support Open Sourcing

# 0.3.2
- fix: Don't suggest punning for var fields

# 0.3.1
- feat: allows passing directories, files and globs to the CLI
    Also makes it so no arguments expand to all Motoko files underneath
    the current directory

# 0.3.0
- feat: lints unneeded returns
- fix: make linting for switches over booleans more precise (#2)

# 0.2.1
- chore: Updates release process

# 0.2.0
- Makes assign-minus, assign-plus, no-bool-switch, and pun-fields rules default
- Adds rule guarding against pure/ imports
- Adds rule to disallow non-primitive return types from public functions
