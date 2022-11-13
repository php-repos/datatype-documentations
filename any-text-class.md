# Introduction

The `AnyText` class is part of the Datatype package.
You can use this class for making any text object.
It sets the value as an empty string is `null` passes as the value.
Here you can see its API and see how to use it.

## Usage

You can initiate an object from the class:

```php
use Saeghe\Datatype\AnyText;

$text = new AnyText('hello world');

// Use the variable as a string
echo $text; // Output: 'hello world'
strlen($text); // Output: 11

// Use the string() method
echo $text->string(); // Output: 'hello world'
```

You can use the `set` method to override the existing variable:

```php
$text->set('hello universe');
```
