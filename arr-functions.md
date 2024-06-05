# Introduction

The `Arr` namespace is part of the Datatype package.
Here you can see a list of included functions and their documentation.

## every

### Signature

```php
function every(array $array, Closure $check = null): bool
```

### Definition

This function checks whether all elements in an array pass a test implemented by a provided function.
If no function is provided, it checks if all elements in the array are truthy.

### Examples

```php
use function PhpRepos\Datatype\Arr\every;

echo (int) every(['foo', 'bar', 'baz'], fn ($item) => is_string($item)); // Output: 1
echo (int) every(['foo', 'bar', 'baz'], fn ($item, $key) => is_numeric($key)); // Output: 1
echo (int) every(['foo', 'bar', 'baz', 'quux'], fn ($item, $key) => strlen($item) === 3); // Output: 0
echo (int) every([1, 2, 3]); // Output: 1
echo (int) every(['foo', 'bar', 'baz']); // Output: 1
echo (int) every([null, 0, '', []]); // Output: 0
```

## first_key

### Signature

```php
function first_key(array $array, Closure $condition = null): string|int|null
```

### Definition

This function returns the key of the first item in the given array that passes the given condition.
If no condition is provided, it returns the first key of the array.
If the array is empty, it returns `null`.

### Examples

```php
use function PhpRepos\Datatype\Arr\first_key;

echo first_key(['foo' => 1, 'bar' => 2, 'baz' => 2], fn ($item, $key) => $item === 2); // Output: 'bar'
echo first_key(['foo', 'baz']); // Output: 0
assert_true(null === first_key([null => 'foo', 'foo' => 'bar']));
echo first_key([1 => 'bar', 'foo' => 'baz']); // Output: 1
echo first_key('foo' => ['bar'], 'bar' => 'baz'); // Output: 'foo'
assert_true(null === first_key([]));
```

## first

### Signature

```php
function first(array $array, Closure $condition = null): mixed
```

### Definition

This function returns the value of the first item in the given array that passes the given condition.
If no condition is provided, it returns the first value of the array.
If the array is empty, it returns `null`.

### Examples

```php
use function PhpRepos\Datatype\Arr\first;

assert_true('bar' === first(['foo', 'bar', 'baz'], fn ($item, $key) => Str\first_character($item) === 'b'));
assert_true('foo' === first(['foo', 'baz']));
assert_true(null === first([null, 'foo']));
assert_true(1 === first([1, 'foo']));
assert_true(['bar'] === first(['bar'], 'foo'));
assert_true(null === first([]));
```

## forget

### Signature

```php
function forget(array $array, Closure $condition): array
```

### Definition

This function removes items from an array that match a certain condition defined in the provided closure.
The closure receives the current item's value and key as arguments.

### Examples

```php
use function PhpRepos\Datatype\Arr\forget;

$array = ['foo', 'bar', 'baz'];

assert_true([2 => 'baz'] === forget($array, fn ($value, $key) => $value === 'foo' || $key === 1));
```

## has

### Signature

```php
function has(array $array, Closure $closure): bool
```

### Definition

This function checks if an item in an array passes the provided closure condition, and returns `true` if it does, `false` otherwise.

### Examples

```php
use function PhpRepos\Datatype\Arr\has;

assert_true(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $item === 'qux'));
assert_true(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $key === 'baz'));
assert_false(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $key === 0));
assert_false(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $item === null));
```

## insert_after

### Signature

```php
function insert_after(array $array, mixed $key, array $additional): array
```

### Definition

This function adds the provided array of items after the specified key in the initial array.
It puts the given additional values at the end of the array when the given key does not exist.

### Examples

```php
use function PhpRepos\Datatype\Arr\insert_after;

insert_after(['foo', 'baz'], 0, ['bar']); // Output: ['foo', 'bar', 'baz']
insert_after(['a' => 'foo', 'b' => 'baz'], 'a', ['c' => 'bar']); // Output: ['a' => 'foo', 'c' => 'bar', 'b' => 'baz']
insert_after(['a' => 'foo', 'b' => 'baz'], 'd', ['c' => 'bar']); // Output: ['a' => 'foo', 'b' => 'baz', 'c' => 'bar']
```

## last_key

### Signature

```php
function last_key(array $array, Closure $condition = null): null|int|string
```

### Definition

This function returns the key of the last item in the given array that passes the given condition.
If no condition is provided, it returns the last key of the array.
If the array is empty, it returns `null`.

### Examples

```php
use function PhpRepos\Datatype\Arr\last_key;

assert_true('baz' === last_key(['foo' => 1, 'bar' => 2, 'baz' => 2], fn ($item, $key) => $item === 2));
assert_true(null === last_key([]));
assert_true(1 === last_key(['foo', 'baz']));
assert_true('' === last_key(['foo' => 'bar', null => 'foo']));
assert_true(1 === last_key(['foo' => 'baz', 1 => 'bar']));
assert_true('foo' === last_key(['bar' => 'baz', 'foo' => ['bar']]));
```

## last

### Signature

```php
function last(array $array, Closure $condition = null): mixed
```

### Definition

This function returns the value of the last item in the given array that passes the given condition.
If no condition is provided, it returns the last value of the array.
If the array is empty, it returns `null`.

### Examples

```php
use function PhpRepos\Datatype\Arr\last;

assert_true('baz' === last(['foo', 'bar', 'baz'], fn ($item, $key) => first_character($item) === 'b'));
assert_true('baz' === last(['foo', 'baz']));
assert_true(null === last(['foo', null]));
assert_true(1 === last(['foo', 1]));
assert_true(['bar'] === last(['foo', ['bar']]));
assert_true(null === last([]))
```

## map

### Signature

```php
function map(array $array, Closure $callback): array
```

### Definition

This function applies a given function to every item in an array and returns a new array of the results.

### Examples

```php
use function PhpRepos\Datatype\Arr\map;

$array = [1, 2, 3, 4];

assert_true([0, 2, 6, 12] === map($array, fn ($item, $key) => $key * $item));
```

## reduce

### Signature

```php
function reduce(array $array, Closure $callback, mixed $carry = null): mixed
```

### Definition

This function applies a given function to each item in an array, accumulating the result and returning it.
It returns the carry when the array is empty

### Examples

```php
use function PhpRepos\Datatype\Arr\reduce;

assert_true('bar' === reduce(['foo', 'bar', 'baz'], fn ($carry, $value, $key) => $key === 1 ? $value : $carry));
assert_true('bar' === reduce(['foo', 'bar', 'baz'], fn ($carry, $value) => $value === 'bar' ? $value : $carry));
assert_true('foo' === reduce([], fn ($carry, $value, $key) => $value, 'foo'));
```

## skip

### Signature

```php
function skip(array $array, int $offset): array
```

### Definition

The `skip` function returns a new array by skipping the first `$offset` elements from the given array.

### Examples

```php
use function PhpRepos\Datatype\Arr\skip;

$arr = ['foo', 'bar', 'baz'];

assert_true(['foo', 'bar', 'baz'] === skip($arr, 0));
assert_true(['bar', 'baz'] === skip($arr, 1));
assert_true(['baz'] === skip($arr, 2));
assert_true([] === skip($arr, 3));
```

## take

### Signature

```php
function take(array &$array, Closure $condition): mixed
```

### Definition

This function retrieves an item from an array that meets the provided condition and removes it from the array.
If no item is found, it returns `null`.

### Examples

```php
use function PhpRepos\Datatype\Arr\take;

$arr = ['foo', 'bar', 'baz'];
$result = take($arr, fn ($item, $key) => $item === 'bar');
assert_true('bar' === $result);
assert_true([0 => 'foo', 2 => 'baz'] === $arr);

$arr = ['foo', 'bar', 'baz'];
$result = take($arr, fn ($item, $key) => $item === 'qux');

assert_true(null === $result);
assert_true([0 => 'foo', 1 => 'bar', 2 => 'baz'] === $arr);
```
