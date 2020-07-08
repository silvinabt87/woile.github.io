<!--
.. title: Rust for pythonistas
.. slug: rust-for-pythonistas
.. date: 2019-05-28 05:54:33 UTC-03:00
.. tags:
.. category:
.. link:
.. description:
.. type: text
-->

# RUST FOR PYTHONISTAS

## Data structures with typing

| Python                  | Rust                                           | Docs       |
| ----------------------- | ---------------------------------------------- | ---------- |
| `num: int = 1`          | `let num: i32 = 1; // used as default integer` | [integers] |
| `word: str = "avocado"` | `let word: String = String::from("avocado");`  | [strings]  |
| `point: Tuple = (1, 2)` | `let point: (i32, i32) = (1, 2)`               | [tuples]   |

## builins

| Python               | Rust                            |
| -------------------- | ------------------------------- |
| `print("holis")`     | `println!("holis")`             |
| `map`                | `a_vector.into_iter().map()`    |
| `filter`             | `a_vector.into_iter().filter()` |
| `functools.reduce`\* | `a_vector.into_iter().fold()`   |

- not a builtin but still useful

## Use one variable

Because of immutability and [borrowing][borrowing], try not to spread variables arround.
This basically means, do not spread the content of a variable into multiple variables.
This is not a problem with native data structures, but it's easy to forget about it.

In rust something like this will fail:

```rust
let mama = String::from("pipo");
let moma = mama;
println!("{} {}", mama, moma);
```

Why? Security and robustness.

### Immutability

What does this mean?

[integers]: https://doc.rust-lang.org/book/ch03-02-data-types.html#integer-types
[strings]: https://doc.rust-lang.org/book/ch08-02-strings.html
[tuples]: https://doc.rust-lang.org/book/ch03-02-data-types.html#the-tuple-type
[borrowing]: https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html