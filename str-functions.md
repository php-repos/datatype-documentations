a# Introduction

The `Str` namespace is part of the Datatype package.
Here you can see a list of included functions and their documentation.

## after_first_occurrence

### Signature

```php
after_first_occurrence(string $subject, string $needle): string
```

### Definition

The `after_first_occurance` returns the substring of the given subject after the first occurrence of the given needle.

It returns the subject if the needle is an empty string.

It returns the subject string when the subject does not contain the needle.

### Examples

```php
echo after_first_occurrence('hello world', ''); // Output: 'hello world'
echo after_first_occurrence('My\Class\Name', '\\'); // Output: 'Class\Name'
echo after_first_occurrence('This is another sentence contains i to test', 'c'); // Output: 'e contains i to test'
echo after_first_occurrence('hello world', 'my'); // Output: 'hello world'
```

## after_last_occurrence

### Signature

```php
after_last_occurrence(string $subject, string $needle): string
```

### Definition

The `after_last_occurrence` returns the given subject’s substring after the given needle’s last occurrence.

It returns the subject string when the needle is empty.

It returns the subject string when the subject does not contain the needle.

### Examples

```php
echo after_last_occurrence('hello world', ''); // Output: 'hello world'
echo after_last_occurrence('My\Class\Name', '\\'); // Output: 'Name'
echo after_last_occurrence('This is another sentence contains i to test', 'c'); // Output: ' to test'
echo after_last_occurrence('hello world', 'my'); // Output: 'hello world'
```

## before_first_occurrence

### Signature

```php
before_first_occurrence(string $subject, string $needle): string
```

### Definition

The `before_first_occurrence` returns the substring of the given subject before the first occurrence of the given needle.

It returns the subject string when the needle is empty.

It returns the subject string when the subject does not contain the needle.

### Examples

```php
echo before_first_occurrence('hello world', ''); // Output: 'hello world'
echo before_first_occurrence('My\Class\Name', '\\'); // Output: 'My'
echo before_first_occurrence('This is another sentence contains i to test', 'c'); // Output: 'This is another senten'
echo before_first_occurrence('hello world', 'my'); // Output: 'hello world'
```

## before_last_occurrence

### Signature

```php
before_last_occurrence(string $subject, string $needle): string
```

### Definition

The `before_last_occurrence` returns the substring of the given subject before the first occurrence of the given needle.

It returns the subject when the needle is empty.

It returns the subject when the subject does not contain the needle.

### Examples

```php
echo before_last_occurrence('hello world', ''); // Output: 'hello world'
echo before_last_occurrence('My\Class\Name', '\\'); // Output: 'My\Class'
echo before_last_occurrence('This is another sentence contains i to test', 'i'); // Output: 'This is another sentence contains '
echo before_last_occurrence('hello world', 'my'); // Output: 'hello world'
```

## between

### Signature

```php
function between(string $subject, string $start, string $end): string
```

### Definition

The `between` returns substring in the given subject between the given start and the given end substrings.

It returns the substring in the given subject from the given start to the end of the subject when the given end is empty.

It returns the substring in the given subject from the start to the given end of the subject when the given start is empty.

It returns the given subject when the start and the end are empty strings.

### Examples

```php
echo between('hello world', 'hello', 'world'); // Output: ' '
echo between('This is hello world', 'This is ', ' world'); // Output: 'hello'
echo between('This is hello world', '', ' world'); // Output: 'This is hello'
echo between('This is hello world', '', ''); // Output: 'This is hello world'
```

## first_character

### Signature

```php
first_character(string $subject): string
```

### Definition

It returns the first character of the given subject.

It returns an empty string when the subject is an empty string.

### Examples

```php
echo first_character('Hello World'); // Output: 'H'
echo first_character(' Hello World!'); // Output: ' '
echo first_character(''); // Output: ''
```

## last_character

### Signature

```php
last_character(string $subject): string
```

### Definition

It returns the last character of the given subject.

It returns an empty string when the subject is an empty string.

### Examples

```php
echo last_character('Hello World'); // Output: 'd'
echo last_character('Hello World!'); // Output: '!'
echo last_character(''); // Output: ''
```

## remove_first_character

### Signature

```php
remove_first_character(string $subject): string
```

### Definition

It removes the first character of the given subject and returns the remaining substring.

It returns an empty string when the given string is an empty string.

### Examples

```php
echo remove_first_character('hello world'); // Output: 'ello world'
echo remove_first_character(''); // Output: ''
```

## remove_last_character

### Signature

```php
remove_last_character(string $subject): string
```

### Definition

It removes the last character of the given subject and returns the remaining substring.

It returns an empty string when the given string is an empty string.

### Examples

```php
echo remove_last_character('hello world'); // Output: 'hello worl'
echo remove_last_character(''); // Output: ''
```

## replace_first_occurrence

### Signature

```php
replace_first_occurrence(string $subject, string $search, string $replace): string
```

### Definition

It replaces the first occurrence of the given substring in the given subject.

It returns an empty string when the given subject is an empty string.

It returns the subject when it does not contain the search substring.

### Examples

```php
echo replace_first_occurrence('hello world', 'world', 'universe'); // Output: 'hello universe'
echo replace_first_occurrence('hello world world', 'world', 'universe'); // Output: 'hello universe world'
echo replace_first_occurrence('hello world hello', 'hello', 'hi'); // Output: 'hi world hello'
echo replace_first_occurrence('', 'hello', 'hi'); // Output: ''
echo replace_first_occurrence('hello world', 'universe', 'hi'); // Output: 'hello world'
```

## starts_with_regex

### Signature

```php
starts_with_regex(string $subject, string $pattern): bool
```

### Definition

It checks if the given subject starts with the given regex.

It adds extra backslashes when the regex finishes with a backslash.

### Examples

```php
echo (string) starts_with_regex('h3ll0 world', '[A-Za-z]\d[A-Za-z]{2}\d'); // Output: 1
echo (string) starts_with_regex('word h3ll0', '[A-Za-z]\d[A-Za-z]{2}\d'); // Output: 0
echo (string) starts_with_regex('hello world', '[A-Za-z]\d[A-Za-z]{2}\d'); // Output: 0
echo (string) starts_with_regex('c:\\', '[A-Za-z]:\\'); // Output: 1
echo (string) starts_with_regex('c:\\filename', '[A-Za-z]:\\'); // Output: 1
echo (string) starts_with_regex(':\\', '[A-Za-z]:\\'); // Output: 0
echo (string) starts_with_regex('/root', '[A-Za-z]:\\'); // Output: 0
```
