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
