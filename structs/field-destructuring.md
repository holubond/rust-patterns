# Field destructuring

Assume we have a basket containing apples and pears (let's say we only need to track the number of those).

```rust
struct Basket {
    apples: u64,
    pears: u64, 
}
```

Sometimes, we want to empty a content of a basket into another basket. Let's make it simple by implementing `std::ops::Add`, so we can later do `let result = basket_a + basket_b;`

**Bad implementation**

```rust
impl std::ops::Add for Basket {
    type Output = Self;
    
    fn add(mut self, rhs: Self) -> Self::Output {
        self.apples += rhs.apples;
        self.pears += rhs.pears;
        self
    }
}
```

Note that rhs is 'emptied' (consumed), as we pass an owned value.

The problem with this implementation is that whenever a new field is added (let's say we want to store cherries in the basket as well), we might forget to add `self.cherries += rhs.cherries;`, which can cause bugs that are nasty to find (empty basket + basket of cherries results in an empty basket).

**Good implementation**

We can use destructuring to combat this issue. Whenever a field is added to the `Basket` struct, the code won't compile until we manually add cherries to the destructuring, by which we are likely to realise they should be added (+=) as well.


```rust
impl std::ops::Add for Basket {
    type Output = Self;

    fn add(mut self, rhs: Self) -> Self::Output {
        let Self { apples, pears } = rhs;
        
        self.apples += apples;
        self.pears += pears;
        self
    }
}
```