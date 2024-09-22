# Table of Contents

- [[#Decode a hex string to ASCII|Decode a hex string to ASCII]]

## Convert a hex char to its decimal number

```rust
'a'.to_digit(16)  // 10
```
## Decode a hex string to ASCII

1. Given a hex string

```rust
"48656c6c6f20576f726c6421"
```

2. Decode into byte-sized chunks

```rust
["48", "65", "6c", "6c", "6f", "20", "57", "6f", "72", "6c", "64", "21"]
```

```rust
let chunks: Vec<&str> = (0..hex.len())
	.step_by(2)
	.map(|i| &hex[i..=i + 1])
	.collect();
```

3. Convert each hex pair to a decimal byte value

```rust
[72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100, 33]
```

```rust
let bytes: Vec<u8> = chunks
	.iter()
	.map(|chunk| u8::from_str_radix(chunk, 16).unwrap())
	.collect();
```

4. Map bytes to [[ASCII]] characters

```rust
['H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd', '!']
```

5. Combine characters

```rust
"Hello World!"
```

```rust
String::from_utf8(bytes);
```

6. All together!

```rust
fn to_ascii(hex: &str) -> String {
    String::from_utf8(
        (0..hex.len())
            .step_by(2)
            .map(|i| &hex[i..=i+1])
            .map(|bytestr| u8::from_str_radix(bytestr, 16).unwrap())
            .collect()
    ).unwrap()
}
```

## Encode ASCII as a hex string

1. Given an [[ASCII]] string

```rust
"Hello World!"
```

2. Convert the string to bytes

```rust
let bytes: &[u8] = ascii.as_bytes();
```

3. Rust can automatically display a byte representing [[ASCII]] as hex

```rust
let hex: String = bytes
	.into_iter()
	.map(|byte| format!("{:02x}", byte))
	.collect();
```

4. All together!

```rust
fn to_hex(ascii: &str) -> String {
	ascii
		.as_bytes()
		.into_iter()
		.map(|byte| format!("{:02x}", byte))
		.collect()
}
```