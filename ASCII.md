- [[#ASCII Range|ASCII Range]]
- [[#Byte Value|Byte Value]]
- [[#Byte String Literal|Byte String Literal]]

## ASCII Range

0 to 127

## Byte Value

The byte value of an ASCII character.

```rust
b'0' == 48
```

This does not work with non-ASCII characters.

```rust
error: non-ASCII character in byte literal
 --> src/main.rs:2:12
  |
2 |     dbg!(b'你');
  |            ^^
  |            |
  |            must be ASCII
  |            this multibyte character does not fit into a single byte
```

You can get this value dynamically by casing the `char` to a `u8`. This would also give you a number for Unicode characters so it's best to check if the character falls within the ASCII range first.

```rust
if c.is_ascii() {
	todo!()
}
```

Otherwise you will chop off part of the Unicode character. For example, the character `Ķ` should have a decimal value of `310`. But casting it to just a byte produced the decimal value `54`.

```rust
let c = 'Ķ';
println!("{:b}", c as u32);   // 100110110
println!("{:09b}", c as u8);  // 000110110
```

## Byte String Literal

A byte string literal is a sequence of raw ASCII bytes represented as a slice of `u8` (`&[u8]`). It's useful for handling binary data or ASCII-only strings. Also does not work with any non-ASCII characters.

```rust
let byte_str = b"Hello, world!";
println!("{:?}", byte_str);  // [72, 101, 108, ..] (&[u8])
```

