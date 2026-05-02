# Java — JUnit 5

## Setup (Maven)

```xml
<!-- pom.xml -->
<dependencies>
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.11.0</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>5.12.0</version>
    <scope>test</scope>
  </dependency>
  <dependency>
    <groupId>org.assertj</groupId>
    <artifactId>assertj-core</artifactId>
    <version>3.26.3</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

```bash
mvn test                          # run all tests
mvn test -Dtest=CartTest          # single class
mvn test -Dtest=CartTest#totalIsZero  # single method
```

## File & Naming Conventions

- Files: `src/test/java/com/example/CartTest.java`
- Class: `<ProductionClass>Test`
- Methods: `@Test void <behaviorDescription>()`

## Basic Test

```java
import org.junit.jupiter.api.*;
import static org.assertj.core.api.Assertions.*;

class CartTest {

    private Cart cart;

    @BeforeEach
    void setUp() {
        cart = new Cart();
    }

    @Test
    void totalIsZeroForEmptyCart() {
        assertThat(cart.total()).isEqualByComparingTo("0.00");
    }

    @Test
    void addItemIncreasesTotal() {
        cart.add(new Item("book", new BigDecimal("12.99"), 2));
        assertThat(cart.total()).isEqualByComparingTo("25.98");
    }

    @Test
    void addNegativeQtyThrowsIllegalArgument() {
        assertThatThrownBy(() -> cart.add(new Item("x", BigDecimal.ONE, -1)))
            .isInstanceOf(IllegalArgumentException.class)
            .hasMessageContaining("qty must be positive");
    }
}
```

## Parametrized Tests

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.*;

@ParameterizedTest
@CsvSource({
    "1, 10.00, 10.00",
    "2, 10.00, 20.00",
    "0, 10.00,  0.00",
})
void totalCalculation(int qty, String price, String expected) {
    cart.add(new Item("x", new BigDecimal(price), qty));
    assertThat(cart.total()).isEqualByComparingTo(expected);
}
```

## Mocking with Mockito

```java
import org.mockito.*;
import org.mockito.junit.jupiter.MockitoExtension;
import org.junit.jupiter.api.extension.ExtendWith;

@ExtendWith(MockitoExtension.class)
class CheckoutServiceTest {

    @Mock EmailService emailService;
    @InjectMocks CheckoutService checkoutService;

    @Test
    void sendsConfirmationOnCheckout() {
        Cart cart = new Cart();
        cart.add(new Item("x", new BigDecimal("5.00"), 1));

        checkoutService.checkout(cart, "a@b.com");

        verify(emailService).send("a@b.com", new BigDecimal("5.00"));
    }

    @Test
    void returnsOrderIdFromRepository() {
        when(orderRepository.save(any())).thenReturn(new Order(42L));
        long id = checkoutService.checkout(cart, "a@b.com");
        assertThat(id).isEqualTo(42L);
    }
}
```

## Assertions (AssertJ — prefer over JUnit built-ins)

```java
assertThat(list).hasSize(3).contains("a").doesNotContain("z");
assertThat(map).containsKey("id").containsEntry("name", "Alice");
assertThat(optional).isPresent().hasValue("x");
assertThat(result).usingRecursiveComparison().isEqualTo(expected);
```

## Common Annotations

| Annotation           | Purpose                                  |
|----------------------|------------------------------------------|
| `@Test`              | Mark test method                         |
| `@BeforeEach`        | Run before each test                     |
| `@AfterEach`         | Run after each test                      |
| `@BeforeAll`         | Run once before all (must be `static`)   |
| `@Nested`            | Group related tests in inner class       |
| `@DisplayName`       | Human-readable test name                 |
| `@Disabled`          | Skip test with explanation               |
| `@Tag("slow")`       | Filter test execution                    |
