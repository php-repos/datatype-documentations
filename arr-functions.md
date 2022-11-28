# Introduction

The `Arr` namespace is part of the Datatype package.
Here you can see a list of included functions and their documentation.

---
## every

### Signature

```php
function every(array $array, \Closure $check = null): bool
```

### Definition

It returns true when every item in the array passes the check, otherwise, it returns false.

If the check is null or not given, it returns true when every item on the every is valid.

### Examples

```php
use function Saeghe\Datatype\Arr\every;

echo (int) every(['foo', 'bar', 'baz'], fn ($item) => is_string($item)); // Output: 1
echo (int) every(['foo', 'bar', 'baz'], fn ($item, $key) => is_numeric($key)); // Output: 1
echo (int) every(['foo', 'bar', 'baz'], fn ($item, $key) => strlen($item) > 3); // Output: 0
echo (int) every([1, 2, 3]); // Output: 1
echo (int) every(['foo', 'bar', 'baz']); // Output: 1
echo (int) every([null, 0, '', []]); // Output: 0
```
---
## first_key

### Signature

```php
function first_key(array $array, \Closure $condition = null): mixed
```

### Definition

It returns the key of the first item in the given array that passes the given condition.

It returns the first key of the array when the condition is null or not given.

It returns `null` when the given array is empty.

### Examples

```php
use function Saeghe\Datatype\Arr\first_key;

echo first_key(['foo' => 1, 'bar' => 2, 'baz' => 2], fn ($item, $key) => $item === 2); // Output: 'bar'
echo first_key(['foo', 'baz']); // Output: 0
assert_true(null === first_key([null => 'foo', 'foo' => 'bar']));
echo first_key([1 => 'bar', 'foo' => 'baz']); // Output: 1
echo first_key('foo' => ['bar'], 'bar' => 'baz'); // Output: 'foo'
assert_true(null === first_key([]));
```
---
## first

### Signature

```php
function first(array $array, \Closure $condition = null): mixed
```

### Definition

It returns the value of the first item in the given array that passes the given condition.

It returns the first value of the array when the condition is null or not given.

It returns `null` when the given array is empty.

### Examples

```php
use function Saeghe\Datatype\Arr\first;

assert_true('bar' === first(['foo', 'bar', 'baz'], fn ($item, $key) => Str\first_character($item) === 'b'));
assert_true('foo' === first(['foo', 'baz']));
assert_true(null === first([null, 'foo']));
assert_true(1 === first([1, 'foo']));
assert_true(['bar'] === first(['bar'], 'foo'));
assert_true(null === first([]));
```
---
## has

### Signature

```php
function has(array $array, \Closure $closure): bool
```

### Definition

It returns true when at least one item in the given array passes the given condition.

### Examples

```php
use function Saeghe\Datatype\Arr\has;

assert_true(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $item === 'qux'));
assert_true(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $key === 'baz'));
assert_false(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $key === 0));
assert_false(has(['foo' => 'bar', 'baz' => 'qux'], fn ($item, $key) => $item === null));
```
---
## insert_after

### Signature

```php
function insert_after(array $array, mixed $key, array $additional): array
```

### Definition

It puts the given additional values after the given key in the array.

It puts the given additional values at the end of the array when the given key does not exist.

### Examples

```php
use function Saeghe\Datatype\Arr\insert_after;

insert_after(['foo', 'baz'], 0, ['bar']); // Output: ['foo', 'bar', 'baz']
insert_after(['a' => 'foo', 'b' => 'baz'], 'a', ['c' => 'bar']); // Output: ['a' => 'foo', 'c' => 'bar', 'b' => 'baz']
insert_after(['a' => 'foo', 'b' => 'baz'], 'd', ['c' => 'bar']); // Output: ['a' => 'foo', 'b' => 'baz', 'c' => 'bar']
```
---
## last_key

### Signature

```php
function last_key(array $array, \Closure $condition = null): mixed
```

### Definition

It returns the key of the last item in the given array that passes the given condition.

It returns the last key of the array when the condition is null or not given.

It returns `null` when the given array is empty.

### Examples

```php
use function Saeghe\Datatype\Arr\last;

assert_true('baz' === last_key(['foo' => 1, 'bar' => 2, 'baz' => 2], fn ($item, $key) => $item === 2));
assert_true(null === last_key([]));
assert_true(1 === last_key(['foo', 'baz']));
assert_true('' === last_key(['foo' => 'bar', null => 'foo']));
assert_true(1 === last_key(['foo' => 'baz', 1 => 'bar']));
assert_true('foo' === last_key(['bar' => 'baz', 'foo' => ['bar']]));
```
---
## last

### Signature

```php
function last(array $array, \Closure $condition = null): mixed
```

### Definition

It returns the value of the last item in the given array that passes the given condition.

It returns the last value of the array when the condition is null or not given.

It returns `null` when the given array is empty.

### Examples

```php
use function Saeghe\Datatype\Arr\last;

assert_true('baz' === last(['foo', 'bar', 'baz'], fn ($item, $key) => first_character($item) === 'b'));
assert_true('baz' === last(['foo', 'baz']));
assert_true(null === last(['foo', null]));
assert_true(1 === last(['foo', 1]));
assert_true(['bar'] === last(['foo', ['bar']]));
assert_true(null === last([]))
```
---
