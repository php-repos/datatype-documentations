# Datatype Package

## Introduction

The datatype package contains some data classes and some functions in different namespaces that you may need as a basic datatype for your project.

## Installation

You can run the following command to install this package:

```shell
saeghe add https://github.com/saeghe/datatype.git
```

## Functions

The Datatype package contains some namespaced functions to work with different datatypes.

### Str functions

The `Str` namespace contains a set of functions to work on strings.
For more information, please read [its documentation](https://saeghe.com/packages/datatype/documentations/str-functions)

### Arr functions

You can find some useful functions to work on arrays in this file.
For more information, please read [its documentation](https://saeghe.com/packages/datatype/documentations/arr-functions)

## Classes

Here you can find a list of available classes in this package.

### Text

The `Text` class is an abstract class for any text string.
By default, this class runs a validation against your text and throws an `InvalidArgumentException` if the given text is invalid.
If you extend this class, you need to define an `is_valid` method.
For more information, please read [its documentation](https://saeghe.com/packages/datatype/documentations/text-class)

> **Note**
> This class accepts a `string` so if you pass a `null` to it, the string sets as an empty string.

### AnyText

The `AnyText` class is a default class that accepts any text string.
This class extends the `Text` abstract class and can be used wherever you don’t have any validation rules for the string.
For more information, please read [its documentation](https://saeghe.com/packages/datatype/documentations/any-text-class)

### Collection

Just like what the `Text` class is to strings, the `Collection` class is to arrays.
You can extend this class to validate the type of acceptable keys and values for your collection.
Extending from this class force you to implement two methods, the `is_valid_key` and the `is_valid_value` that should return a boolean result.
For more information, please read [its documentation](https://saeghe.com/packages/datatype/documentations/collection-class)

### ArrayCollection

The `ArrayCollection` class allows you to use the collection methods against any set of key values.
You can consider it as a regular array that has access to the `Collection` methods.
For more information, please read [its documentation](https://saeghe.com/packages/datatype/documentations/array-collection-class)
