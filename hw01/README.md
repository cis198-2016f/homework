# Homework 01: Rust Finger Exercises

**Due Wednesday, 2016-09-07, 11:59pm.**

Please ask questions on Piazza (Penn students) or Google Groups (other).
Links on [course homepage].

[course homepage]: cis198-2016f.github.io

## Overview

In this assignment, you'll get your first crack at writing some short functions
in a Rust library, building and testing using Cargo, and writing modules. This
assignment is intended to be relatively straightforward, and will make sure
you're set up properly in the Rust ecosystem.

You may find it useful to reference first lecture slides and [Rust by Example](http://rustbyexample.com/)

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:

- https://classroom.github.com/assignment-invitations/9f8383b9aaf081fbbeedd8cb0027d08d

Don't clone this repository just yet -- we'll use Cargo to initialize the git
repository.

## Part 01: Cargo

Create a new Rust library: `cargo new hw01`. Cargo creates this directory
structure:

```
hw01
├── .git/
├── .gitignore
├── Cargo.toml
└── src
    └── lib.rs
```

Since this is a library and not an executable, Cargo creates `lib.rs` instead of
`main.rs`.

Add your Classroom for GitHub repository as a git remote:

```
git remote add origin git@github.com:cis198-2016f/hw01-<username>.git
git push -u origin master
```

You can build this project from anywhere in the project tree with
`cargo build`. Be sure to compile periodically as you work!

Go ahead and add `tests_provided.rs` to your Cargo project. You'll write the
rest of this assignment from scratch!

### Modules

Before you go any further, a word on modules and crates.

A crate is a package of Rust code -- which might be a binary, a library, or
multiple of both. A module is an logical grouping of some Rust code, used for
namespacing and code organization. Someone using your library can selectively
import the modules that they want to use.

One easy way of making and using modules is to put each one in a different file.
Any file `foo.rs` implicitly defines a module named `foo`.

```
hw01
├── Cargo.toml
└── src
    ├── problem1.rs
    ├── problem2.rs
    └── lib.rs
```

To add these files to your crate, add these lines to the top of `lib.rs`.

```rust
pub mod problem1;
pub mod problem2;
```

Until you add this directive, Cargo will not try to build `problem1` as part of
your crate. The `pub` keyword exposes the module `problem1` to any other crate
that imports your library. You can omit `pub` to leave modules private, so that
items in a module can only be used in your crate; however, if a function is not
exported or used internally, it will emit a dead code warning.

Sometimes you don't want to create a new file for each new module. You could
structure your project like this instead:

```rust
// filename: src/lib.rs
pub mod problem1 {
    // ...
}

pub mod problem2 {
    // ...
}

// ...
```

This is exactly the same as the 3-file structure above.

Items that can be exported from modules include functions, type definitions, and
other modules.

```rust
// problem1.rs

/// Functions are private (only available to this module) by default.
/// Use the `pub` keyword to mark this function as public.
pub fn sum(slice: &[i32]) -> i32 {
    // ...
}
```

There are a few different ways to import items from a different module:

1. To add `sum` to the scope of a file, add the line `use problem1::sum` to the
   top of the file. You can call the function with `sum()`.

2. To import multiple things from a module, use curly brace-expansion:
   `use problem1::{sum, dedup};`

3. To import all items from a module, use `use problem1::*`.

4. To import an entire module, write `use problem1;`. Since the items inside
   this module are not imported directly, they need to be *namespaced*:
   `problem1::sum()`. This is more verbose, but avoids adding too many things
   into your namespace.

You can read more about modules in the Rust book
[here](https://doc.rust-lang.org/book/crates-and-modules.html).

#### `tests_provided.rs`

We provided a `tests_provided.rs` file with a few test cases to start you off
with. You should add this to your library as a separate module, as you did with
each `problemN` module (but the tests don't need to be `pub`).

At the top of this file, there's a `#![cfg(test)]` attribute. `cfg`, short for
"configuration", is part of how Rust handles conditional compilation. This is
similar to (but way better than) the use of `#ifdef`s and include guards in C.
In this case, `#![cfg(test)]` tells the compiler that this module is not to be
compiled unless the `--test` flag is used with `rustc` (`cargo test` adds this
flag under the hood). This is handy because it means that you don't have to
waste time recompiling your tests every time you build unless you're actually
going to run them.

All of the functions in `test.rs` are annotated with the `#[test]` attribute;
this tells the compiler that they're tests. Test functions must have the
signature `fn() -> ()`, or else they will not compile. Tests will be run when
you invoke `cargo test`.

Any test which doesn't cause a `panic!` is considered to pass. You should use
[`assert!`][assert] or [`assert_eq!`][assert_eq] to check guarantees and
equality (respectively).

[assert]: https://doc.rust-lang.org/std/macro.assert!.html
[assert_eq]: https://doc.rust-lang.org/std/macro.assert_eq!.html

Write at least one non-trivial test for each function that you implement. Write
these in a new module, `tests_student.rs`, and declare the module in `lib.rs`.

## Part 02: Basic Functions

For consistency, we ask that you put each problem into its own file, and name
the file as `problemN.rs`. Remember to declare each module at the top of
`lib.rs` as well!

### Problem 01: Vector & Slice Manipulation

Create a new module in your library named `problem1` by starting `problem1.rs`.

Complete the following three functions. Note that all of these functions take
their arguments by reference, rather than by value.

Don't use any of the standard library methods on the `Vec` class which implement
the target behavior, since using them would defeat the point of this exercise
:). (However, basic functions such as `contains()` and `push()` are fine).

```rust
/// Computes the sum of all elements in the input i32 slice named `slice`
pub fn sum(slice: &[i32]) -> i32 {
    // TODO
    unimplemented!();
}

/// Deduplicates items in the input vector `vs`. Produces a vector containing
/// the first instance of each distinct element of `vs`, preserving the
/// original order.
pub fn dedup(vs: &Vec<i32>) -> Vec<i32> {
    // TODO
    unimplemented!();
}

/// Filters a vector `vs` using a predicate `pred` (a function from `i32` to
/// `bool`). Returns a new vector containing only elements that satisfy `pred`.
pub fn filter(vs: &Vec<i32>, pred: &Fn(i32) -> bool) -> Vec<i32> {
    // TODO
    unimplemented!();
}
```

### Problem 02: Sieve of Eratosthenes

Create a new module in your library named `problem2` inside `problem2.rs`.

The [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
is used to find all primes below some given number (`n`), and is an efficient
way to find small primes.

Iterate through the numbers from `2` to `n`. For each number `i`:

1. If `i` has been crossed-out from previous iterations, skip it.
2. If `i` isn't crossed-out yet, then it is prime.
   Cross-out all multiples of `i` from `i*i` to `n`. These are non-prime.

Head over to the Wikipedia page for a more detailed description and some fun
animated examples.

Your function takes a number, `n`, and return a list of all prime numbers
less than `n`. (Notice that you can't use a Rust array in this case, since you
don't know the length at compile time.)

```rust
/// Find all prime numbers less than `n`.
/// For example, `sieve(7)` should return `[2, 3, 5]`
pub fn sieve(n: u32) -> Vec<u32> {
    // TODO
    unimplemented!();
}
```

### Problem 03: Towers of Hanoi

[The Towers of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi) is a
classical mathematics and computer science puzzle. Imagine you have three pegs,
one of which holds a stack of discs in increasing size from bottom to top. Your
goal is to move all of the discs from the first peg to the third peg, using the
second peg as an intermediate. Your only restrictions are that you may only
move one disc at a time, and a disc may only be placed on a disc larger than it
(or on an empty peg).

Check out the Wikipedia page (linked above) for algorithm details and snazzy
animations.

This function will take in a number of discs, and the names of the three
pegs, and return a vector of `(u8, u8)` tuples. Each tuple represents one move
between (source, destination).

```rust
/// Solves for the sequence of moves required to move all discs from peg 1 to
/// peg 3, using peg 2 as an intermediary.
pub fn hanoi(num_discs: u32) -> Vec<(u8, u8)> {
    // TODO
    unimplemented!();
}
```

### Problem 04: Bloom Filter

A [bloom filter](https://en.wikipedia.org/wiki/Bloom_filter) is a probabilistic
data structure which tests whether a given element is part of a set. It is meant
to be space efficient, so it does not simply store each member of the set
and compare against a list when queried. Given a value to query, bloom filters
can only return one of two results: _probably_ in the set, and definitely not in
the set.

The algorithm is simple:

First, initialize a fixed size boolean array for storage and *k* different hash
functions for testing.

For each element to insert:

1. Find the hash of the element with using each of your *k* hash functions.
2. Set the bools at those locations in your array to be *true*.

To test if an element is in the set:

1. Find the hash of the element with using each of your *k* hash functions.
2. If *all* of the hash locations are *true*, return "probably true". Otherwise,
   return "definitely false."

Given this structure, it's possible to return false positives but not false
negatives.

* You may assume that the `&str`s only contains ASCII characters, so you can
  safely convert each character to `u8` by casting with `as u8`. You can also
  use the `&str` functon `as_bytes()`.

* Use a storage array of size 20. (This is impractically small for more than a
  few elements, but good for testing.)

```rust
/// Simulates a bloom filter by accepting an array of three hash functions, a
/// data vector, and another value to query. Returns `true` if `value` is
/// "probably" in the data vector and `false` if it is definitely not in the
/// data vector.
fn bloom(data: &Vec<&str>, hashes: [fn(&[u8]) -> u64; 3], value: &str)
        -> bool {
    // TODO
    unimplemented!
}
```

Here are three hash functions to use. Add this code to your `problem4` module.

```rust
fn djb2(bytes: &[u8]) -> u64 {
    let mut hash: u64 = 5381;
    for b in bytes {
        // hash * 33 + c
        hash = (hash.wrapping_shr(5) + hash) + (*b as u64);
    }

    return hash;
}

fn fnv(bytes: &[u8]) -> u64 {
    let mut hash = 0xcbf29ce484222325;
    for b in bytes {
        hash = hash ^ (*b as u64);
        hash = hash.wrapping_mul(0x100000001b3);
    }
    return hash;
}

fn jenkins(bytes: &[u8]) -> u64 {
    let mut hash = 0;
    for b in bytes {
        hash += *b as u64;
        hash += hash.wrapping_shr(10);
        hash ^= hash.wrapping_shl(6);
    }
    hash += hash.wrapping_shr(3);
    hash ^= hash.wrapping_shl(11);
    hash += hash.wrapping_shr(15);
    return hash;
}
```

## Survey

Add a `README.md` file to your repository with the answers to these questions:

1. Introduce yourself! What other languages do you know? What classes have you
   taken (or are taking)? How comfortable are you have with git and using the
   command line?
   (We'll try to keep this class at the right level for people taking it, so
   it's okay if you don't have as much experience.)

2. What are you interested in learning from this class?

3. How hard did you find this assignment?

## Submission

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. **Make sure it is visible on Github!** This is your
submission. Work must be in the master branch at the due time.

Your repository should look like this:

```
[git root]
├── Cargo.toml
└── src
    ├── lib.rs
    ├── problem1.rs
    ├── problem2.rs
    ├── problem3.rs
    ├── problem4.rs
    ├── tests_provided.rs [same as given]
    └── tests_student.rs [your tests]
```

`cargo test` should run all of the tests in your homework. Make sure you have
written at least one test for each function you have written.
