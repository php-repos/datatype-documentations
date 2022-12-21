# Introduction

The `Collection` class is part of the Datatype package.
You can use this class to make collections.
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

---
### count

It returns count of available items in the collection.

```php
public function count(): int
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
assert_true(2 === $collection->count());
```
---
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

Output:

```shell
1 John
2 Jane
```
---
## every

### Signature

```php
public function every(Closure $check = null): bool
```

### Definition

It returns true when every item in the collection passes the check, otherwise, it returns false.

If the check is null or not given, it returns true when every item on the collection is valid.

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

The `except` accepts a closure and returns a new collection of items that do not pass the condition.

The `except` method returns a new collection.

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

Output:

```shell
array(1) {
  [2]=>
  string(4) "Jane"
}
```
---
### filter

The `filter` accepts a closure and returns a new collection of items that passes the condition.

The `filter` method returns a new collection.

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

Output:

```shell
array(1) {
  [1]=>
  string(4) "John"
}
```
---
## first_key

### Signature

```php
public function first_key(Closure $condition = null): string|int|null
```

### Definition

It returns the key of the first item in the collection that passes the given condition.

It returns the first key of the collection when the condition is null or not given.

It returns `null` when the collection is empty.

### Examples

```php
echo new Collection(['foo' => 1, 'bar' => 2, 'baz' => 2])->first_key(fn ($item, $key) => $item === 2); // Output: 'bar'
echo new Collection(['foo', 'baz'])->first_key(); // Output: 0
assert_true(null === new Collection([null => 'foo', 'foo' => 'bar'])->first_key());
echo new Collection([1 => 'bar', 'foo' => 'baz'])->first_key(); // Output: 1
echo new Collection('foo' => ['bar'], 'bar' => 'baz')->first_key(); // Output: 'foo'
assert_true(null === new Collection([])->first_key());
```
---
## first

### Signature

```php
public function first(Closure $condition = null): mixed
```

### Definition

It returns the value of the first item in the given array that passes the given condition.

It returns the first value of the array when the condition is null or not given.

It returns `null` when the given array is empty.

### Examples

```php
assert_true('bar' === new Collection(['foo', 'bar', 'baz'])->first(fn ($item, $key) => Str\first_character($item) === 'b'));
assert_true('foo' === new Collection(['foo', 'baz'])->first());
assert_true(null === new Collection([null, 'foo'])->first());
assert_true(1 === new Collection([1, 'foo'])->first());
assert_true(['bar'] === new Collection(['bar'], 'foo')->first());
assert_true(null === new Collection([])->first());
```
---
### forget

It unsets items from the collection that passes the given condition.

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

Output:

```shell
array(1) {
  [2]=>
  string(4) "Jane"
}
```
---
## has

### Signature

```php
public function has(Closure $closure): bool
```

### Definition

It returns true when at least one item in the collection passes the given condition.

### Examples

```php
assert_true(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $item === 'qux'));
assert_true(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $key === 'baz'));
assert_false(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $key === 0));
assert_false(new Collection(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $item === null));
```
---
## items

### Signature

```php
public function items(): array
```

### Definition

It returns an array contain all items of the collection.

### Examples

```php
$collection = new Collection(['foo' => 'bar', 'baz' => 'qux'];
assert_true(['foo' => 'bar', 'baz' => 'qux'] === $collection->items());
```
---
### keys

The `keys` method returns the collection's keys as an array.

```php
public function keys(): array
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
var_dump($collection->keys());
```

Output:

```shell
array(2) {
  [0]=>
  int(1)
  [1]=>
  int(2)
}
```
---
## last_key

### Signature

```php
public function last_key(Closure $condition = null): string|int|null
```

### Definition

It returns the key of the last item in the collection that passes the given condition.

It returns the last key of the collection when the condition is null or not given.

It returns `null` when the given collection is empty.

### Examples

```php
assert_true('baz' === new Collection(['foo' => 1, 'bar' => 2, 'baz' => 2])->last_key(fn ($item, $key) => $item === 2));
assert_true(null === new Collection([])->last_key());
assert_true(1 === new Collection(['foo', 'baz'])->last_key());
assert_true('' === new Collection(['foo' => 'bar', null => 'foo'])->last_key());
assert_true(1 === new Collection(['foo' => 'baz', 1 => 'bar'])->last_key());
assert_true('foo' === new Collection(['bar' => 'baz', 'foo' => ['bar']])->last_key());
```
---
## last

### Signature

```php
public function last(Closure $condition = null): mixed
```

### Definition

It returns the value of the last item in the collection that passes the given condition.

It returns the last value of the collection when the condition is null or not given.

It returns `null` when the collection is empty.

### Examples

```php
assert_true('baz' === new Collection(['foo', 'bar', 'baz'])->last(fn ($item, $key) => first_character($item) === 'b'));
assert_true('baz' === new Collection(['foo', 'baz'])->last());
assert_true(null === new Collection(['foo', null])->last());
assert_true(1 === new Collection(['foo', 1])->last());
assert_true(['bar'] === new Collection(['foo', ['bar']])->last());
assert_true(null === new Collection([])->last())
```
---
## map

### Signature

```php
public function map(Closure $closure): array
```

### Definition

It returns an array contain returned value by running each item of the collection against the given closure.

### Examples

```php
$collection = new Collection(['foo', 'bar', 'baz']);

assert_true(['0foo', '1bar', '2baz'] === $collection->map(fn ($item, $key) => $key.$item));
```
---
### push

It pushes the given item to the collection.

```php
public function push(mixed $value): static
```

#### Example

```php
$collection = new Collection([1 => 'foo', 2 => 'bar']);
$collection->push('baz');

assert_true([1 => 'foo', 2 => 'bar', 'baz'] === $collection->items());
```
---
### put

It puts the given item at the given index in the collection.

It puts items to the collection by natural key when it is not passed.

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
---
### reduce

The `reduce` returns a single value as the result of running the given closure against all items in the collection.
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
---
### take

It takes the first value of the collection that passes the given condition and unsets the item from the collection.

It returns null and keep the collection intact when condition does not meet for items.

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
---
### values

The `values` method returns an array of values from the collection.

```php
public function values(): array
```

#### Example

```php
$collection = new Collection([1 => 'John', 2 => 'Jane']);
var_dump($collection->values());
```

Output:

```shell
array(2) {
  [0]=>
  string(4) "John"
  [1]=>
  string(4) "Jane"
}
```
---
