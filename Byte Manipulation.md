##### Count bits
---
```rust
fn bitcount(mut byte: u8) -> usize {
	let mut count = 0;
	while byte > 0 {
		count += byte & 1;
		byte >>= 1;
	}
	count
}
```

##### Hamming distance
---
```rust
fn hamming(s1: &str, s2: &str) -> usize {
	s1
		.bytes()
		.zip(s2.bytes())
		.map(|(b1, b2)| b1 ^ b2)
		.map(|b| bitcount(b))
		.sum()
}
```

