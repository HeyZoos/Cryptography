In Rust, the `%` is just a remainder, not a modulo operation. If you want something to behave like the `%` operator in Python you need to use the Euclidean remainder.

```rust
pub const fn rem_euclid(self, rhs: i8) -> i8
```

- [Why % works differently between Rust and Python](https://users.rust-lang.org/t/why-works-differently-between-rust-and-python/83911)
