# Table of Contents

- [[#Scan Iterator|Scan Iterator]]
- [[#Cycling Iterator|Cycling Iterator]]
- [[#Encryption using a cycling iterator|Encryption using a cycling iterator]]
- [[#Implementing a custom iterator|Implementing a custom iterator]]

## Scan Iterator

```rust
fn cumsum(n: &[i32]) -> Vec<i32> {
    (0..n.len())
        .scan(0, |sum, i| {
	        *sum += n[i];
	        Some(*sum)
        })
        .collect()
}
```

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

## Implementing a custom iterator

```rust
struct Fib {
    prev: u64,
    curr: u64,
    n: u64,
}

impl Fib {
    fn new() -> Self {
        Self { prev: 0, curr: 1, n: 0 }
    }
}

impl Iterator for Fib {
    type Item = u64;

    fn next(&mut self) -> Option<Self::Item> {
        if self.n == 0 {
            self.n += 1;
            Some(0)
        } else if self.n == 1 {
            self.n += 1;
            Some(1)
        } else {
            let nxt = self.prev + self.curr;
            self.prev = self.curr;
            self.curr = nxt;
            Some(nxt)
        }
    }
}
```

```rust
Fib::new().take(10).collect::<Vec<u64>>()
```
