The simplest example of a [[Symmetric]] encryption algorithm. This implementation is for lowercase values only.

```rust
fn caesar(input: &str, key: u8) -> String {  
    let mut output = String::with_capacity(input.len());  
  
    for ch in input.bytes() {  
        output.push(wrapping_add(ch, key, 97, 123) as char);  
    }
    
    output  
}
```