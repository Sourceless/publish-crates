[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![CI Status](https://github.com/katyo/publish-crates/workflows/build-test/badge.svg)](https://github.com/katyo/publish-crates/actions)

# Publish Rust crates using GitHub Actions

## Features

- Reads manifests to get info about crates and dependencies
- Checks versions of external dependencies to be exists in registry
- Checks matching paths and versions of internal dependencies
- Checks that no changes makes since last release when version of internal dependency is not changed
- Skips publising of internal dependencies which does not updated
- Publishes crates with `cargo publish` in right order according dependecies
- Awaits when published crate will be awailable in registry before publishing crates which depends from it
- Works fine with workspaces without cyclic dependencies

## Unimplemented features

- Supports different registries than [crates.io](https://crates.io/)

## Inputs

- `path` Sets path to crate or workspace ('.' by default)
- `args` Extra arguments for `cargo publish` command
- `dry-run` Set to 'true' to bypass exec `cargo publish`

## Usage examples

Basic usage (`Cargo.toml` sits in repository root):

```yaml
steps:
    - uses: actions/checkout@v2
    - uses: actions-rs/toolchain@v1
      with:
          toolchain: stable
          override: true
    - uses: katyo/publish-crates@v1
      env:
          CARGO_REGISTRY_TOKEN: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```

Advanced usage (`Cargo.toml` sits in 'packages' subdir, and you would like to skip verification and bypass real publishing):

```yaml
steps:
    - uses: actions/checkout@v2
    - uses: actions-rs/toolchain@v1
      with:
          toolchain: stable
          override: true
    - uses: katyo/publish-crates@v1
      with:
          path: './packages'
          args: --no-verify
          dry-run: true
      env:
          CARGO_REGISTRY_TOKEN: ${{ secrets.CARGO_REGISTRY_TOKEN }}
```
