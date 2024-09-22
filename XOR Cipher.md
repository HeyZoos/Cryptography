This is my solution to the [Single Byte XOR Cipher](https://cryptopals.com/sets/1/challenges/3) challenge in Cryptopals. I feel like there were a couple of lessons learned:

- The key was `'X'`, but the best key was `88`, I think there's more to be learned here
- I really did just need to check all possible hex characters, but I assumed it was all lower-case which was a mistake
	- Brute forcing helped me find the answer fastest
- A super simple scoring method is really all I needed

```rust
fn main() {  
    dbg!(crack("1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736"));  
}
  
fn crack(ciphertext: &str) -> (u8, u32, String) {  
    let mut best_key = None;  
    let mut best_score = u32::MIN;  
    let mut best_plaintext = None;  
  
    for key in 0..=255u8 {  
        let bytes: Vec<u8> = (0..ciphertext.len()) // [0, 1, 2, ...]  
            .step_by(2) // [0, 2, 4, ...]  
            .map(|i| &ciphertext[i..=i + 1]) // ["1b", "37", "37", ...]  
            .map(|chunk| u8::from_str_radix(chunk, 16).unwrap()) // [27, 55, 55, ...]  
            .map(|byte| byte ^ key) // [20, 56, 56, ...]  
            .collect();
            
        let plaintext = String::from_utf8(bytes);  
  
        if let Ok(plaintext) = plaintext {  
            let score = score(&plaintext);
            if score > best_score {  
                best_score = score;  
                best_key = Some(key);  
                best_plaintext = Some(plaintext);  
            }        
        }    
    }

	println!("best key = {}", best_key.unwrap().to_ascii_lowercase());  // Gave me 120 == 'X'???
      
    (best_key.unwrap(), best_score, best_plaintext.unwrap())  
}

  
fn score(plaintext: &str) -> u32 {  
    let mut score = 0;  
  
    plaintext.chars().for_each(|c| {  
        if c.is_alphabetic() {  
            score += 1;  
        }
    });
      
    score  
}
```