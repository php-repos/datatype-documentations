# Introduction

The `Text` class is part of the Datatype package.
You can use this class to make text objects.
Here you can see its API and see how to use it.

## Usage

```php
use Saeghe\Datatype\Text;

$body = new Text('This is an input');

// Use the variable as a string
echo $body;
$length = strlen($body)

// Use the string() method on the instance
echo $body->string();
```

You can also modify the variable using the `set` method:

```php
$body->set('new value for the body');
```
