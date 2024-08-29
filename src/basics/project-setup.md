## Project Setup

In this chapter we are going to cover how to setup the project. We create a new project using `cargo`:

```bash
cargo new simplang
```

But we are not done yet. What we are going to use is cargo workspaces. This allows us to have multiple crates in one project and allows us to reuse code between them. We are going to setup the `Cargo.toml` file like this:

```toml
[workspace]
members = []
resolver = "2"
```

This ensures that all crates that we create inside the `simplang` project are part of the workspace.

We will have the following crates:

- `simplang-core`: This crate will contain lexer and parser code.
- `simplang-analyzer`: This crate will contain the analyzer code.
- `simplang-compiler`: This crate will contain the compiler code.

This might change as we progress through the book. This page will be updated accordingly.

## Building the Project

To build the project we can use the following command:

```bash
cargo run
```

## Librarys

For the early stages we will probably do not need any additional libraries. Some libraries which we might need include:

1. [`miette`](https://crates.io/crates/miette) or [`thiserror`](https://crates.io/crates/thiserror) for error handling.
2. [`log`](https://crates.io/crates/log) for logging.
3. [`clap`](https://crates.io/crates/clap) for command line argument parsing.
4. [`serde`](https://crates.io/crates/serde) and [`serde_json`](https://crates.io/crates/serde_json) for serialization and deserialization.
5. [`llvm_sys`](https://crates.io/crates/llvm-sys) and maybe [`inkwell`](https://crates.io/crates/inkwell) for LLVM bindings.

We will add these libraries as we need them.

## Final

If you have any issues take a look at the [GitHub repository](https://github.com/DevInSilence/simplang/tree/setupV1). If you have any questions or suggestions feel free to open an issue or a pull request.
