A byte string literal is a sequence of raw ASCII bytes represented as a slice of `u8` (`&[u8]`). It's useful for handling binary data or ASCII-only strings.

```rust
let byte_str = b"Hello, world!";
println!("{:?}", byte_str);  // [72, 101, 108, ..] (&[u8])
```