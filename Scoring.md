# Table of Contents

- [[#English Letter Frequencies|English Letter Frequencies]]
- [[#Scoring by frequency|Scoring by frequency]]

## English Letter Frequencies

https://en.wikipedia.org/wiki/Letter_frequency

```rust
fn get_english_letter_frequencies() -> HashMap<char, f32> {  
    let mut freqs = HashMap::new();  
    freqs.insert('a', 8.167);  
    freqs.insert('b', 1.492);  
    freqs.insert('c', 2.782);  
    freqs.insert('d', 4.253);  
    freqs.insert('e', 12.702);  
    freqs.insert('f', 2.228);  
    freqs.insert('g', 2.015);  
    freqs.insert('h', 6.094);  
    freqs.insert('i', 6.966);  
    freqs.insert('j', 0.153);  
    freqs.insert('k', 0.772);  
    freqs.insert('l', 4.025);  
    freqs.insert('m', 2.406);  
    freqs.insert('n', 6.749);  
    freqs.insert('o', 7.507);  
    freqs.insert('p', 1.929);  
    freqs.insert('q', 0.095);  
    freqs.insert('r', 5.987);  
    freqs.insert('s', 6.327);  
    freqs.insert('t', 9.056);  
    freqs.insert('u', 2.758);  
    freqs.insert('v', 0.978);  
    freqs.insert('w', 2.360);  
    freqs.insert('x', 0.150);  
    freqs.insert('y', 1.974);  
    freqs.insert('z', 0.074);  
    freqs  
}
```

## Scoring by frequency

My biggest take aways from this scoring function is that:

1. It is _very_ helpful to include some sort of _penalty_ for characters we don't want
2. You can get away with treating a character's frequency as the score to add
3. It's not enough to just look up the frequencies of the vowels

```rust
fn score(plaintext: &str) -> f32 {
    let freqs = get_english_letter_frequencies();
    let mut score = 0.0;

    for c in plaintext.chars() {
        let c = c.to_ascii_lowercase();
        if let Some(&frequency) = freqs.get(&c) {
            score += frequency;
        } else if c.is_ascii_control() {
            score -= 50.0;
        } else {
            score -= 5.0;
        }
    }

    score
}
```