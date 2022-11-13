# Introduction

The `Text` abstract class is part of the Datatype package.
You can use this class to make text objects.
Here you can see its API and see how to use it.

## Usage

When you extend this class, you need to add one abstract method. 
For example, let's assume you have a `Username` class and the length of the username should be at least 6 characters. 

```php
use Saeghe\Datatype\Text;

class Username extends Text
{
    public function is_valid(string $string): bool
    {
        return strlen($string) >= 6;
    }
}
```

Now you can make new instance of this class and use it:

```php
$username = new Username('john.doe');

// Use the variable as a string
echo $username;
$length = strlen($username)

// Use the string() method on the instance
echo $username->string();
```

If you try to make a new user with an invalid value, it throws an `InvalidArgumentException`.

```php
try {
    $username = Username('john');
} catch (\InvalidArgumentException $exception) {
    echo $exception->getMessage(); // Output: 'Invalid string passed to text.'
}
```

You can also modify the variable using the `set` method:

```php
$username->set('jane.doe');
```

The `set` method also validates the passed value.
