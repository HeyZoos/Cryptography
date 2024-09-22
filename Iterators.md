# Table of Contents

## Cycling Iterator

This is extremely useful, especially for the application of a repeating key. The following example:

```rust
let nums = vec![1, 2, 3];
let mut cyclic_iter = nums.iter().cycle();
for _ in 0..10 {
    println!("{}", cyclic_iter.next().unwrap());
}
```

Will produce:

```
1
2
3
1
2
3
1
2
3
1
```

## Encryption using a cycling iterator

```rust
fn to_hex(bytes: Vec<u8>) -> String {  
    let mut output = String::new();  
  
    bytes.iter().for_each(  
        |byte| output.push_str(format!("{:02x}", byte).as_str())  
    );  
    output  
}  
  
fn encrypt(plaintext: &str, key: &str) -> String {  
    let mut repeating_key = key.as_bytes().iter().cycle();  
    let mut output = vec![];  
  
    for plaintext_byte in plaintext.as_bytes() {  
        let key_byte = repeating_key.next().unwrap();  
        output.push(plaintext_byte ^ key_byte);  
    }  
    to_hex(output)  
}
```