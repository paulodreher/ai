# C# — xUnit / NUnit / MSTest

## Setup (xUnit — recommended for new projects)

```bash
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
dotnet add package Moq
dotnet add package FluentAssertions

dotnet test                          # run all
dotnet test --filter "FullyQualifiedName~CartTests"
dotnet test --collect:"XPlat Code Coverage"
```

## File & Naming Conventions

- Files: `tests/CartTests.cs` (project: `MyApp.Tests`)
- Class: `<ProductionClass>Tests`
- Methods: `<Method>_<Scenario>_<ExpectedResult>` or plain descriptive name

## Basic Test (xUnit)

```csharp
using Xunit;
using FluentAssertions;

public class CartTests
{
    private readonly Cart _cart;

    public CartTests()         // xUnit creates a new instance per test
    {
        _cart = new Cart();
    }

    [Fact]
    public void Total_EmptyCart_ReturnsZero()
    {
        _cart.Total().Should().Be(0m);
    }

    [Fact]
    public void Add_ValidItem_IncreasesTotal()
    {
        _cart.Add(new CartItem("book", 12.99m, qty: 2));
        _cart.Total().Should().BeApproximately(25.98m, precision: 0.01m);
    }

    [Fact]
    public void Add_NegativeQty_ThrowsArgumentException()
    {
        Action act = () => _cart.Add(new CartItem("x", 1m, qty: -1));
        act.Should().Throw<ArgumentException>().WithMessage("*qty*");
    }
}
```

## Parametrized Tests

```csharp
// xUnit — Theory + InlineData
[Theory]
[InlineData(1, 10.00, 10.00)]
[InlineData(2, 10.00, 20.00)]
[InlineData(0, 10.00,  0.00)]
public void Total_VariesByQty(int qty, decimal price, decimal expected)
{
    _cart.Add(new CartItem("x", price, qty));
    _cart.Total().Should().Be(expected);
}

// MemberData for complex objects
public static IEnumerable<object[]> TotalData =>
[
    [new CartItem("a", 5m, 2), 10m],
    [new CartItem("b", 3m, 3),  9m],
];

[Theory, MemberData(nameof(TotalData))]
public void Total_MemberData(CartItem item, decimal expected)
{
    _cart.Add(item);
    _cart.Total().Should().Be(expected);
}
```

## Mocking with Moq

```csharp
using Moq;

public class CheckoutServiceTests
{
    private readonly Mock<IEmailService> _emailMock = new();
    private readonly CheckoutService _svc;

    public CheckoutServiceTests()
    {
        _svc = new CheckoutService(_emailMock.Object);
    }

    [Fact]
    public void Checkout_SendsConfirmationEmail()
    {
        var cart = new Cart();
        cart.Add(new CartItem("x", 5m, 1));

        _svc.Checkout(cart, "a@b.com");

        _emailMock.Verify(e => e.Send("a@b.com", 5m), Times.Once);
    }

    [Fact]
    public void Checkout_RepositoryReturnsId()
    {
        _repoMock.Setup(r => r.Save(It.IsAny<Order>())).Returns(new Order { Id = 42 });
        var id = _svc.Checkout(new Cart(), "a@b.com");
        id.Should().Be(42);
    }
}
```

## Async Tests

```csharp
[Fact]
public async Task FetchUser_ValidId_ReturnsUser()
{
    var user = await _service.FetchUserAsync(1);
    user.Should().NotBeNull();
    user!.Id.Should().Be(1);
}

[Fact]
public async Task FetchUser_InvalidId_ThrowsNotFoundException()
{
    Func<Task> act = () => _service.FetchUserAsync(-1);
    await act.Should().ThrowAsync<NotFoundException>();
}
```

## FluentAssertions Highlights

```csharp
collection.Should().HaveCount(3).And.Contain("a");
result.Should().BeEquivalentTo(expected);        // deep structural compare
dto.Should().BeEquivalentTo(entity, opt => opt.ExcludingMissingMembers());
action.Should().NotThrow();
```

## NUnit Quick Reference

```csharp
[TestFixture]
public class CartTests
{
    [SetUp] public void SetUp() { ... }
    [TearDown] public void TearDown() { ... }

    [Test] public void Total_IsZero() { Assert.That(_cart.Total(), Is.Zero); }

    [TestCase(1, 10.0, 10.0)]
    [TestCase(2, 10.0, 20.0)]
    public void Total_Parametrized(int qty, double price, double expected)
    {
        Assert.That(_cart.Total(), Is.EqualTo(expected).Within(0.01));
    }
}
```
