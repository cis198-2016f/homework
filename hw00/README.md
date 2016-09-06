# Homework 00: Hello Rust & Hello Cargo!

**Due Wednesday, 2016-09-07, 11:59pm.**

Please ask questions on Piazza (Penn students) or Google Groups (other).
Links on [course homepage].

[course homepage]: cis198-2016f.github.io

#### Classroom for GitHub

We will be using Classroom for GitHub, which creates private repositories for
students. To create your very own private homework repository (owned by us),
click the link on the course Piazza. Don't initialize it just yet! We'll do that
at the end of this assignment.

(Since there's not much to submit for this assignment, this link isn't public.
Future assignments will let anyone create a repository.)

If there is no starter code, such as in this homework, you can use Cargo to
initialize the git repository for you. See below. But first, Rust!

#### Git

If you aren't fmailiar with using Git or you want to brush up, here are a few
good links:

- The [Git Parable][parable]: an article explaining the motivation behind and understanding _why_ git.
- [Learn Git Branching][branch]: an interactive git tutorial
- The [Official Git Book][git-book]: the official git book is a great and easy
  to understand resource for learning git.

[git-book]: https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control
[branch]: http://learngitbranching.js.org/
[parable]: http://tom.preston-werner.com/2009/05/19/the-git-parable.html

## Installing Rust

For this homework, you'll install the Rust compiler (rustc) and the Rust package
manager (Cargo). We'll be using Rust v1.11, the current stable version.

We will be using [rustup.rs], a tool which manages multiple Rust toolchains
on your system. It supports Linux, OS X, and Windows.

If you don't want to install Rustup on your own OS, you can install it on a
virtual machine instead. CIS196 has set up a vm image which has some common
tools installed; instructions are [available here][vm]. You still have to
install Rust on your own (but it's easy!)

[vm]: https://www.seas.upenn.edu/~cis196/VM/

Install [rustup.rs] by following the instructions on the website. It will
automatically download the Stable toolchain and set that as your default.

[rustup.rs]: https://www.rustup.rs/

To check that Rust and Cargo are installed properly, run these commands,
and check that the output matches below:

```
$ rustc --version
rustc 1.11.0 (9b21dcd6a 2016-08-15)
$ cargo --version
cargo 0.13.0-nightly (88e46e9 2016-06-26)
```

Awesome! Rust is ready to roll.

## Hello, Rust!

We're ready to start with our standard Hello World program!
Create a file named `main.rs` and modify the code snippet below to print out
"Hello, \<your name\>!".

```rust
fn main() {
    let name = "Ferris"; // Ferris is the name of Rust's unofficial crustacean mascot
    println!("Hello, {}!", name);
}
```

Then, compile your program: `rustc main.rs`. Run the resulting `main` binary to
test it (`./main` on Unix and `main.exe` on Windows). Boom! You did it! You're a
Rust programmer now! 🎊🎉👍

## Hello, Cargo!

Rust has a fantastic package and build manager, Cargo. Cargo handles all the gory build
automation and dependency management details for you, so you don't have to
worry about it when creating (or building or updating) a project.

Using `rustc` directly is fine for small projects, but Cargo really removes a
lot of the friction of manual project management. If you've ever used `rake` or
`pip` for dependency management, Cargo is like those, plus all of the build
power provided by a good `Makefile`.

To make a new project for homework 0, run the command
`cargo new --bin hw00` (which will *create* the folder `hw00`).

We are going to create an executable program, so we pass to Cargo the flag
`--bin`. (If we wanted to write a library, we would omit this flag.)

Cargo creates this directory structure for you:

```
hw00
├── Cargo.toml
└── src
    └── main.rs
```

You may notice that the `main.rs` file Cargo creates looks suspiciously
similar to the "Hello World" code above. Since you've already written your
"Hello World" program, move the file you created into `src`, overwriting
the `main.rs` that Cargo generated.

To finish things off, build your project: run `cargo build` from any
directory in the project tree. You can run your executable with `cargo run`.
That's it!

## Bonus: Configuration

Adding to your personal environment setup is one of the many joys of starting
a new programming language.

Rust provides a [couple of official config files][configs.md], including Vim,
Emacs, and Sublime. If you aren't sure what to use, I highly suggest trying vim!

  [configs.md]: https://github.com/rust-lang/rust/blob/master/src/etc/CONFIGS.md

## Submission

`cargo new` creates a git repository (with `.gitignore`) if you aren't in an
existing repo. To add your submission repository as a git remote:

```
git remote add origin git@github.com:cis198-2016f/hw00-<username>.git
git push -u origin master
```

(If you're not using SSH for GitHub, you will need to use the HTTPS URL instead.
I also highly recommend generating an SSH key and using it to authenticate with
GitHub -- [instructions here][src].)

[ssh]: https://help.github.com/articles/generating-an-ssh-key/

Commit and push your work to the master branch of your Classroom for Github
repository for this HW. **Make sure it is visible on Github!** This is your
submission. Work must be in the master branch at the due time.

That's it! ok bye get outta here :point_right:
