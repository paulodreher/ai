---
name: unit-test
description: Write, improve, or review unit tests. Use when the user asks to "write tests", "add unit tests", "test this function", "improve test coverage", "fix failing tests", or discusses testing frameworks, mocks, assertions, or TDD. Covers Python (pytest), JavaScript/TypeScript (Jest, Vitest), Java (JUnit), C# (xUnit/NUnit), C++ (GoogleTest), Go (testing), PHP (PHPUnit), Rust (built-in), and Ruby (RSpec).
version: 1.0.0
---

# Unit Test Skill

Write focused, fast, deterministic unit tests that verify one behavior at a time.

## Core Principles

1. **One assertion per test** (or one behavior) — split multi-concern tests
2. **Arrange / Act / Assert** structure in every test body
3. **Descriptive names** — `test_returns_zero_when_list_is_empty`, not `test_1`
4. **No production side-effects** — mock I/O, clocks, randomness, network, DB
5. **Tests must be independent** — no shared mutable state between tests
6. **Fast** — a unit test suite should run in seconds, not minutes

## What to Test

- Happy path (expected inputs → expected outputs)
- Edge cases (empty, zero, null/None, max values, empty collections)
- Error paths (invalid input raises the right exception/error)
- Boundary conditions (off-by-one, overflow, length limits)

## What NOT to Test

- Language or framework internals
- Third-party library behavior
- Implementation details that can change without breaking behavior

## Mocking Strategy

| Dependency type     | Strategy                            |
|---------------------|-------------------------------------|
| Database            | In-memory DB or repository mock     |
| HTTP / external API | Mock the HTTP client, not the URL   |
| Time / clocks       | Inject a clock or freeze time       |
| File system         | Use temp dirs or VFS                |
| Randomness          | Seed or inject RNG                  |

## Language-Specific Guidance

See the `languages/` subdirectory for framework-specific patterns:

- [Python — pytest](languages/python.md)
- [JavaScript — Jest / Vitest](languages/javascript.md)
- [TypeScript — Jest / Vitest](languages/typescript.md)
- [Java — JUnit 5](languages/java.md)
- [C# — xUnit / NUnit](languages/csharp.md)
- [C++ — GoogleTest / Catch2](languages/cpp.md)
- [Go — testing + testify](languages/go.md)
- [PHP — PHPUnit](languages/php.md)
- [Rust — built-in + rstest](languages/rust.md)
- [Ruby — RSpec / Minitest](languages/ruby.md)

## Process

1. Identify the function / class under test and its contract
2. Read any existing tests to follow conventions already in place
3. Detect the language and framework in use (check deps/config files)
4. Load the relevant `languages/` reference file for patterns
5. Write tests covering happy path, edge cases, and error paths
6. Run the test suite to confirm all pass
7. Report coverage gaps if asked
