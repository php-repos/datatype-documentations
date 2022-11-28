# Introduction

The `Collection` abstract class is part of the Datatype package.
You can use this class to make collections.
Here you can see its API and see how to use it.

## Usage

When you extend this class, you need to add two abstract methods.
For example, assume you want to make a collection for keeping a list of names:

```php
use Saeghe\Datatype\Collection;

Class ListItems extends Collection
{
    public function key_is_valid(mixed $key): bool
    {
        return is_int($key);
    }

    public function value_is_valid(mixed $value): bool
    {
        return is_string($value) && strlen($value) > 3;
    }
}
```

Now you can make a new instance of this class and use it as an array or use the available methods on the class:

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);

// Use as array
foreach ($list as $number => $item) {
    echo $number . ' ' . $item . ','; // Output: 1 John,2 Jane
}

echo count($list); // Output: 2

// Use items() method on the object
foreach ($list->items() as $number => $item) {
    echo $number . ' ' . $item . ','; // Output: 1 John,2 Jane
}

echo count($list->items()); // Output: 2
```

## Methods

Here you can see a list of the available methods on the collection:

---
### append

You can use the `append` method to append new key-value pairs to the existing item on the object.
All keys and values get validated against the rules you defined.

```php
public function append(array $items): static
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$list->append([3 => 'Mark', 4 => 'Sara']); // [1 => 'John', 2 => 'Jane', 3 => 'Mark', 4 => 'Sara']
```
---
### each

The `each` method can get used to run a closure against each item of the collection.
The `each` method always passes the value as the first argument and the key as the second argument to the given closure.

```php
public function each(\Closure $closure): static
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$list->each(function ($value, $key) {
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
public function every(\Closure $check = null): bool
```

### Definition

It returns true when every item in the collection passes the check, otherwise, it returns false.

If the check is null or not given, it returns true when every item on the collection is valid.

### Examples

```php
echo (int) new ListItems(['foo', 'bar', 'baz'])->every(fn ($item) => is_string($item)); // Output: 1
echo (int) new ListItems(['foo', 'bar', 'baz'])->every(fn ($item, $key) => is_numeric($key)); // Output: 1
echo (int) new ListItems(['foo', 'bar', 'baz'])->every(fn ($item, $key) => strlen($item) > 3); // Output: 0
echo (int) new ListItems([1, 2, 3])->every(); // Output: 1
echo (int) new ListItems(['foo', 'bar', 'baz'])->every(); // Output: 1
echo (int) new ListItems([null, 0, '', []])->every(); // Output: 0
```
---
### except

The `except` accepts a closure and returns a new collection of items that do not pass the condition.

The `except` method returns a new collection.

```php
public function except(?\Closure $closure = null): static
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$result = $list->except(function ($value, $key) {
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
public function filter(?\Closure $closure = null): static
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$result = $list->filter(function ($value, $key) {
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
public function first_key(\Closure $condition = null): mixed
```

### Definition

It returns the key of the first item in the collection that passes the given condition.

It returns the first key of the collection when the condition is null or not given.

It returns `null` when the collection is empty.

### Examples

```php
echo new ListItems(['foo' => 1, 'bar' => 2, 'baz' => 2])->first_key(fn ($item, $key) => $item === 2); // Output: 'bar'
echo new ListItems(['foo', 'baz'])->first_key(); // Output: 0
assert_true(null === new ListItems([null => 'foo', 'foo' => 'bar'])->first_key());
echo new ListItems([1 => 'bar', 'foo' => 'baz'])->first_key(); // Output: 1
echo new ListItems('foo' => ['bar'], 'bar' => 'baz')->first_key(); // Output: 'foo'
assert_true(null === new ListItems([])->first_key());
```
---
## first

### Signature

```php
public function first(\Closure $condition = null): mixed
```

### Definition

It returns the value of the first item in the given array that passes the given condition.

It returns the first value of the array when the condition is null or not given.

It returns `null` when the given array is empty.

### Examples

```php
use function Saeghe\Datatype\Arr\first;

assert_true('bar' === new ListItems(['foo', 'bar', 'baz'])->first(fn ($item, $key) => Str\first_character($item) === 'b'));
assert_true('foo' === new ListItems(['foo', 'baz'])->first());
assert_true(null === new ListItems([null, 'foo'])->first());
assert_true(1 === new ListItems([1, 'foo'])->first());
assert_true(['bar'] === new ListItems(['bar'], 'foo')->first());
assert_true(null === new ListItems([])->first());
```
---
### forget

The `forget` method removes the item with the given key from items in the collection.

```php
public function forget(mixed $key): static
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$list->forget(1);
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
## has

### Signature

```php
public function has(\Closure $closure): bool
```

### Definition

It returns true when at least one item in the collection passes the given condition.

### Examples

```php
assert_true(new ListItems(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $item === 'qux'));
assert_true(new ListItems(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $key === 'baz'));
assert_false(new ListItems(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $key === 0));
assert_false(new ListItems(['foo' => 'bar', 'baz' => 'qux'])->has(fn ($item, $key) => $item === null));
```
---
### keys

The `keys` method returns the collection's keys as an array.

```php
public function keys(): array
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
var_dump($list->keys());
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
pubic function last_key(\Closure $condition = null): mixed
```

### Definition

It returns the key of the last item in the collection that passes the given condition.

It returns the last key of the collection when the condition is null or not given.

It returns `null` when the given collection is empty.

### Examples

```php
assert_true('baz' === new ListItems(['foo' => 1, 'bar' => 2, 'baz' => 2])->last_key(fn ($item, $key) => $item === 2));
assert_true(null === new ListItems([])->last_key());
assert_true(1 === new ListItems(['foo', 'baz'])->last_key());
assert_true('' === new ListItems(['foo' => 'bar', null => 'foo'])->last_key());
assert_true(1 === new ListItems(['foo' => 'baz', 1 => 'bar'])->last_key());
assert_true('foo' === new ListItems(['bar' => 'baz', 'foo' => ['bar']])->last_key());
```
---
## last

### Signature

```php
public function last(\Closure $condition = null): mixed
```

### Definition

It returns the value of the last item in the collection that passes the given condition.

It returns the last value of the collection when the condition is null or not given.

It returns `null` when the collection is empty.

### Examples

```php
assert_true('baz' === new ListItems(['foo', 'bar', 'baz'])->last(fn ($item, $key) => first_character($item) === 'b'));
assert_true('baz' === new ListItems(['foo', 'baz'])->last());
assert_true(null === new ListItems(['foo', null])->last());
assert_true(1 === new ListItems(['foo', 1])->last());
assert_true(['bar'] === new ListItems(['foo', ['bar']])->last());
assert_true(null === new ListItems([])->last())
```
---
### put

The `put` method adds the given value by the given key to the items of the collection.

```php
public function put(mixed $value, mixed $key = null): static
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$list->put('mark', 3); // Items: [1 => 'John', 2 => 'Jane', 3 => 'Mark']
```
---
### reduce

The `reduce` returns a single value as the result of running the given closure against all items in the collection.
It works just like the `array_reduce` function. If no carry passed, the initial carry sets as `null`.

```php
public function reduce(\Closure $closure, mixed $carry = null): mixed
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
$result = $list->reduce(function ($carry, $value) {
    return $value === 'Jane' || $carry;
}, false);

var_dump($result); // Output: bool(true)
```
---
### values

The `values` method returns an array of values from the collection.

```php
public function values(): array
```

#### Example

```php
$list = new ListItems([1 => 'John', 2 => 'Jane']);
var_dump($list->values());
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
