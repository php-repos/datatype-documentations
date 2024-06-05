# Introduction

The `Map` class in the `datatype` package is an implementation of the `ArrayAccess`, `IteratorAggregate`, and `Countable` interfaces,
which provides a way to work with arrays using object-oriented syntax.
The class is designed to hold `Pair` objects, which are composed of a key-value pair.
The `key` is used as the index and the value is the data.

> **Note**
> For more information about the Pair datatype, please read [its documentation](https://phpkg.com/packages/datatype/documentations/pair-class)

Here you can see its API and see how to use it.

## Usage

You can make a new instance of this class and use it as an array or use the available methods on the class:

```php
$items = ['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')];
$map = new Map($items);

assert_true($items === $map->items());

// Use as array
foreach ($map as $key => $pair) {
    echo $key.$pair->key.$pair->value.','; // Output: foo1baz,bar2qux
}

echo count($map); // Output: 2

// Use items() method on the object
foreach ($map->items() as $key => $pair) {
    echo $key.$pair->key.$pair->value.','; // Output: foo1baz,bar2qux
}

echo count($map->items()); // Output: 2
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
$items = ['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')];
$map = new Map($items);
assert_true(2 === $map->count());
```

### each

This method takes a Closure function as a parameter.
It iterates over the collection's items and calls the provided closure function for each item, passing the item's value and key as arguments.
The method returns the current instance of the class.

```php
public function each(Closure $closure): static
```

#### Example

```php
$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));

$result = [];
$actual = $map->each(function (Pair $pair, mixed $index) use (&$result) {
    $result[] = $index . $pair->key . $pair->value;
});

assert_true($actual instanceof Map);
assert_true([new Pair(1, 'foo'), new Pair(2, 'bar')] == $actual->items());
assert_true(['01foo', '12bar'] === $result);
```

## every

### Signature

```php
public function every(Closure $check = null): bool
```

### Definition

This method returns a boolean indicating whether all items in the map pass the check function.
The check function takes two arguments: the value and the key of the current item.
If the check function is not provided, the method checks if all items in the map have a truthy value.
It returns true when the map is empty.

### Examples

```php
// It returns true when the map is empty.
$map = new Map();
assert_true($map->every());
// It returns true when every item has value.
$map = new Map();
$map->push(new Pair(1, 'foo'))
    ->push(new Pair(2, 'bar'))
    ->push(new Pair(3, 'baz'));
assert_true($map->every());
// It returns false when items are empty.
$map = new Map();
$map->put(new Pair(1, null));
assert_false($map->every());

$map->put(new Pair(2, 0));
assert_false($map->every());

$map->put(new Pair(3, ''));
assert_false($map->every());
// It returns true when every item passes the check.
$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));
assert_true($map->every(fn (Pair $item) => is_string($item->value)));
assert_true($map->every(fn (Pair $item) => is_numeric($item->key)));
assert_false($map->every(fn (Pair $item) => is_null($item)));
```

### except

This method returns a new `Map` object that contains items that do not pass the check function.
The check function takes two arguments: the value and the key of the current item.
If the check function is not provided, the method filters out items that have a truthy value.

```php
public function except(Closure $check = null): static
```

#### Example

```php
$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));
$map->put(new Pair(4, 'qux'));

$result = $map->except(function (Pair $pair, int $index) {
    return $index === 0 || $pair->key === 2 || $pair->value === 'baz';
});

assert_true($result instanceof Map);
assert_true([
    3 => new Pair(4, 'qux')
    ] == $result->items()
);

$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, ''));
$map->put(new Pair(3, null));
$map->put(new Pair(4, 'qux'));
$map->put(new Pair(5, 0));
$map->put(new Pair(null, 'value'));
$map->put(new Pair(0, 'string'));

$result = $map->except();

assert_true($result instanceof Map);
assert_true([
    1 => new Pair(2, ''),
    2 => new Pair(3, null),
    4 => new Pair(5, 0),
    ] == $result->items()
);
```

## filter

### Signature

```php
public function filter(Closure $check = null): static
```

### Definition

The `filter` method returns a new map containing only the elements that satisfy the given condition specified
by the `$check` closure. If no closure is provided, it removes all elements that are considered "falsy" 
(e.g., `null`, `false`, `0`, `''`).

### Examples

```php
$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put($item2 = new Pair(2, 'bar'));
$map->put($item3 = new Pair(3, 'baz'));
$map->put($item4 = new Pair(4, 'qux'));

$result = $map->filter(function (Pair $pair, $key) {
    return $key === 1 || $pair->value === 'baz' || $pair->key === 4;
});

assert_true($result instanceof Map);
assert_true([1 => $item2, 2 => $item3, 3 => $item4] === $result->items());

$map = new Map();
$map->put(new Pair(1, null));
$map->put(new Pair(2, ''));
$map->put(new Pair(3, 0));
$map->put($item4 = new Pair(4, 'foo'));

$result = $map->filter();

assert_true($result instanceof Map);
assert_true([3 => $item4] === $result->items());
```

## first_key

### Signature

```php
public function first_key(Closure $closure = null): null|int|string
```

### Definition

This method returns the key of the first item in the map that passes the check function.
The check function takes two arguments: the value and the key of the current item.
If the check function is not provided, the method returns the first key in the map.

### Examples

```php
$map = new Map();
$map->push(new Pair(1, 'foo'));
$map->push(new Pair(2, 'bar'));
$map->push(new Pair(3, 'baz'));
assert_true(2 === $map->first_key(fn (Pair $pair) => $pair->value === 'bar'));

$map = new Map();
$map->push(new Pair(1, 'foo'));
$map->push(new Pair(2, 'bar'));
$map->push(new Pair(3, 'baz'));

assert_true(1 === $map->first_key());
```

## first

### Signature

```php
public function first(Closure $condition = null): ?Pair
```

### Definition

This method returns the first item in the map that passes the check function.
The check function takes two arguments: the value and the key of the current item.
If the check function is not provided, the method returns the first item in the map.

### Examples

```php
$map = new Map();
assert_true(null === $map->first());

$map->put(new Pair(1, 'foo'));
assert_true(1 === $map->first()->key);
assert_true('foo' === $map->first()->value);

$map->put(new Pair(1, 'bar'));
assert_true(1 === $map->first()->key);
assert_true('bar' === $map->first()->value);

$map->put(new Pair(2, 'baz'));
assert_true(1 === $map->first()->key);
assert_true('bar' === $map->first()->value);
assert_true(2 === $map->last()->key);
assert_true('baz' === $map->last()->value);

$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));

$result = $map->first(fn (Pair $pair) => str_starts_with($pair->value, 'b'));

assert_true(2 === $result->key);
assert_true('bar' === $result->value);
```

### forget

This method removes items from the map that pass the check function.
The check function takes two arguments: the value and the key of the current item.
The method returns the current map object, which allows you to chain multiple calls.

```php
public function forget(Closure $condition): static
```

#### Example

```php
$map = new Map();
$map->put($item = new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));

assert_true([
        2 => new Pair(3, 'baz'),
    ] == $map->forget(fn (Pair $pair, int $index) => $pair === $item || $index === 1)->items()
);
```

## has

### Signature

```php
public function has(Closure $closure): bool
```

### Definition

The `has` method checks if the provided closure returns `true` for any of the elements in the map.
The closure is passed two arguments, the value and the key of the current element.
It returns a boolean indicating whether or not the condition is `true` for any of the elements.

### Examples

```php
$map = new Map();
$map->put(new Pair('foo', 'bar'));
$map->put(new Pair('baz', 'qux'));

assert_true($map->has(fn ($item) => $item->value === 'qux'));
assert_true($map->has(fn ($item) => $item->key === 'baz'));
assert_false($map->has(fn ($item) => $item->key === 0));
assert_false($map->has(fn ($item) => $item->value === null));
```

## items

### Signature

```php
public function items(): array
```

### Definition

The `items` method in the `Map` class returns an array representation of the items in the collection.
Each item in the array is an instance of the `Pair` class, which has two properties: key and value.
The key property represents the index of the item in the original array,
and the value property represents the value of the item.

### Examples

```php
$items = ['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')];
$map = new Map($items);

assert_true(['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')] == $map->items());
```

## last

### Signature

```php
public function last(Closure $condition = null): ?Pair
```

### Definition

This method takes an optional closure as an argument that is used as a condition to filter the items in the map.
If the closure is not provided, the method will return the last item in the map.
If the closure is provided, the method will iterate over each item in the map and return the last item that satisfies the condition provided in the closure.
The closure takes two arguments, the current item and its key, and should return a boolean value.
If the condition is not met for any of the items in the map, the method will return `null`.

### Examples

```php
$map = new Map();
assert_true(null === $map->last());

$map->put(new Pair(1, 'foo'));
assert_true(1 === $map->last()->key);
assert_true('foo' === $map->last()->value);

$map->put(new Pair(1, 'bar'));
assert_true(1 === $map->last()->key);
assert_true('bar' === $map->last()->value);

$map->put(new Pair(2, 'baz'));
assert_true(1 === $map->first()->key);
assert_true('bar' === $map->first()->value);
assert_true(2 === $map->last()->key);
assert_true('baz' === $map->last()->value);

$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));

$result = $map->last(fn (Pair $pair) => str_starts_with($pair->value, 'b'));

assert_true(3 === $result->key);
assert_true('baz' === $result->value);
```

### keys

This method returns an array of all the keys in the Map object.
It iterates over the items in the `$items` property and adds each item's key to the array.
The returned array contains the keys of the `Pair` objects in the same order as they appear in the `$items` property.

> **Note**
> It's important to note that the method does not return the index of the items in the `$items` array but the key of the Pair.

```php
public function keys(): array
```

#### Example

```php
$map = new Map();
assert_true([] === $map->keys());

$map->put(new Pair(1, 'foo'));
assert_true([1] === $map->keys());

$map->put(new Pair('bar', 'baz'));
assert_true([1, 'bar'] === $map->keys());
```

### map

This method is used to apply a callback function to each element in the map's underlying items array 
and return a new array containing the results.
The callback function takes two arguments: the value and the key of the current element. .
If the underlying items array is empty, the method will return an empty array.

```php
public function map(Closure $callable): array
```

#### Example

```php
$map = new Map();
assert_true([] === $map->map(fn (Pair $pair) => $pair->key.$pair->value));

$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'), 'baz');
assert_true(['01foo', 'baz2bar'] === $map->map(fn (Pair $pair, mixed $index) => $index.$pair->key.$pair->value));
```

### push

It pushes the given pair to the map items.
It replaces existing pair when the pair key matches with the given pair.
It is important to note that the push method can only be used to add `Pair` objects to the items array, and not any other data types.
Attempting to use the push method with any other data type will result in an error.

```php
public function push(Pair $item): static
```

#### Example

```php
$map = new Map();
$map->push(new Pair(1, 'foo'));
assert_true([new Pair(1, 'foo')] == $map->items());
$map->push(new Pair(2, 'bar'));
assert_true([new Pair(1, 'foo'), new Pair(2, 'bar')] == $map->items());

$map = new Map();
$map->push(new Pair(1, 'foo'));
assert_true([new Pair(1, 'foo')] == $map->items());
$map->push(new Pair(1, 'bar'));
assert_true([new Pair(1, 'bar')] == $map->items());
```

### put

It puts the given pair in the given index to the map.
If the index not passed, it puts the given pair to the map by natural index.
It the index not passed and there is item by matching the given pair's key, then the item with matched key replaces.

```php
public function put(Pair $item, int|string|null $index = null): static
```

#### Example

```php
$map = new Map();
$map->put(new Pair(1, 'foo'));
assert_true(1 === $map->first()->key);
assert_true('foo' === $map->first()->value);

$map->put(new Pair(1, 'bar'));
assert_true(1 === $map->first()->key);
assert_true('bar' === $map->first()->value);

$map->put(new Pair(2, 'baz'));
assert_true(1 === $map->first()->key);
assert_true('bar' === $map->first()->value);
assert_true(2 === $map->last()->key);
assert_true('baz' === $map->last()->value);

$map->put($item = new Pair(3, 'qux'), 'foo');
assert_true($item === $map->items()['foo']);
```

### reduce

The `reduce` returns a single value as the result of running the given closure against all items in the map.
It works just like the `array_reduce` function. If no carry passed, the initial carry sets as `null`.

```php
public function reduce(Closure $closure, mixed $carry = null): mixed
```

#### Example

```php
$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));
$result = $map->reduce(fn ($carry, Pair $pair) => $pair->value === 'bar' ? $pair : $carry);
assert_true(new Pair(2, 'bar') == $result);

$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));
$result = $map->reduce(fn ($carry, Pair $pair) => $pair->value === 'not-exists' ? $pair : $carry);
assert_true(null === $result);

$map = new Map();
$map->put(new Pair(1, 'foo'));
$map->put(new Pair(2, 'bar'));
$map->put(new Pair(3, 'baz'));
$result = $map->reduce(fn ($carry, Pair $pair) => $pair->value === 'not-exists' ? $pair : $carry, 'this is carry');
assert_true('this is carry' === $result);
```

### skip

The `skip` method returns a new map by skipping the specified number of elements from the beginning of the given map.

```php
public function skip(int $offset): static
```

#### Example

```php
$map = new Map();
$map->push($item1 = new Pair(1, 'foo'));
$map->push($item2 = new Pair(2, 'bar'));
$map->push($item3 = new Pair(3, 'baz'));

assert_true([0 => $item1, 1 => $item2, 2 => $item3] === $map->skip(0)->items());
assert_true([0 => $item2, 1 => $item3] === $map->skip(1)->items());
assert_true([0 => $item3] === $map->skip(2)->items());
assert_true([] === $map->skip(3)->items());
```

### take

It returns the first value of the map that passes the given condition and unsets the item from the map.
It returns `null` and keep the map intact when condition does not meet for items.

```php
public function take(Closure $condition): ?Pair
```

#### Example

```php
$map = new Map();
$map->push($item1 = new Pair(1, 'foo'));
$map->push($item2 = new Pair(2, 'bar'));
$map->push($item3 = new Pair(3, 'baz'));
$result = $map->take(fn (Pair $pair) => $pair->value === 'bar');
assert_true($item2 === $result);
assert_true([0 => $item1, 2 => $item3] === $map->items());

$map = new Map();
$map->push($item1 = new Pair(1, 'foo'));
$map->push($item2 = new Pair(2, 'bar'));
$map->push($item3 = new Pair(3, 'baz'));
$result = $map->take(fn (Pair $pair) => $pair->value === 'qux');
assert_true(null === $result);
assert_true([0 => $item1, 1 => $item2, 2 => $item3] === $map->items());
```

### values

It retrieves an array of the values stored in the map.

```php
public function values(): array
```

#### Example

```php
$map = new Map();
assert_true([] === $map->values());

$map->put(new Pair(1, 'foo'));
assert_true(['foo'] === $map->values());

$map->put(new Pair('bar', 'baz'));
assert_true(['foo', 'baz'] === $map->values());
```
