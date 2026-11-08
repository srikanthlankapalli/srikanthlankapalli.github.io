---
layout: post
title: "Generators"
date: 2026-09-24 00:00:00 +0000
categories:
  - programming
  - python
---

Python generators are a simple way to create iterators without storing everything in memory at once.

They are especially useful when working with large datasets or when you want to lazily produce values one at a time.

## Why use generators?

A normal function returns a single value, but a generator yields one value at a time and pauses between yields. This makes it easy to process streams of data efficiently.

```python
def count_up_to(limit):
    current = 1
    while current <= limit:
        yield current
        current += 1

for value in count_up_to(5):
    print(value)
```

This prints:

```python
1
2
3
4
5
```

## When to prefer generators

- processing large files
- streaming data from APIs
- working with infinite or very large sequences
- reducing memory usage in data pipelines

Generators are a powerful tool in Python because they keep code readable while improving efficiency.
