- [[#Basic Function|Basic Function]]
- [[#Usage with structs|Usage with structs]]
- [[#Lifetime Subtyping|Lifetime Subtyping]]

Seem to mostly come up around closures. The syntax is a way to specify that a function is generic over *any* lifetime.

##### Basic Function
---

```rust
fn apply<F, R>(s: &str, f: F) -> R where F: for<'a> Fn(&'a str) -> R {
	f(s)
}
```

##### Usage with structs
---

The only gotcha here is that the stored function needs to be invoked surrounded by parentheses `(self.func)` - this is to differentiate it from a method call.

```rust
struct Processor<F, R>
where
    F: for<'a> Fn(&'a str) -> R,
{
    func: F,
}

impl<F, R> Processor<F, R>
where
    F: for<'a> Fn(&'a str) -> R,
{
    fn new(f: F) -> Self {
        Processor { func: f }
    }
    
    fn process(&self, s: &str) -> R {
        (self.func)(s)
    }
}
```

##### Lifetime Subtyping
---

In the event where a lifetime is ambiguous, you have to declare a relationship between the types and return the shorter of the two. Lifetime `'b` outlives lifetime `'a` and therefore it is at least as useful as `'a`. Put another way, it can be used to fulfill any situation where a `'a` would be required.

```rust
fn longest<'a, 'b>(a: &'a str, b: &'b str) -> &'a str
where
	'b: 'a,
{
	if a.len() > b.len() {
		a
	} else {
		b
	}
}
```

Without the `'b: 'a` constraint, there is a risk that `'b` could be shorter lived than `'a` if returned, leading to a dangling reference. What's nice is that this relationship will be guaranteed *regardless of input order*.

If you pass the longer-lived string reference in the `'a` position, the lifetime will be shortened to the `'b` lifetime because of the trait bound. It is always safe to restrict possibilities, never to expand them. That is why this usage compiles and runs:

```rust
fn main() {
    let a = String::from("a much longer string");
    let result;
    {
        let b = String::from("short");
        result = longest(&a, &b);
        println!("The longest string is: {}", result);
    }
}
```

