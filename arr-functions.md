# Introduction

The `Arr` namespace is part of the Datatype package.
Here you can see a list of included functions and their documentation.

## insert_after

### Signature

```php
insert_after(array $array, mixed $key, array $additional): array
```

### Definition

It puts the given additional values after the given key in the array.

It puts the given additional values at the end of the given array when the given key not exists.

### Examples

```php
insert_after(['foo', 'baz'], 0, ['bar']); // Output: ['foo', 'bar', 'baz']
insert_after(['a' => 'foo', 'b' => 'baz'], 'a', ['c' => 'bar']); // Output: ['a' => 'foo', 'c' => 'bar', 'b' => 'baz']
insert_after(['a' => 'foo', 'b' => 'baz'], 'd', ['c' => 'bar']); // Output: ['a' => 'foo', 'b' => 'baz', 'c' => 'bar']
```

## last

### Signature

```php
last(array $array): array
```

### Definition

It returns the last item from the given array.

If the array is empty, `null` returns.

### Examples

```php
last(['foo', 'baz']); // Output: 'baz'
last(['foo', null]); // Output: null
last(['foo', 1]); // Output: 1
last(['foo', ['bar']]); // Output: ['bar']
last([]); // Output: null
```
