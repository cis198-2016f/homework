# Homework 3: "Iterating" On Your Binary Search Tree

**Due Wednesday 2016-09-21, 11:59pm.**

For questions, please post on Piazza (Penn students) or Google Groups (other).
Links on homepage.

## Overview

This assignment is modeled after Alexis Beingessner (Gankro)'s [_Learning Rust
With Entirely Too Many Linked Lists_][TML] (TML).

[TML]: http://cglab.ca/~abeinges/blah/too-many-lists/book/

This time, you should following along with Chapter 3. We won't do everything in
there, but the content is similar.

You'll modify the data structure, `insert`, `search`, and the tests, and
write the code necessary to turn your BST into various iterator types:
`IntoIter`, `Iter`, and `IterMut`.

#### GitHub Classroom

We're using GitHub Classroom, which manages private homework repositories
for students. To create your very own private homework repository (owned by
us), click this link:

* https://classroom.github.com/assignment-invitations/4f6c6bdb55321eecdc0f16249a320d31

## Instructions

Like in HW2, your GitHub repository will be pre-populated with a Cargo
project. Clone it to get started. You can see the [starter code] as its own
repo, as well.

[starter code]: https://github.com/cis198-2016f/hw02

Edit `Cargo.toml` to add yourself as the author.

Write your implementation and tests in `src/second.rs`. `lib.rs` is provided.

We'll essentially be modifying HW2, so you should copy `first.rs` over as
`second.rs` in HW3.

As before, your code doesn't have to follow an exact interface, but the
function signatures provided will probably work best.

## Clippy

Again like HW2, you should add Clippy to your project. We'll use the same
version of Clippy and Rust nightly that we did last week, so set your project
override:

```
cd hw03
rustup override set nightly-2016-09-05
```

## Implementation

This _roughly_ corresponds to
[TML 3](http://cglab.ca/~abeinges/blah/too-many-lists/book/second.html). We
recommend following along with the text for a walkthrough and general advice.

We will use:

* `Option` and `take` (but *not* `map`).
* Type aliases.
* Generic type parameters (`BST<T>`) with trait bounds (`Ord`).
* Named lifetimes and lifetime bounds on type parameters.
* Traits, trait implementations, and associated types.
* Iterators.

**NOTE:**

[TML 3.1] introduces `map` and anonymous functions and closures. Since we
haven't covered this in class, you can  use `match`es instead of closures
everywhere in this assignment. However, you're welcome to use `map`/closures if
you want the extra practice - simple closures aren't hard.

> Second, `match option { None => None, Some(x) => Some(y) }` is such an
> incredibly common idiom that it was called `map`. `map` takes a function to
> execute on `x` in the `Some(x)` to produce the `y` in `Some(y)`. We could
> write a proper `fn` and pass it to `map`, but we'd much rather write what to
> do *inline*.

### Details

Start by modifying `BST` and `Link` to take a generic element `T` instead of `i32`.

Then, replace the `Link` type with a type alias for `Option<Box<Node<T>>>` (Woo,
angle brackets!). Update all of the code which uses `Link`.

* Since you didn't need to use `mem::replace` in HW2, you won't need `take` yet.

* You will need some kind of trait bound on `T`.

* If you want, take a look through the [`Option` documentation][optdoc] to
  find useful methods.

[optdoc]: https://doc.rust-lang.org/std/option/enum.Option.html

Now that `Link` is a type alias and not a struct, you cannot directly `impl`
methods for it, because you don't own the type `Option`. Work around this by
defining a generic trait `Set<T>` which provides your two functions
`insert(&mut self, e: T) -> bool` and `search(&self, e: T) -> bool`.

Implement `Set<T>` for `Link<T>`, by adapting the functions you wrote for
HW02 to be generic over `T`. Make sure your old tests still pass.

Now you will transform your ordinary BST into an extraordinary BST iterator!
We're going to use the [`Iterator`] trait.

Build yourself an `IntoIter` struct like the one in [TML 3.4].

* Since this is a BST, and not a list, we're going to cheat a bit and just
  iterate over the rightmost edge of the tree. (You can easily implement an
  actual traversal of the tree here, but it gets more complicated when you
  iterate over references.)

* Implement the [`Iterator`] trait for `IntoIter`. This requires an associated
  type, `type Item`, and an implementation of `next(&mut self) ->
  Option<Self::Item>`.

Instead of implementing `BST::into_iter` as a plain member function,
we're going to do something _way_ cooler:

* [`IntoIterator`] is a trait with one method, `into_iter`. This is the
  sugar that fuels Rust's `for` loops. Go ahead and `impl IntoIterator for
  BST`. Don't forget: this trait requires you to declare associated types!
  Read the [docs][`IntoIterator`] for the deets.

[`Iterator`]: https://doc.rust-lang.org/std/iter/trait.Iterator.html
[`IntoIterator`]: https://doc.rust-lang.org/std/iter/trait.IntoIterator.html

BAM! Now your BST can harness the power of `for` loops. Try this:

```rust
let mut bst = BST::new();
bst.insert(1);
bst.insert(2);
bst.insert(3);

for elt in bst { // calls bst.into_iter()
    println!("{}", elt);
}
```

But wait, there's more! What about borrowed iteration? Implement
an `Iter` struct, as described in [TML 3.5] (similar to `IntoIter`).

* Again, instead of implementing `BST::iter`, we're going to  use
  `IntoIterator`. Since this now iterates over references, you need to `impl
  IntoIterator for &BST`.

* Again, iterate over the rightmost edge of the tree.

* You're going to need named lifetimes here! To start, you have to
  implement `impl<'a, T> IntoIterator for &'a BST<T>`.

This is what allows:

```rust
for elt in &bst { // calls (&bst).into_iter()
    println!("{}", elt);
}
```

Finally, we'll move on to [TML 3.6], `IterMut`. You know what you need to do.

```rust
for elt in &mut bst { // calls (&mut bst).into_iter()
    println!("{}", elt);
}
```

[TML 3.1]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-option.html
[TML 3.4]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-into-iter.html
[TML 3.5]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-iter.html
[TML 3.6]: http://cglab.ca/~abeinges/blah/too-many-lists/book/second-iter-mut.html

### Tests

Write tests for all three iterators. You should check two things:

* For a BST, you should be able to compile a for loop over
  `bst`, `&bst`, or `&mut bst`.

* The values returned by the iterator should be correct. You can explictly
  get an iterator (with, for example, `(&mut bst).into_iter()`), then
  `assert_eq!` the values returned by `next()`.

## Submission

Commit and push your work to the master branch of your GitHub Classroom
repository for this HW. Make sure it is visible on Github! This is your
submission. Work must be in the master branch at the due time.

Your submission should run on `nightly-2016-09-06`.
Your code should not emit any warnings (or errors).
`cargo test` should run all of your tests successfully.
Make sure you have written tests which cover every one of your functions.

If you have any comments or feedback on this assignment, include them in the
README of your submission.
