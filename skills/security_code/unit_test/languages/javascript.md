# JavaScript — Jest / Vitest

## Setup

```bash
# Jest
npm install --save-dev jest @jest/globals

# Vitest (preferred for Vite projects)
npm install --save-dev vitest
```

```json
// package.json
{
  "scripts": {
    "test": "jest --coverage",
    "test:watch": "jest --watch"
  }
}
```

```js
// vitest.config.js
import { defineConfig } from 'vitest/config'
export default defineConfig({ test: { coverage: { reporter: ['text', 'lcov'] } } })
```

## File & Naming Conventions

- Files: `src/__tests__/cart.test.js` or `src/cart.test.js`
- Describe blocks: feature or class name
- Test names: verb phrase describing behavior

## Basic Test

```js
// cart.test.js
import { Cart } from './cart.js'

describe('Cart', () => {
  test('total is zero for empty cart', () => {
    const cart = new Cart()
    expect(cart.total()).toBe(0)
  })

  test('add item increases total', () => {
    const cart = new Cart()
    cart.add({ name: 'apple', price: 1.5, qty: 2 })
    expect(cart.total()).toBe(3.0)
  })

  test('throws on negative qty', () => {
    const cart = new Cart()
    expect(() => cart.add({ name: 'x', price: 1, qty: -1 })).toThrow('qty must be positive')
  })
})
```

## Mocking

```js
// Mock a module
jest.mock('./emailService')
import { sendEmail } from './emailService'

test('sends confirmation on checkout', () => {
  const cart = new Cart()
  cart.add({ name: 'x', price: 5, qty: 1 })
  cart.checkout('a@b.com')
  expect(sendEmail).toHaveBeenCalledWith('a@b.com', { total: 5 })
})

// Mock a function
const mockFn = jest.fn().mockReturnValue(42)
expect(mockFn()).toBe(42)
expect(mockFn).toHaveBeenCalledTimes(1)

// Spy
const spy = jest.spyOn(console, 'error').mockImplementation(() => {})
// ... test ...
spy.mockRestore()
```

## Async Tests

```js
// Promise
test('fetches user', async () => {
  const user = await fetchUser(1)
  expect(user.id).toBe(1)
})

// Timer mocks
jest.useFakeTimers()
test('debounce fires after delay', () => {
  const fn = jest.fn()
  debounce(fn, 300)()
  jest.advanceTimersByTime(300)
  expect(fn).toHaveBeenCalledTimes(1)
})
jest.useRealTimers()
```

## Setup / Teardown

```js
beforeEach(() => { /* reset state */ })
afterEach(() => { jest.clearAllMocks() })
beforeAll(() => { /* one-time setup */ })
afterAll(() => { /* one-time teardown */ })
```

## Common Matchers

| Matcher                        | Use case                       |
|--------------------------------|--------------------------------|
| `toBe(val)`                    | Strict equality (`===`)        |
| `toEqual(val)`                 | Deep equality                  |
| `toBeNull()` / `toBeUndefined()` | Null / undefined checks      |
| `toContain(item)`              | Array/string contains          |
| `toMatchObject(obj)`           | Partial object match           |
| `toThrow('message')`           | Exception thrown               |
| `toHaveBeenCalledWith(...args)`| Mock call arguments            |
