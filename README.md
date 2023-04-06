# justerror
[<img alt="github" src="https://img.shields.io/badge/github-shakacode/justerror-8da0cb?style=for-the-badge&labelColor=555555&logo=github" height="20">](https://github.com/shakacode/justerror)
[<img alt="crates.io" src="https://img.shields.io/crates/v/justerror.svg?style=for-the-badge&color=fc8d62&logo=rust" height="20">](https://crates.io/crates/justerror)
[<img alt="docs.rs" src="https://img.shields.io/badge/docs.rs-justerror-66c2a5?style=for-the-badge&labelColor=555555&logo=docs.rs" height="20">](https://docs.rs/justerror)
<!-- cargo-sync-readme start -->

This macro piggybacks on [`thiserror`](https://github.com/dtolnay/thiserror) crate and is supposed to reduce the amount of handwriting when you want errors in your app to be described via explicit types (rather than [`anyhow`](https://github.com/dtolnay/anyhow)).

> ### ShakaCode
> If you are looking for help with the development and optimization of your project, [ShakaCode](https://www.shakacode.com) can help you to take the reliability and performance of your app to the next level.
>
> If you are a developer interested in working on Rust / ReScript / TypeScript / Ruby on Rails projects, [we're hiring](https://www.shakacode.com/career/)!

## Installation

Add to `Cargo.toml`:

```toml
justerror = "0.1"
```

Add to `main.rs`:

```rust
#[macro_use]
extern crate justerror;
```

## Usage
This macro takes a subject struct or enum and applies `thiserror` attributes with predefined `#[error]` messages.

Generally, you can attach `#[Error]` macro to an error type and be done with it.

```rust
#[Error]
enum EnumError {
    Foo,
    Bar {
        a: &'static str,
        b: usize
    },
}

eprintln!("{}", EnumError::Bar { a: "Hey!", b: 42 });

// EnumError::Bar
// === ↴
// a: Hey!
// b: 42
```

Macro accepts two optional arguments:
- `desc`: string
- `fmt`: `display` | `debug` | `"<custom format>"`

Both can be applied at the root level.

```rust
#[Error(desc = "My emum error description", fmt = debug)]
enum EnumError {
    Foo(usize),
}
```

And at the variant level.

```rust
#[Error(desc = "My emum error description", fmt = debug)]
enum EnumError {
    #[error(desc = "Foo error description", fmt = display)]
    Foo(usize),
}
```

`fmt` can also be applied to individual fields.

```rust
#[Error(desc = "My emum error description", fmt = debug)]
enum EnumError {
    #[error(desc = "Foo error description", fmt = display)]
    Foo(#[fmt(">5")] usize),
}
```

See [tests](tests/tests.rs) for more examples.

<!-- cargo-sync-readme end -->

## License
MIT.
