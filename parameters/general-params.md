Instead of requiring a specific implementation of function parameters, it is good practice to only ask for what we need.

Assume we implement a method of a `Cart`, where we want to store ID of each product. Instead of this

```rust
fn add_products(&mut self, products: &Vec<Product>) {
    ...
}
```

we might want to accept any collection (a HashMap, for example), not necessarily a vector. A better solution looks like this:

```rust
fn add_products(&mut self, products: &[Product]) {
    ...
}
```