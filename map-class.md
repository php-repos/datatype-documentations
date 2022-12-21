# Introduction

The `Map` class is part of the Datatype package.
You can use this class to make a collections of pairs.

> **Note**
> For more information about the Pair datatype, please read [its documentation](https://saeghe.com/packages/datatype/documentations/pair-class)

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

---
### count

It returns count of available items in the map.

```php
public function count(): int
```

#### Example

```php
$items = ['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')];
$map = new Map($items);
assert_true(2 === $map->count());
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
---
## every

### Signature

```php
public function every(Closure $check = null): bool
```

### Definition

It returns true when the map is empty.

It returns true when every item has value.

It returns false when items are empty.

It returns true when every item passes the check.

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
---
### except

The `except` accepts a closure and returns a new map of items that do not pass the condition.

The `except` method returns a new collection.

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
---
## first_key

### Signature

```php
public function first_key(Closure $closure = null): null|int|string
```

### Definition

It returns the key of the pair for the first item in the map that passes the given condition.

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
---
## first

### Signature

```php
public function first(Closure $condition = null): ?Pair
```

### Definition

It returns the first item of the map.

It returns the first item of the map that passes the given closure.

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
---
### forget

It unsets items from the map that passes the given condition.

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
---
## has

### Signature

```php
public function has(Closure $closure): bool
```

### Definition

It returns true when at least one item in the map passes the given condition.

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
$items = ['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')];
$map = new Map($items);

assert_true(['foo' => new Pair(1, 'baz'), 'bar' => new Pair(2, 'qux')] == $map->items());
```
---
## last

### Signature

```php
public function last(Closure $condition = null): ?Pair
```

### Definition

It returns the last item of the map.

It returns the last item of the map that passes the given closure.

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
---
### keys

It returns an array contain pair's key for each pair in the map.

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
---
### map

It returns empty array as map when there is no item in the map.

It returns an array contain results of running the callable against each item in the map.

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
---
### push

It pushes the given pair to the map items.

It replaces existing pair when the pair key matches with the given pair.

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
---
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
---
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
---
### take

It takes the first value of the map that passes the given condition and unsets the item from the map.

It returns null and keep the map intact when condition does not meet for items.

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
---
### values

It returns an array of map pair's values.

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
---
