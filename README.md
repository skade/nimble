# Nimble

Async friendly, simple and fast binary encoding/decoding in Rust.

## Binary encoding scheme

This crate uses a minimal binary encoding scheme. For example, consider the following `struct`:

```
struct MyStruct {
    a: u8,
    b: u16,
}
```

`encode()` will serialize this into `Vec` of size `3` (which is the sum of sizes of `u8` and `u16`).

Similarly, for types which can have dynamic size (`Vec`, `String`, etc.), `encode()` prepends the size of encoded value
as `u64`.

## Usage

Add `nimble` in your `Cargo.toml`'s `dependencies` section:

```toml
[dependencies]
nimble = { version = "0.1", features = ["derive"] }
```

For encoding and decoding, any type must implement two traits provided by this crate, i.e., `Encode` and `Decode`. For
convenience, `nimble` provides `derive` macros (only when `"derive"` feature is enabled) to implement these traits.

```rust
use nimble::{Encode, Decode};

#[derive(Encode, Decode)]
struct MyStruct {
    a: u8,
    b: u16,
}
```

Now you can use `encode()` and `decode()` functions to encode and decode values of `MyStruct`. In addition to this, you
can also use `MyStruct::encode_to()` function to encode values directly to a type implementing `AsyncWrite` and
`MyStruct::decode_from()` function to decode values directly from a type implementing `AsyncRead`.

> Note: Most of the functions exposed by this crate are `async` functions and returns `Future` values. So, you'll need
an executor to drive the `Future` returned from these functions. `async-std` and `tokio` are two popular options.

### Features

- `tokio`: Select this feature when you are using `tokio`'s executor to drive `Future` values returned by functions in
  this crate.
  - **Enabled** by default.
- `async-std`: Select this feature when you are using `async-std`'s executor to drive `Future` values returned by
  functions in this crate.
  - **Disabled** by default.
- `derive`: Enables derive macros for implementing `Encode` and `Decode` traits.
  - **Disabled** by default.

> Note: Features `tokio` and `async-std` are mutually exclusive, i.e., only one of them can be enabled at a time.
Compilation will fail if either both of them are enabled or none of them are enabled.

## License

Licensed under either of

- Apache License, Version 2.0 (http://www.apache.org/licenses/LICENSE-2.0)
- MIT license (http://opensource.org/licenses/MIT)

at your option.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as 
defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
