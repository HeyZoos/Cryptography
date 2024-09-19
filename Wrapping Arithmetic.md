This is a useful utility to keep in mind, especially when implementing something like the [[Caesar Cipher]] where you want to keep your values within a certain range. 

```rust
fn wrapping_add(input: u8, addend: u8, min: u8, max: u8) -> u8 {  
    let range = max - min;  
    let normalized = input - min;  // normalize starting point to 0
    let sum = normalized + addend;  
    (sum % range) + min  // shift value back into desired range
}
```

This implementation only works for unsigned values. It can be written more concisely as:

```rust
fn wrapping_add(input: u8, addend: u8, min: u8, max: u8) -> u8 {
	let range = max - min;
	(input - min + addend) % range + min 
)
```

If the implementation needs to be concerned with negative numbers, swap out the remainder operation for the [[Euclidean Remainder]] to always get a positive number.

```rust
fn wrapping_add(input: i32, addend: i32, min: i32, max: i32) -> i32 {
	let range = max - min;
	(input - min + addend).rem_euclid(range) + min 
)
```