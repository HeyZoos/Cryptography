## Multiline strings

It's very easy to copy over the input plaintext incorrectly which results in unexpected [[Encryption]] results. For example, here's a test with the correct input:

```rust
#[test]  
fn test_encrypt() {  
    let plaintext = "Burning 'em, if you ain't quick and nimble\nI go crazy when I hear a cymbal";  
    let expected = "0b3637272a2b2e63622c2e69692a23693a2a3c6324202d623d63343c2a26226324272765272a282b2f20430a652e2c652a3124333a653e2b2027630c692b20283165286326302e27282f";  
    let output = encrypt(plaintext, "ICE");  
    assert_eq!(output, expected);  
}
```

But the first few times I copied it over incorrectly, it included a trailing space:

```rust
Burning 'em, if you ain't quick and nimble\nI go crazy when I hear a cymbal
```

Note that if you're outputting [[Hex]] you'll never have any form of whitespace.