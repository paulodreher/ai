# PHP — PHPUnit

## Setup

```bash
composer require --dev phpunit/phpunit
composer require --dev mockery/mockery          # optional, richer mocks

./vendor/bin/phpunit                            # run all
./vendor/bin/phpunit tests/CartTest.php         # single file
./vendor/bin/phpunit --filter testAddItem       # filter
./vendor/bin/phpunit --coverage-text            # requires Xdebug or pcov
```

```xml
<!-- phpunit.xml -->
<phpunit bootstrap="vendor/autoload.php" colors="true">
  <testsuites>
    <testsuite name="Unit">
      <directory>tests/Unit</directory>
    </testsuite>
  </testsuites>
  <source>
    <include><directory>src</directory></include>
  </source>
</phpunit>
```

## File & Naming Conventions

- Files: `tests/Unit/CartTest.php`
- Class: `class CartTest extends TestCase` (must extend `PHPUnit\Framework\TestCase`)
- Methods: `public function test<BehaviorDescription>(): void` or with `@test` annotation

## Basic Test

```php
<?php

namespace Tests\Unit;

use App\Cart;
use App\CartItem;
use InvalidArgumentException;
use PHPUnit\Framework\TestCase;
use PHPUnit\Framework\Attributes\Test;

class CartTest extends TestCase
{
    private Cart $cart;

    protected function setUp(): void
    {
        $this->cart = new Cart();
    }

    #[Test]
    public function totalIsZeroForEmptyCart(): void
    {
        $this->assertSame(0.0, $this->cart->total());
    }

    #[Test]
    public function addItemIncreasesTotal(): void
    {
        $this->cart->add(new CartItem('book', 12.99, qty: 2));
        $this->assertEqualsWithDelta(25.98, $this->cart->total(), 0.001);
    }

    #[Test]
    public function addNegativeQtyThrowsException(): void
    {
        $this->expectException(InvalidArgumentException::class);
        $this->expectExceptionMessageMatches('/qty must be positive/i');
        $this->cart->add(new CartItem('x', 1.0, qty: -1));
    }
}
```

## Data Providers

```php
use PHPUnit\Framework\Attributes\DataProvider;

public static function totalProvider(): array
{
    return [
        'single item'   => [1, 10.0, 10.0],
        'multiple qty'  => [2, 10.0, 20.0],
        'zero qty'      => [0, 10.0,  0.0],
    ];
}

#[Test]
#[DataProvider('totalProvider')]
public function totalCalculation(int $qty, float $price, float $expected): void
{
    $this->cart->add(new CartItem('x', $price, $qty));
    $this->assertEqualsWithDelta($expected, $this->cart->total(), 0.001);
}
```

## Mocking

```php
// PHPUnit built-in mocks
public function testSendsConfirmationOnCheckout(): void
{
    $email = $this->createMock(EmailServiceInterface::class);
    $email->expects($this->once())
          ->method('send')
          ->with('a@b.com', $this->equalTo(5.0));

    $svc = new CheckoutService($email);
    $this->cart->add(new CartItem('x', 5.0, 1));
    $svc->checkout($this->cart, 'a@b.com');
}

// Mockery (richer API)
use Mockery;

public function testWithMockery(): void
{
    $email = Mockery::mock(EmailServiceInterface::class);
    $email->shouldReceive('send')->once()->with('a@b.com', 5.0);
    // ...
}

protected function tearDown(): void
{
    Mockery::close();   // required when using Mockery
}
```

## Common Assertions

```php
$this->assertSame($expected, $actual);            // strict ===
$this->assertEquals($expected, $actual);          // loose ==
$this->assertEqualsWithDelta($exp, $act, 0.001);  // floats
$this->assertCount(3, $collection);
$this->assertContains($item, $array);
$this->assertArrayHasKey('id', $data);
$this->assertInstanceOf(User::class, $result);
$this->assertNull($value);
$this->assertTrue($condition);
$this->assertMatchesRegularExpression('/pattern/', $string);
```

## Exceptions

```php
// PHPUnit 10+ attribute style
#[Test]
public function invalidPriceThrows(): void
{
    $this->expectException(\DomainException::class);
    $this->expectExceptionCode(422);
    new CartItem('x', -1.0, 1);
}
```
