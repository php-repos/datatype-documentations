# Introduction

The `Pair` class is part of the Datatype package.
You can use this class to make a pair data of your desired values.

Here you can see its API and see how to use it.

## Usage

You can make a new instance of this class and use it as an array or use the available methods on the class:

```php
$pair = new Pair(1, 'foo');
assert_true(['key' => 1, 'value' => 'foo'] === $pair->get());
```

## Methods

Here you can see a list of the available methods on the collection:

---
### get

It returns an array of key and value 
where the key index contains the key of the pair and the value index contains the value of the pair.

```php
public function get(): array
```

#### Example

```php
$pair = new Pair(1, 'foo');
assert_true(['key' => 1, 'value' => 'foo'] === $pair->get());
```
---
### value

It returns a new pair object by setting the value of the pair to the given value.

```php
public function value(mixed $value): static
```

#### Example

```php
$pair = new Pair(1, 'foo');
$new_pair = $pair->value('bar');

assert_true($pair->value === 'foo');
assert_true(1 === $new_pair->key);
assert_true('bar' === $new_pair->value);
```
---
