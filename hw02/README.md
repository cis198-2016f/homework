# Homework 2: A Mediocre Binary Search Tree

**Due Wednesday 2016-09-14, 11:59pm.**

For questions, please post on Piazza (Penn students) or Google Groups (other).
Links on homepage.

## Overview

This assignment is modeled after Alexis Beingessner (Gankro)'s [_Learning Rust
With Entirely Too Many Linked Lists_][TML], or for short, "Too Many Lists" (TML).


You should start by reading [Chapter 1][TML] of TML, which is a short
introduction to why linked lists are the worst data structure ever.

We will be implementing a [Binary Search Tree][BST].

[TML]: http://cglab.ca/~abeinges/blah/too-many-lists/book/
[BST]: https://en.wikipedia.org/wiki/Binary_search_tree

#### Classroom for GitHub

We're using Classroom for GitHub, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:

* https://classroom.github.com/assignment-invitations/881fb9d351e47b5f79715aa5df010d5f

## Instructions

Unlike the previous assignments, your Classroom for GitHub repository will be
initialized with a Cargo project. Just clone it to get started. You can see the
[starter code] as its own repo, as well.

Edit `Cargo.toml` to add yourself as the author.

You'll write your implementation in `src/first.rs`. You'll also write tests
in this file. The main file, `lib.rs`, is provided.

Your code doesn't have to follow any exact interface, just the guidelines below.
You'll write the data structure definition, `insert`, `search`, and tests.

[starter code]: https://github.com/cis198-2016f/hw02

## Clippy

[Clippy] is a _compiler plugin_ which runs [lints] on your code, which means
it's user-provided code that runs during the Rust compilation process. Clippy
emits compiler warnings on common general programming errors as well as
Rust-specific errors.

Compiler plugins are only supported on Rust's nightly builds, so we will switch
to nightly for this assignment.

To tell Rustup to compile a particular project with the nightly
toolchain instead of the stable one, `cd` into your project directory and run
`rustup override set nightly`. This will install the most recent nightly
toolchain if it's not already installed, and then override your default
toolchain for this project folder.

You can (and should) instead pin Rustup to a specific nightly version. We will
test your code on `nightly-2016-09-05` (Monday's build). Set this override with:

```
rustup override set nightly-2016-09-05`
```

Clippy has already been enabled for your project in `lib.rs`.

[Clippy]: https://github.com/Manishearth/rust-clippy
[lints]: https://en.wikipedia.org/wiki/Lint_%28software%29

### Implementation

This _roughly_ corresponds to [TML 2]: A Bad Singly-Linked Stack. We recommend
following along with the text for walkthrough on steps to take and general
advice.

Start off by defining your type ([TML 2.1: Layout][TML 2.1]):

[TML 2]: http://cglab.ca/~abeinges/blah/too-many-lists/book/first.html
[TML 2.1]: http://cglab.ca/~abeinges/blah/too-many-lists/book/first-layout.html

* Define a `BST` type: a `pub struct` with a `root` element of type `Link`.
* Define `Link` as an `enum` with two instances: `Empty` and `More`.
  `More` should contain a boxed `Node`.
* Define `Node` as a `struct` containing an `i32` element. Instead of a single
  `next` element, it should also contain two child `Link`s: `left` and `right`.
* Add `#[derive(Debug)]` to the line before each of the three type definitions.
  This allows you to debug-print a value: `println!("{:#?}", bst);`
  which produces multi-line, indented debug prints!

First, implement `BST::new()` which creates an empty BST.

Then, instead of TML's `push` and `pop`, implement:

#### `insert(i32) -> bool`

Insert an element into the BST. Return `true` if successful and `false` if the
element was already in the BST.

* If inserting into an empty link, place the element in this link (making it
  no longer empty).
* If inserting into a non-empty link:
  * return `false` if the element is in this node; otherwise,
  * recurse to the left if the new value is less than the node's value
  * recurse to the right if the new value is greater than the node's value
* You should not end up needing `mem::replace` like in TML.

#### `search(i32) -> bool`

Search for an element in the BST. Return `true` if the element was found and
`false` otherwise.

* If searching an empty link, return `false`; the element can't be found.
* If searching a non-empty link:
  * return `true` if the element is in this node; otherwise,
  * recurse to the left if the target value is less than the node's value
  * recurse to the right if the target value is greater than the node's value

#### Hints

You should need: `struct`, `enum`, `impl`, `if let`/`match`/`ref`/`ref mut`, `Box`,
`&`/`&mut`, dereferencing (`*`).

You should not need: named lifetimes, `Option`, `Vec`, or `use`.

In our reference implementation, `BST::insert` and `BST::search` are short
functions which just call longer (15-30 lines), recursive methods on `Link`.

### Tests

You can run tests with `cargo test`. To show any printed output of successful
tests, use `cargo test -- --nocapture`.

Tests should be defined in the way specified in
[TML 2.6](http://cglab.ca/~abeinges/blah/too-many-lists/book/first-test.html).
This keeps unit tests close to the code that they test. For example,

```rust
#[cfg(test)]
mod test {
    use super::List;

    #[test]
    fn test_push_pop() {
        // ...
    }
}
```

You should test `search` and `insert` in various orders, for both true and
false results. Use `assert!()` and/or `assert_eq!()`. They don't have
to be any more complex than the tests in TML 2.6.

### Submission

Just like before, commit and push your work to the master branch of your
Classroom for Github repository for this HW. ***Make sure it is visible on
Github!*** This is your submission. (Work must be in the master branch at the
due time.)

Your submission should run on `nightly-2016-09-06`.
Your code should not emit any warnings (or errors).
`cargo test` should run all of your tests successfully.
Make sure you have written tests which cover every one of your functions.
