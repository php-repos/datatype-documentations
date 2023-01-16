# Introduction

The `Collection` class in the `Datatype` package provides a set of convenient methods for working with arrays.
It implements `ArrayAccess`, `IteratorAggregate`, and `Countable` interfaces, which allows you to use it like an array.
Here you can see its API and see how to use it.

## Usage

You can make a new instance of this class and use it as an array or use the available methods on the class:

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);

// Use as array
foreach ($collection as $number => $item) {
    echo $number . ' ' . $item . ','; // Output: 1 John,2 Jane
}

echo count($collection); // Output: 2

// Use items() method on the object
foreach ($collection->items() as $number => $item) {
    echo $number . ' ' . $item . ','; // Output: 1 John,2 Jane
}

echo count($collection->items()); // Output: 2
```

## Methods

Here you can see a list of the available methods on the collection:

### count

This method returns the number of items in the collection.

```php
public function count(): int
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
assert_true(2 === $collection->count());
```

### each

The `each` method can get used to run a closure against each item of the collection.
The `each` method always passes the value as the first argument and the key as the second argument to the given closure.

```php
public function each(Closure $closure): static
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
$collection->each(function ($value, $key) {
    echo $key . ' ' . $value . PHP_EOL;
});
```

```shell
// Output:
1 John
2 Jane
```

## every

### Signature

```php
public function every(Closure $check = null): bool
```

### Definition

This method checks whether all items in the collection pass the provided check closure.
If no closure is provided, it returns true if all items are truthy.

### Examples

```php
echo (int) new Collection(['foo', 'bar', 'baz'])->every(fn ($item) => is_string($item)); // Output: 1
echo (int) new Collection(['foo', 'bar', 'baz'])->every(fn ($item, $key) => is_numeric($key)); // Output: 1
echo (int) new Collection(['foo', 'bar', 'baz'])->every(fn ($item, $key) => strlen($item) > 3); // Output: 0
echo (int) new Collection([1, 2, 3])->every(); // Output: 1
echo (int) new Collection(['foo', 'bar', 'baz'])->every(); // Output: 1
echo (int) new Collection([null, 0, '', []])->every(); // Output: 0
```
---
### except

This method returns a new collection containing all items that don't pass the provided check closure.
If no closure is provided, it returns all items that are falsy.

```php
public function except(Closure $check = null): static
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
$result = $collection->except(function ($value, $key) {
    return $value === 'John';
});
var_dump($result);
```

```shell
// Output:
array(1) {
  [2]=>
  string(4) "Jane"
}
```

### filter

This method returns a new collection containing all items that pass the provided filter closure.
If no closure is provided, it returns all items that are truthy.

```php
public function filter(Closure $closure = null): static
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
$result = $collection->filter(function ($value, $key) {
    return $value === 'John';
});
var_dump($result);
```

```shell
// Output:
array(1) {
  [1]=>
  string(4) "John"
}
```

## first_key

### Signature

```php
public function first_key(Closure $condition = null): string|int|null
```

### Definition

This method returns the key of the first item in the collection that passes the provided condition closure.
If no closure is provided, it returns the first key of the collection.
If the collection is empty, it returns `null`.

### Examples

```php
echo new Collection(['foo' => 1, 'bar' => 2, 'baz' => 2])->first_key(fn ($item, $key) => $item === 2); // Output: 'bar'
echo new Collection(['foo', 'baz'])->first_key(); // Output: 0
assert_true(null === new Collection([null => 'foo', 'foo' => 'bar'])->first_key());
echo new Collection([1 => 'bar', 'foo' => 'baz'])->first_key(); // Output: 1
echo new Collection('foo' => ['bar'], 'bar' => 'baz')->first_key(); // Output: 'foo'
assert_true(null === new Collection([])->first_key());
```

## first

### Signature

```php
public function first(Closure $condition = null): mixed
```

### Definition

This method returns the first item in the collection that passes the provided condition closure.
If no closure is provided, it returns the first item of the collection.
If the collection is empty, it returns `null`.

### Examples

```php
assert_true('bar' === new Collection(['foo', 'bar', 'baz'])->first(fn ($item, $key) => Str\first_character($item) === 'b'));
assert_true('foo' === new Collection(['foo', 'baz'])->first());
assert_true(null === new Collection([null, 'foo'])->first());
assert_true(1 === new Collection([1, 'foo'])->first());
assert_true(['bar'] === new Collection(['bar'], 'foo')->first());
assert_true(null === new Collection([])->first());
```

### forget

This method removes all items from the collection that pass the provided condition closure, and returns the collection object.
It does nothing when the check returns null for items.

```php
public function forget(Closure $condition): static
```

#### Example

```php
$collection = new Collection([1 => 'foo', 2 => 'bar', 3 => 'baz']);
$collection->forget(fn ($item, $key) => $item === 'foo' || $key === 2);
assert_true([3 => 'baz'] === $collection->items());

$collection = new Collection([1 => 'foo', 2 => 'bar']);
$collection->forget(fn ($item, $key) => $item === 'hello worlds');
assert_true([1 => 'foo', 2 => 'bar'] === $collection->items());

```

```shell
// Output:
array(1) {
  [2]=>
  string(4) "Jane"
}
```

## has

### Signature

```php
public function has(Closure $closure): bool
```

### Definition

This method checks whether the collection contains any items that pass the provided closure.
It returns `true` if at least one item passes the closure, and `false` otherwise.

### Examples

```php
assert_true(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $item === 'qux'));
assert_true(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $key === 'baz'));
assert_false(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $key === 0));
assert_false(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $item === null));
```

## items

### Signature

```php
public function items(): array
```

### Definition

This method returns the underlying array of the collection.

### Examples

```php
$collection = new Collection(['foo' => 'bar', 'baz' => 'qux'];
assert_true(['foo' => 'bar', 'baz' => 'qux'] === $collection->items());
```

### keys

This method returns the keys of the underlying array of the collection.

```php
public function keys(): array
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
var_dump($collection->keys());
```

```shell
// Output:
array(2) {
  [0]=>
  int(1)
  [1]=>
  int(2)
}
```

## last_key

### Signature

```php
public function last_key(Closure $condition = null): string|int|null
```

### Definition

This method returns the key of the last item in the collection that passes the provided condition closure.
If no closure is provided, it returns the last key of the collection.
If the collection is empty, it returns `null`.

### Examples

```php
assert_true('baz' === new Collection(['foo' => 1, 'bar' => 2, 'baz' => 2])->last_key(fn ($item, $key) => $item === 2));
assert_true(null === new Collection([])->last_key());
assert_true(1 === new Collection(['foo', 'baz'])->last_key());
assert_true('' === new Collection(['foo' => 'bar', null => 'foo'])->last_key());
assert_true(1 === new Collection(['foo' => 'baz', 1 => 'bar'])->last_key());
assert_true('foo' === new Collection(['bar' => 'baz', 'foo' => ['bar']])->last_key());
```

## last

### Signature

```php
public function last(Closure $condition = null): mixed
```

### Definition

This method returns the last item in the collection that passes the provided condition closure.
If no closure is provided, it returns the last item of the collection.
If the collection is empty, it returns `null`.

### Examples

```php
assert_true('baz' === new Collection(['foo', 'bar', 'baz'])->last(fn ($item, $key) => first_character($item) === 'b'));
assert_true('baz' === new Collection(['foo', 'baz'])->last());
assert_true(null === new Collection(['foo', null])->last());
assert_true(1 === new Collection(['foo', 1])->last());
assert_true(['bar'] === new Collection(['foo', ['bar']])->last());
assert_true(null === new Collection([])->last())
```

## map

### Signature

```php
public function map(Closure $closure): array
```

### Definition

This method applies the provided closure to each item in the collection and returns an array of the results.

### Examples

```php
$collection = new Collection(['foo', 'bar', 'baz']);

assert_true(['0foo', '1bar', '2baz'] === $collection->map(fn ($item, $key) => $key.$item));
```

### push

This method adds the provided value to the end of the collection.

```php
public function push(mixed $value): static
```

#### Example

```php
$collection = new Collection([1 => 'foo', 2 => 'bar']);
$collection->push('baz');

assert_true([1 => 'foo', 2 => 'bar', 'baz'] === $collection->items());
```

### put

This method adds the provided value to the collection at the provided key.
If no key is provided, the value is added to the array by natural key.

```php
public function put(mixed $value, int|string|null $key = null): static
```

#### Example

```php
$collection = new Collection([1 => 'foo', 2 => 'bar']);
$collection->put('baz', 3);
assert_true([1 => 'foo', 2 => 'bar', 3 => 'baz'] === $collection->items());

$collection = new Collection([1 => 'foo', 2 => 'bar']);
$collection->put('baz');

assert_true([1 => 'foo', 2 => 'bar', 'baz'] === $collection->items());
```

### reduce

This method applies the provided closure to each item in the collection and returns a single value.
The closure takes 3 arguments: the carry value, the current item, and the current key.
It works just like the `array_reduce` function. If no carry passed, the initial carry sets as `null`.

```php
public function reduce(Closure $closure, mixed $carry = null): mixed
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
$result = $collection->reduce(function ($carry, $value) {
    return $value === 'Jane' || $carry;
}, false);

var_dump($result); // Output: bool(true)
```

### take

This method returns the first item in the collection that passes the provided condition closure and removes it from the collection.
If no item passes the condition, it returns `null`.

```php
public function take(Closure $condition): mixed
```

#### Example

```php
$collection = new Collection(['foo', 'bar', 'baz']);
$result = $collection->take(fn ($item, $key) => $item === 'bar');
assert_true('bar' === $result);
assert_true([0 => 'foo', 2 => 'baz'] === $collection->items());

$collection = new Collection(['foo', 'bar', 'baz']);
$result = $collection->take(fn ($item, $key) => $item === 'qux');
assert_true(null === $result);
assert_true([0 => 'foo', 1 => 'bar', 2 => 'baz'] === $collection->items());
```

### values

This method returns an array of the values in the collection.

```php
public function values(): array
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
var_dump($collection->values());
```

```shell
// Output:
array(2) {
  [0]=>
  string(4) "John"
  [1]=>
  string(4) "Jane"
}
```
