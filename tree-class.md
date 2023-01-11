# Introduction

The `Tree` class is part of the Datatype package.
You can use this class to define any tree structures.
Here you can see its API and see how to use it.

## Usage

You can make a new instance of this class by passing a root to the constructor and then use the following methods:

```php
$tree = new Tree('/');

assert_true($tree->root === '/');
assert_true($tree->vertices() instanceof Collection);
assert_true($tree->edges() instanceof Collection);
assert_true(['/'] === $tree->vertices()->items());
assert_true([] === $tree->edges()->items());
```

## Methods

Here you can see a list of the available methods on the tree:

---
## edge

### Signature

```php
public function edge($vertex, $leaf): static
```

### Definition

It adds an edge to the tree.

### Examples

```php
$tree = new Tree('/');
$tree->edge('/', 'home');
assert_true(['/', 'home'] === $tree->vertices()->items());
assert_true([new Pair('/', 'home')] == $tree->edges()->items());
```
---
## edges

### Signature

```php
public function edges(): Collection
```

### Definition

It returns a collection of defined edges.

### Examples

```php
$tree = new Tree('/');
$tree->edge('/', 'home');
assert_true([new Pair('/', 'home')] == $tree->edges()->items());
```
---
## vertices

### Signature

```php
public function vertices(): Collection
```

### Definition

It returns a collection of added vertices.

### Examples

```php
$tree = new Tree('/');
$tree->edge('/', 'home');
assert_true(['/', 'home'] === $tree->vertices()->items());
```
---
