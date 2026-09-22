# My Rust Journey - Part 1

## Background

While bash scripting is my first programming experience, I consider Python to be my first programming language.
Writing Python programs was the first time I felt like I actually understood programming, and in the years since I've picked it up, I've become fairly adept with it.
Not contribute-to-the-source-code adept, but enough that I can always draft an initial plan of how to tackle a problem with Python.
(Whether that plan will work as initially written is another story.)

As I've delved deeper into research computing, however, I find myself reaching for a statically compiled language.
While most of my Python projects have not needed the performance boost of a compiled language, I have a desire to have the tool available in my arsenal for when it is useful.
That combined with the transitioning of the FOSS community towards Rust, and most of my favorite CLI tools being written in Rust, leads me to wanting to learn how to program with it.

Which brings us to now. 

## Getting started

Most of what I've found online so far has mentioned two resources:

1. **"The Book"** - the [free, official guide to Rust](https://doc.rust-lang.org/book/title-page.html).
2. **Rustlings** - an [interactive set of exercises](https://rustlings.rust-lang.org/) where you troubleshoot broken Rust code.

After a brief skim of the first few pages of "The Book", I decided that the Rustlings approach is more suited to my current mood.

### Installing Rust

There was a brief confusion at first in how to install Rust. 
I'd known about `cargo`, and had assumed that was the all-in-one program for working with Rust.
It seems, however, that is not the case. 

Instead, one starts with the `rustup` script, which installs what you need to run Rust, including the `cargo` program.

On my Ubuntu 24.04 system, I ran this:

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

per [the instructions](https://doc.rust-lang.org/book/ch01-01-installation.html).

This showed some information about what the script was going to modify in my shell environment.
To proceed, I had to select whether I wanted the default installation or to customize it, or to cancel.

I pressed `enter` to select the default installation.

Apparently, I had already installed Rust on my system; it seems that I've walked this path previously, though I've forgotten.
The installer proceeded just fine, however, installing the latest version of its programs.

I activated the environment in my current shell by running

```bash
. "$HOME/.cargo/env"
```

Probably unnecessary, since I had previously had Rust installed, but it shouldn't hurt anything.

Installing `rustup` also brings along `cargo`, which means if nothing else, I'm able to install Rust programs I find with `cargo install` commands.
That's a pretty good consolation prize if I don't stick with Rust for the long-haul. 

### Installing Rustlings

With Rust (re)installed, I'm ready to install Rustlings.

I confirmed that `cargo` was indeed installed by running `which cargo`.

Then I followed the Rustlings' instructions.

```bash
cargo install rustlings
```

This took less than a minute on my system.

Then, after navigating to my desired location my filesystem, I ran

```bash
rustlings init
```

followed by pressing enter to create the exercises directory.

I next moved into the directory with

```bash
cd rustlings
```

and entered

```bash
rustlings
```

to get started.

### Editor

I'm a longtime `vim` user, but in the last couple years I've been learning to use the [Helix editor](helix-editor.com). 
I won't get into the full details here, but I've found that I enjoy using Helix, though I still enjoy using `vim`.
It's been kind of weird - when I'm using `vim`, I'll catch myself using Helix patterns, and while I'm using Helix, I'll catch myself using `vim` patterns.

Anyways, one of the nice things about Helix is the built-in support for Language Server Protocol (LSP) and language formatters.
Helix will autodetect LSPs, providing support for type-hinting, syntax error highlighting, and more.
The language formatter will autoformat the document when you save it.

For working with Rust in Helix, I installed [rust-analyzer](https://github.com/rust-lang/rust-analyzer).

I installed `rust-analyzer` by running

```bash
rustup component add rust-analyzer
```

As for the formatter, there is the `rustfmt` component, which apparently hitched a ride with something I installed earlier, because

```bash
rustup component add rustfmt
```

just reported that the component was already up to date.

Checking the status of the LSP and formatter with

```bash
hx --health rust
```

shows green for the LSP, but says that there is no configured formatter.

But, supposedly, this is [a bug in Helix](https://github.com/helix-editor/helix/discussions/14014), and the formatter should work as expected. 

## Learning with Rustlings

Rustlings works by watching the exercise files, and compiling them automatically when there has been a change.
So the idea is to have a second terminal open where you make the changes, save them, and then see if you were correct in the Rustlings terminal.

Once you successfully fix the broken exercises, you tell Rustlings to move to the next exercise.
All the while, you are able to ask it for hints on the current exercise.

The very first exercise is automatically completed once you acknowledge the introductory message.
That being said, it's worth reading the contents of the first exercise, at `excercises/00_intro/intro1.rs`, as that gives a clue on how to handle the second exercise.

If, like me, you find yourself passing an exercise by making a change you don't fully understand, checkout the corresponding solution at `solutions/SAME_PATH_AS_EXERCISE`.
(The solution will not populate, however, until you pass the exercise.)

I won't do a blow-by-blow account of how to work through the Rustlings exercises, as that would defeat the point of learning it for yourself.

Moving forward, I'll only describe exercises that I found challenging, or provide commentary on things that I find interesting from a "meta" point of view.

### Error explanations

The first thing that stood out to me as I started working through the exercises were the statements like this:

```
For more information about this error, try `rustc --explain ERROR_CODE`
```

Running the specified command gives examples of what may cause that kind of error.

On one hand, this is pretty neat, especially in comparison to Exceptions in Python.
On the other hand, the first time I tried using this command, the examples it described skipped over the fundamentally simple error that the exercise was highlighting.

This feels like a pretty good UX design. 
I'm concerned, though, that the content of the explanation does not always highlight the "obvious simple" mistake.

### Command terminations

New to me, coming from Python, is the need for the command terminator `;`.
That in itself isn't a huge deal; from the brief glimpses I've had of non-Python programming languages, this is a common motif.

Oddly, though, Rust is okay with omitting the terminator `;`, but only for the final command in the file.
A program of

```rust
println!("First line");
println!("Second line");
```

will work, as will

```rust
println!("First line");
println!("Second line")
```

but not

```rust
println!("First line")
println!("Second line")
```

The compile error will helpfully highlight where you missed the `;` terminator, but to me that just begs the question as to why it doens't automatically handle it, if it is able to determine the cause of the error.

I'm guessing that for more complex programs it won't be as obvious where the terminator should go..

### Exercise goals

It seems to me that Rustlings has done a good job of teaching one component at a time.
This may seem like an obvious design goal, but in my experience, it is challenging to implement effectively.

### Types

In Python, I'm used to "type hinting" to help the parsers and type-linters understand the types I intend a variable to have.
In Rust, it seems that this is required, not optional.

### Error explanations II

As I continue the exercises with Rustling, I'm starting to wonder: are the error explanations it shows built into Rust, or is this some feature of the Rustlings program?
I haven't yet tried to compile a Rust program manually, so I don't yet have evidence one way or the other.

Perhaps I've been too traumatized from troubleshooting Python Exceptions, but it feels like these error explanations are "too easy". 

Which leaves me to trying to figure out how to compile a Rust program manually.
On one hand, I know this is how to work with Rust in the first place.
On the other hand, anytime I hear "compile" and "manually" in the same sentence, it makes me hesitant.

### Compiling a Rust program

I've seen a few examples of simple Rust programs at this point. 
It's time I try to do my own.

I remember seeing a "Hello, World!" page [in The Book](https://doc.rust-lang.org/book/ch01-02-hello-world.html), so I'm going to try working through that.

First, I create a test directory separate from the Rustlings directory - I don't know what all is needed, but I'd rather not clutter up that directory tree.

Following the instructions, I create a file `main.rs` and put some simple code in there, based on my learnin's from Rustlings.

```rust title="main.rs"
fn main(num: i32) {
    println!("You've asked me to print: {}", num);
}
```

Having created the file, I now compile it by running

```bash
rustc main.rs
```

Here I encounter an error.
Apparently, the `main` function should never take arguments!
And, more importantly, the error explanation appears just like it did when using Rustlings. 

Let's try again without an argument for `main`.

```rust title="main.rs"
fn main() {
    println!("No arguments, just a simple 'Hello, World!'.");
}
```

Now when I run the `rustc main.rs` command, there's no error.
Instead, I see that a new `main` file has been created in the directory.

When I run the program with

```bash
./main
```

I get the expected output:

```text
No arguments, just a simple 'Hello, World!'.
```

### Command terminations II

As I work through more exercises, it seems that there is a potential utility to the presence or absence of a command terminator `;` for the last command in the code block.
If I'm understanding it correctly, the last command in a function without a command terminator `;` determines the value that the function will return. 

However, trying to apply the `return` syntax of Python doesn't seem to work.
While it seems that Rust recognizes the `return` keyword itself, it's not used in the same way as in Python.

## End of Part 1

I've reached, and passed, the first quiz included in Rustlings.
So far, I'm feeling pretty good.

The error messages are mostly straightforward and easy to troubleshoot.
The syntax isn't too difficult to understand, although I'm still unsure about the command terminators, especially with nested function calls.

Something that is bothering me, though, is the typing (as in data types, not the pressing of keys on the keyboard).

* How do I decide what the type should be for a variable?
* How do I decide between signed and unsigned?
* What is the deal with `char` versus `&str`?

Hopefully as I progress through the exercises, these questions will be answered.

***AWO***

