# TypeScript — Jest / Vitest

## Setup

```bash
# Jest + ts-jest
npm install --save-dev jest ts-jest @types/jest typescript

# Vitest (zero-config with Vite/Bun)
npm install --save-dev vitest @vitest/coverage-v8
```

```json
// jest.config.json
{
  "preset": "ts-jest",
  "testEnvironment": "node",
  "collectCoverageFrom": ["src/**/*.ts", "!src/**/*.d.ts"]
}
```

```ts
// vitest.config.ts
import { defineConfig } from 'vitest/config'
export default defineConfig({ test: { globals: true, coverage: { provider: 'v8' } } })
```

## File & Naming Conventions

- Files: `src/__tests__/cart.test.ts` or `src/cart.test.ts`
- Types: define test-local interfaces inline — do not export from production code just for tests

## Basic Test

```ts
// cart.test.ts
import { Cart, CartItem } from './cart'

describe('Cart', () => {
  let cart: Cart

  beforeEach(() => {
    cart = new Cart()
  })

  it('starts with zero total', () => {
    expect(cart.total()).toBe(0)
  })

  it('adds items and updates total', () => {
    const item: CartItem = { name: 'book', price: 12.99, qty: 2 }
    cart.add(item)
    expect(cart.total()).toBeCloseTo(25.98)
  })

  it('throws TypeError when price is negative', () => {
    expect(() => cart.add({ name: 'x', price: -1, qty: 1 })).toThrow(TypeError)
  })
})
```

## Mocking with Type Safety

```ts
import { jest } from '@jest/globals'
import type { EmailService } from './emailService'

// Typed mock object
const mockEmail = {
  send: jest.fn<EmailService['send']>(),
} satisfies jest.Mocked<EmailService>

test('sends confirmation on checkout', () => {
  const cart = new Cart(mockEmail)
  cart.add({ name: 'x', price: 5, qty: 1 })
  cart.checkout('a@b.com')
  expect(mockEmail.send).toHaveBeenCalledWith<Parameters<EmailService['send']>>('a@b.com', 5)
})

// Module mock
jest.mock('./repository')
import { UserRepository } from './repository'
const MockRepo = jest.mocked(UserRepository)
```

## Async & Error Types

```ts
test('rejects with NotFoundError for unknown id', async () => {
  await expect(fetchUser(-1)).rejects.toThrow(NotFoundError)
})

// Vitest: toSatisfy for type-narrowed assertions
import { expect } from 'vitest'
test('returns User object', async () => {
  const result = await fetchUser(1)
  expect(result).toSatisfy<User>(u => typeof u.id === 'number')
})
```

## Parametrize

```ts
// Jest
test.each([
  [1, 10, 10],
  [2, 10, 20],
  [0, 10, 0],
] as const)('qty %i × price %d = %d', (qty, price, expected) => {
  const cart = new Cart()
  cart.add({ name: 'x', price, qty })
  expect(cart.total()).toBe(expected)
})

// Vitest
import { it } from 'vitest'
it.each([{ qty: 1, price: 10, expected: 10 }])('qty $qty gives $expected', ({ qty, price, expected }) => {
  // ...
})
```

## Common Pitfalls

- Avoid `as any` in test assertions — use proper mocks or type guards
- Use `toStrictEqual` instead of `toEqual` to catch `undefined` vs missing key differences
- `jest.useFakeTimers()` must be restored in `afterEach` or it bleeds into other tests
