Seem to mostly come up around closures. The syntax is a way to specify that a function is generic over *any* lifetime.

```rust
fn apply<F, R>(s: &str, f: F) -> R where F: for<'a> Fn(&'a str) -> R {
	f(s)
}
```