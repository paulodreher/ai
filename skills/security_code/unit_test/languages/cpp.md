# C++ — GoogleTest / Catch2

## Setup

### GoogleTest (CMake)

```cmake
# CMakeLists.txt
include(FetchContent)
FetchContent_Declare(googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.15.2.tar.gz)
FetchContent_MakeAvailable(googletest)

enable_testing()
add_executable(cart_test tests/cart_test.cpp)
target_link_libraries(cart_test GTest::gtest_main cart_lib)
include(GoogleTest)
gtest_discover_tests(cart_test)
```

```bash
cmake -B build && cmake --build build
ctest --test-dir build --output-on-failure
./build/cart_test --gtest_filter="CartTest.*"
```

### Catch2 (alternative)

```cmake
FetchContent_Declare(Catch2
  GIT_REPOSITORY https://github.com/catchorg/Catch2.git
  GIT_TAG v3.6.0)
FetchContent_MakeAvailable(Catch2)
target_link_libraries(cart_test Catch2::Catch2WithMain)
```

## File & Naming Conventions

- Files: `tests/cart_test.cpp`
- Test suite: `TEST(SuiteName, TestName)` — no spaces, use `_` or CamelCase
- Fixture class: `class CartTest : public ::testing::Test`

## Basic Test (GoogleTest)

```cpp
#include <gtest/gtest.h>
#include "cart.h"

TEST(CartTest, TotalIsZeroForEmptyCart) {
    Cart cart;
    EXPECT_DOUBLE_EQ(cart.total(), 0.0);
}

TEST(CartTest, AddItemIncreasesTotal) {
    Cart cart;
    cart.add({"book", 12.99, 2});
    EXPECT_NEAR(cart.total(), 25.98, 1e-9);
}

TEST(CartTest, NegativeQtyThrows) {
    Cart cart;
    EXPECT_THROW(cart.add({"x", 1.0, -1}), std::invalid_argument);
}
```

## Fixtures

```cpp
class CartTest : public ::testing::Test {
protected:
    Cart cart;

    void SetUp() override {
        cart = Cart{};
    }

    // TearDown() override { }
};

TEST_F(CartTest, StartsEmpty) {
    EXPECT_EQ(cart.size(), 0);
}

TEST_F(CartTest, CheckoutRequiresItems) {
    EXPECT_THROW(cart.checkout(), std::logic_error);
}
```

## Parametrized Tests

```cpp
struct TotalParam { int qty; double price; double expected; };

class CartTotalTest : public ::testing::TestWithParam<TotalParam> {};

INSTANTIATE_TEST_SUITE_P(TotalCases, CartTotalTest, ::testing::Values(
    TotalParam{1, 10.0, 10.0},
    TotalParam{2, 10.0, 20.0},
    TotalParam{0, 10.0,  0.0}
));

TEST_P(CartTotalTest, ComputesCorrectly) {
    auto [qty, price, expected] = GetParam();
    Cart cart;
    cart.add({"x", price, qty});
    EXPECT_NEAR(cart.total(), expected, 1e-9);
}
```

## Mocking with GoogleMock

```cpp
#include <gmock/gmock.h>

class MockEmailService : public IEmailService {
public:
    MOCK_METHOD(void, send, (const std::string& to, double total), (override));
};

TEST(CheckoutTest, SendsConfirmationEmail) {
    MockEmailService email;
    EXPECT_CALL(email, send("a@b.com", 5.0)).Times(1);

    Cart cart;
    cart.add({"x", 5.0, 1});
    CheckoutService svc{&email};
    svc.checkout(cart, "a@b.com");
}
```

## Key Assertion Macros

| Macro                        | Behavior on failure      |
|------------------------------|--------------------------|
| `EXPECT_EQ(a, b)`            | Continue test            |
| `ASSERT_EQ(a, b)`            | Abort test immediately   |
| `EXPECT_NEAR(a, b, eps)`     | Float comparison         |
| `EXPECT_STREQ(s1, s2)`       | C-string equality        |
| `EXPECT_THROW(stmt, ExcType)`| Exception thrown         |
| `EXPECT_NO_THROW(stmt)`      | No exception             |
| `EXPECT_THAT(val, matcher)`  | GMock matcher            |

## Catch2 Quick Reference

```cpp
#include <catch2/catch_test_macros.hpp>
#include <catch2/generators/catch_generators.hpp>

TEST_CASE("Cart total", "[cart]") {
    Cart cart;

    SECTION("empty cart") {
        REQUIRE(cart.total() == 0.0);
    }

    SECTION("after adding item") {
        cart.add({"book", 12.99, 2});
        REQUIRE_THAT(cart.total(), Catch::Matchers::WithinRel(25.98, 1e-6));
    }
}

TEST_CASE("Parametrized total", "[cart]") {
    auto [qty, price, expected] = GENERATE(table<int,double,double>({
        {1, 10.0, 10.0},
        {2, 10.0, 20.0},
    }));
    Cart cart;
    cart.add({"x", price, qty});
    CHECK(cart.total() == expected);
}
```
