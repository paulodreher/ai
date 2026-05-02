# Go — testing package + testify

## Setup

```bash
go get github.com/stretchr/testify

go test ./...                        # run all packages
go test ./internal/cart/...          # specific package
go test -run TestCart -v             # filter by name
go test -cover -coverprofile=cov.out ./...
go tool cover -html=cov.out
go test -race ./...                  # race detector
```

## File & Naming Conventions

- Files: `cart_test.go` (same package or `package cart_test` for black-box tests)
- Functions: `func Test<Name>(t *testing.T)`
- Use `_test.go` suffix — the build tool excludes them from production binaries

## Basic Test

```go
// cart_test.go
package cart_test

import (
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
    "myapp/internal/cart"
)

func TestCart_TotalIsZeroForEmptyCart(t *testing.T) {
    c := cart.New()
    assert.Equal(t, 0.0, c.Total())
}

func TestCart_AddItemIncreasesTotal(t *testing.T) {
    c := cart.New()
    err := c.Add(cart.Item{Name: "book", Price: 12.99, Qty: 2})
    require.NoError(t, err)
    assert.InDelta(t, 25.98, c.Total(), 1e-9)
}

func TestCart_NegativeQtyReturnsError(t *testing.T) {
    c := cart.New()
    err := c.Add(cart.Item{Name: "x", Price: 1.0, Qty: -1})
    require.Error(t, err)
    assert.Contains(t, err.Error(), "qty must be positive")
}
```

## Table-Driven Tests (idiomatic Go)

```go
func TestCart_Total(t *testing.T) {
    tests := []struct {
        name     string
        qty      int
        price    float64
        expected float64
    }{
        {"single item",   1, 10.0, 10.0},
        {"multiple qty",  2, 10.0, 20.0},
        {"zero qty",      0, 10.0,  0.0},
    }

    for _, tc := range tests {
        t.Run(tc.name, func(t *testing.T) {
            c := cart.New()
            _ = c.Add(cart.Item{Name: "x", Price: tc.price, Qty: tc.qty})
            assert.InDelta(t, tc.expected, c.Total(), 1e-9)
        })
    }
}
```

## Mocking

```go
// Define interface in production code; implement mock in test file
type EmailService interface {
    Send(to string, total float64) error
}

type mockEmail struct {
    called bool
    to     string
    total  float64
}
func (m *mockEmail) Send(to string, total float64) error {
    m.called, m.to, m.total = true, to, total
    return nil
}

func TestCheckout_SendsConfirmation(t *testing.T) {
    email := &mockEmail{}
    svc := checkout.NewService(email)

    c := cart.New()
    _ = c.Add(cart.Item{Name: "x", Price: 5.0, Qty: 1})
    err := svc.Checkout(c, "a@b.com")

    require.NoError(t, err)
    assert.True(t, email.called)
    assert.Equal(t, "a@b.com", email.to)
    assert.InDelta(t, 5.0, email.total, 1e-9)
}

// Alternatively use testify/mock for larger interfaces
```

## Subtests & Helpers

```go
func TestCheckout(t *testing.T) {
    t.Run("empty cart returns error", func(t *testing.T) { ... })
    t.Run("sends email", func(t *testing.T) { ... })
}

// Helper — call t.Helper() to get the right line in failures
func newCartWithItem(t *testing.T, price float64, qty int) *cart.Cart {
    t.Helper()
    c := cart.New()
    require.NoError(t, c.Add(cart.Item{Name: "x", Price: price, Qty: qty}))
    return c
}
```

## Testify Cheat Sheet

```go
assert.Equal(t, expected, actual)          // continue on failure
require.Equal(t, expected, actual)         // stop test on failure
assert.NoError(t, err)
assert.ErrorIs(t, err, ErrNotFound)
assert.Len(t, slice, 3)
assert.Contains(t, slice, item)
assert.ElementsMatch(t, a, b)             // order-independent
assert.Eventually(t, cond, timeout, tick) // for async
```

## Benchmarks & Fuzz (built-in)

```go
func BenchmarkCart_Total(b *testing.B) {
    c := cart.New()
    for range b.N {
        c.Add(cart.Item{Name: "x", Price: 1.0, Qty: 1})
        c.Total()
    }
}

func FuzzCartAdd(f *testing.F) {
    f.Add("name", 1.0, 1)
    f.Fuzz(func(t *testing.T, name string, price float64, qty int) {
        c := cart.New()
        c.Add(cart.Item{Name: name, Price: price, Qty: qty})
    })
}
```
