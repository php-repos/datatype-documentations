# Introduction

The `ArrayCollection` class is part of the Datatype package.
You can use this class to make collections of any array.
Here you can see some example of the usage.

> **Note**  
> For more information about collections, please read [collection's documentation](https://saeghe.com/packages/datatype/documentations/collection-class)

## Usage

As mentioned before, you can use the `ArrayCollection` class for making collection object for any array.

```php
use Saeghe\Datatype\ArrayCollection;

$array = new ArrayCollection([1 => 'foo', 'bar' => 'baz', 'qux' => null]);

$count = count($array);

foreach ($array as $key => $value) {
    //
}

array_map($array->items(), ['new' => 'array']);
```

you can use collection methods on the instance:

```php
$array->append([null => null, 0 => 'corge']);
$array->put('waldo', 'name');

$array->each(function ($value, $key) {
    run_func($value, $key);
});

$except = $array->except(function ($value, $key) {
    return is_null($key);
});

$filter = $array->except(function ($value, $key) {
    return ! is_null($value);
});

$keys = $array->keys();
$values = $array->values();
```
