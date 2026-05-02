# Ruby — RSpec / Minitest

## Setup

```bash
# RSpec (preferred for Rails / BDD style)
gem install rspec
bundle add rspec --group=test

rspec                              # run all
rspec spec/cart_spec.rb            # single file
rspec spec/cart_spec.rb:42         # single line
rspec --format documentation       # verbose output
rspec --coverage                   # with simplecov

# Minitest (built into Ruby stdlib)
bundle add minitest --group=test
ruby -Ilib:test test/cart_test.rb
rake test
```

```ruby
# .rspec
--require spec_helper
--format documentation
--color
```

## File & Naming Conventions (RSpec)

- Files: `spec/cart_spec.rb`
- Outer: `RSpec.describe Cart do`
- Inner: `describe '#method' do` / `context 'when condition' do`
- Tests: `it 'does something' do`

## Basic Test (RSpec)

```ruby
# spec/cart_spec.rb
require 'spec_helper'
require 'cart'

RSpec.describe Cart do
  subject(:cart) { described_class.new }

  describe '#total' do
    context 'when empty' do
      it 'returns zero' do
        expect(cart.total).to eq(0.0)
      end
    end

    context 'when items are added' do
      before { cart.add(CartItem.new('book', price: 12.99, qty: 2)) }

      it 'returns the sum of all items' do
        expect(cart.total).to be_within(0.001).of(25.98)
      end
    end
  end

  describe '#add' do
    it 'raises ArgumentError for negative qty' do
      expect { cart.add(CartItem.new('x', price: 1.0, qty: -1)) }
        .to raise_error(ArgumentError, /qty must be positive/i)
    end
  end
end
```

## Shared Examples & Contexts

```ruby
RSpec.shared_examples 'a non-empty cart' do
  it { is_expected.to satisfy { |c| c.size > 0 } }
  it { is_expected.to satisfy { |c| c.total > 0 } }
end

RSpec.describe Cart do
  subject(:cart) do
    c = Cart.new
    c.add(CartItem.new('x', price: 5.0, qty: 1))
    c
  end
  it_behaves_like 'a non-empty cart'
end
```

## Mocking (RSpec doubles)

```ruby
RSpec.describe CheckoutService do
  let(:email_service) { instance_double('EmailService') }
  let(:service)       { described_class.new(email_service) }

  describe '#checkout' do
    it 'sends a confirmation email' do
      cart = Cart.new
      cart.add(CartItem.new('x', price: 5.0, qty: 1))

      expect(email_service).to receive(:send).with('a@b.com', 5.0)
      service.checkout(cart, 'a@b.com')
    end

    it 'returns order id from repository' do
      allow(order_repo).to receive(:save).and_return(double(id: 42))
      expect(service.checkout(cart, 'a@b.com')).to eq(42)
    end
  end
end
```

## Parametrized Tests

```ruby
# RSpec — shared example or loop
[
  [1, 10.0, 10.0],
  [2, 10.0, 20.0],
  [0, 10.0,  0.0],
].each do |qty, price, expected|
  it "qty #{qty} × price #{price} = #{expected}" do
    cart.add(CartItem.new('x', price: price, qty: qty))
    expect(cart.total).to be_within(1e-9).of(expected)
  end
end
```

## Minitest Quick Reference

```ruby
# test/cart_test.rb
require 'minitest/autorun'
require 'cart'

class CartTest < Minitest::Test
  def setup
    @cart = Cart.new
  end

  def test_total_is_zero_for_empty_cart
    assert_equal 0.0, @cart.total
  end

  def test_add_item_increases_total
    @cart.add(CartItem.new('book', price: 12.99, qty: 2))
    assert_in_delta 25.98, @cart.total, 0.001
  end

  def test_add_negative_qty_raises
    assert_raises(ArgumentError) { @cart.add(CartItem.new('x', price: 1.0, qty: -1)) }
  end
end
```

## Common RSpec Matchers

```ruby
expect(value).to eq(3)
expect(value).to be > 0
expect(value).to be_nil
expect(value).to be_truthy / be_falsy
expect(string).to include('substring')
expect(array).to contain_exactly('a', 'b')  # order-independent
expect(array).to match_array(['b', 'a'])
expect(hash).to include(key: 'value')
expect(object).to be_a(User)
expect { code }.to change { Model.count }.by(1)
expect { code }.to raise_error(ErrorClass, /message/)
```
