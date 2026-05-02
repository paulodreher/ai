# Rust — Built-in test framework + rstest

## Setup

```toml
# Cargo.toml
[dev-dependencies]
rstest = "0.23"          # parametrized tests
mockall = "0.13"         # mock generation
```

```bash
cargo test                          # run all tests
cargo test cart                     # filter by name substring
cargo test -- --nocapture           # show println! output
cargo test -- --test-threads=1      # run sequentially
cargo llvm-cov --html               # coverage (cargo-llvm-cov)
```

## File & Naming Conventions

- Unit tests: in the same file as the code under test, inside `#[cfg(test)] mod tests`
- Integration tests: `tests/cart_test.rs` (separate crate, no `mod tests` wrapper needed)
- Test functions: `fn test_<behavior>()`

## Basic Test

```rust
// src/cart.rs

pub struct Cart { items: Vec<Item> }

impl Cart {
    pub fn new() -> Self { Cart { items: vec![] } }
    pub fn add(&mut self, item: Item) -> Result<(), String> { ... }
    pub fn total(&self) -> f64 { ... }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn total_is_zero_for_empty_cart() {
        let cart = Cart::new();
        assert_eq!(cart.total(), 0.0);
    }

    #[test]
    fn add_item_increases_total() {
        let mut cart = Cart::new();
        cart.add(Item { name: "book".into(), price: 12.99, qty: 2 }).unwrap();
        assert!((cart.total() - 25.98).abs() < f64::EPSILON * 100.0);
    }

    #[test]
    fn add_negative_qty_returns_error() {
        let mut cart = Cart::new();
        let result = cart.add(Item { name: "x".into(), price: 1.0, qty: -1 });
        assert!(result.is_err());
        assert!(result.unwrap_err().contains("qty must be positive"));
    }

    #[test]
    #[should_panic(expected = "overflow")]
    fn panics_on_overflow() {
        let mut cart = Cart::new();
        cart.add_unchecked(Item { name: "x".into(), price: f64::MAX, qty: i32::MAX });
    }
}
```

## Parametrized Tests with rstest

```rust
use rstest::rstest;

#[rstest]
#[case(1, 10.0, 10.0)]
#[case(2, 10.0, 20.0)]
#[case(0, 10.0,  0.0)]
fn total_varies_by_qty(#[case] qty: i32, #[case] price: f64, #[case] expected: f64) {
    let mut cart = Cart::new();
    cart.add(Item { name: "x".into(), price, qty }).unwrap();
    assert!((cart.total() - expected).abs() < 1e-9);
}

// Fixture
#[rstest]
fn checkout_sends_email(#[fixture] cart_with_item: Cart) {
    // ...
}

#[fixture]
fn cart_with_item() -> Cart {
    let mut cart = Cart::new();
    cart.add(Item { name: "x".into(), price: 5.0, qty: 1 }).unwrap();
    cart
}
```

## Mocking with mockall

```rust
use mockall::{automock, predicate::*};

#[automock]
pub trait EmailService {
    fn send(&self, to: &str, total: f64) -> Result<(), String>;
}

#[cfg(test)]
mod tests {
    use super::*;
    use mockall::predicate::eq;

    #[test]
    fn checkout_sends_confirmation() {
        let mut mock = MockEmailService::new();
        mock.expect_send()
            .with(eq("a@b.com"), float_is_close(5.0))
            .times(1)
            .returning(|_, _| Ok(()));

        let mut cart = Cart::new();
        cart.add(Item { name: "x".into(), price: 5.0, qty: 1 }).unwrap();
        let svc = CheckoutService::new(mock);
        svc.checkout(&cart, "a@b.com").unwrap();
    }
}
```

## Async Tests

```rust
// tokio
#[tokio::test]
async fn fetch_user_returns_user() {
    let user = fetch_user(1).await.unwrap();
    assert_eq!(user.id, 1);
}

// async-std
#[async_std::test]
async fn fetch_user_invalid_id_errors() {
    let result = fetch_user(-1).await;
    assert!(result.is_err());
}
```

## Assertion Patterns

```rust
assert_eq!(a, b);                          // equality (implements Debug)
assert_ne!(a, b);                          // inequality
assert!(condition, "message {}", value);   // custom message
assert!((f - expected).abs() < 1e-9);     // float comparison

// With pretty-diff (use pretty_assertions crate)
use pretty_assertions::assert_eq;
assert_eq!(large_struct_a, large_struct_b);
```

## Test Organization

```rust
// Group related tests with nested modules
#[cfg(test)]
mod tests {
    mod add {
        use super::super::*;
        #[test] fn positive_qty() { ... }
        #[test] fn zero_qty() { ... }
    }
    mod checkout {
        use super::super::*;
        #[test] fn empty_cart_fails() { ... }
    }
}
```
