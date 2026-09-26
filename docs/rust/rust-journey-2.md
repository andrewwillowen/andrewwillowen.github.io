# My Rust Journey - Part 2

***[Read Part 1 here](rust-journey-1.md)***

## Learning with Rustlings, continued

Looks like the first exercises after Quiz 1 are about types, so it seems that I'm going to get my questions answered.

### String types

It took a bit of experimenting, but eventually I realized that apostrophes (`'`) are used to denote a type of `char`,
while quotation marks (`"`) are used to denote a type of `&str`. 

This is a bit jarring, coming from Python, where something wrapped with a pair of either symbol is considered to be a string.
I have a feeling that I have a lot of `mismatched types` errors in my future...

### Unsatisfied trait bounds

I stumbled across the `.join()` method via a type hint; it appeared to be the same as the Python `str` `.join()` method, but with the arguments reversed (the array has the method, and the separator is the argument). But when I tried

```rust
let string_array = ['x'; 101];
let string = string_array.join('_');
```

I got the error E0599 that 

```
the method `join` exists for array `[char; 101]`, but its trait bounds were not satisfied
```

I have no idea what that statement "trait bounds" is referring to, much less what it takes to satisfy it.

Figuring this out was not necessary for the exercise, but it adds to my confusion on arrays.

### References

`move_sematics5` is the first spot where I feel like something is fundamentally different, not just syntatically different.

I think I tried almost every combination of prefixing (or not prefixing) with the ampersand (`&`) before I stumbled across the solution.

Based on the comments, the `&` prefix denotes a reference to a value, but without "taking ownership" of that value. 
I'm guessing that means the value is being "borrowed" and not taken away.

I may need to actually read the [corresponding chapter in The Book](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html) to learn what is going on here.

### References and mutability

While doing some testing to understand the "Variable Scope" section, I re-encountered an error I had seen before: `error[E0382]: use of moved value`. 
In the explanation of the error, I came across the sentence with the key that I was missing:

> ... a value cannot be owned by more than one variable.

In Python, I can do something like this:

```python
>>> a = [1]  # Define a list, assign it to variable 'a'
>>> b = a    # Assign variable 'b' to the value of 'a'
>>>
>>> a.append(2)  # Modify the value of 'a', in place
>>> print(a)
[1, 2]
>>> print(b)
[1, 2]
>>> # 'a' and 'b' have the same value!
>>> b.append(3)
>>> print(a)
[1, 2, 3]
>>> # Doesn't matter whether 'a' or 'b' is used to modify the value
```

In this case, the value of the list is owned by both `a` and by `b`.

This issue arises because the type `list` is an example of a `mutable` type, a data structure that is modified in place.
Other mutable types include `dict` and `set`. 
This is why methods like `copy.deepcopy()` exist - so that the value of the variable is independent of the original variable it was copied from.

Types such as `int` and `str` are immutable and so do not suffer from this problem - they are always copied when a variable is assigned the same value.

!!! tip

    This shared reference problem is covered nicely in the [Memory and Allocation section](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#memory-and-allocation) of Chapter 4 of The Book.

### Functions and ownership

While I now think I understand what is meant by "ownership", the part that is still confusing is why a function can take ownership of a value.
What helped me realize the answer is when I changed the name of the argument in the function definition, to be different than the variable name used when the function was invoked.

Let's say that we have the function `foo`, which takes the argument of `input`. 
Then in main, I define a different variable name and pass it to the function, like so:

```rust title="main.rs"
fn foo(input: String) {
    println!("{input}")
}

fn main() {
    let user_input: String = "Hello!".to_string();

    foo(user_input);
```

Compiling and executing this script yields the print statement, as expected.

```bash
$ rustc -o main main.rs && ./main
Hello!
```

What I realized is that in order for the function `foo` to work, it has to define a value of `input`.
That is, the function definition of `fn foo(input: String)` is actually a shorthand for

```rust
{   // in scope "foo"...
    // ...define argument variables...
    let input = user_input;
    // ...execute body of fn "foo".
}
```

The argument definition in the function call is what transfers ownership of the value.
Then, once the scope of `foo` is over, the memory used to store the value of `input` (formerly owned by `user_input`) is released.

Now, some of you may be thinking "well, yeah...".
For me, at least, this elucidated a misconception about how function calls work. 
In my mind, a function call worked something like this:

```rust
{   // in scope "foo"...
    // ...*replace* all occurrences of variable "input" with the variable "user_input"...
    // ...execute new, temporary body of fn "foo".
}
```

In hindsight, assigning the definition in the function to the value of the passed argument is a much more elegant and efficient approach.

Fixing this misconception also helped me understand why you shouldn't use mutable types as arguments in Python functions.
Empirically, I knew that this leads to the same problem of a shared reference highlighted above, but I did not know *why* that was the case.

Mirroring the Rust example above,

```python title="main.py"
def foo(input: str):
    print(f"{input}")

if __name__ == "__main__":
    user_input: str = "Hello!"
    foo(user_input)
```

the function call is executing something like

```python
(   # in scope "foo"...
    # ...define argument variables...
    input = user_input
    # ... execute body of "foo".
)
```

If `input` is a `list` instead of a `str`, then there will be multiple owners of the same mutable value.

The final piece is that the `String` type in Rust is mutable, which is why the exercise exposed my misconception.

!!! warning

    To be clear, I have not verified that this is actually how function calls work :sweat_smile:.
    But this mental model does seem to help me understand ownership in Rust better. 

### Enums, modules, and hashmaps, oh my

I pushed myself through these exercises at the end of a long day.
While I managed to get past the exercises, I'm not happy with my understanding of the structures and how they function.
I can pick out the places where ownership is playing out, but it still feels too new to have a full grasp.

It's probably a good idea for me to revisit these exercises at another time.

## End of Part 2

I've reached and passed the second quiz in the Rustlings exercises.

I think there were about twice as many exercises for this part as there were for first part.
Plus, they were a bit more difficult to work through.
I definitely feel like I'm learning a new programming language now.

My summary of the ownership model is that the concept of "context managers" in Python has been applied liberally to the fundamental functionality of Rust.
Contexts (more accurately, memory allocations) are constantly being opened and closed, and with it living at the fundamental level of the programming, this leads to safe memory management.

I think I should do some reading of the book to try to understand some of these structures a bit better.

***AWO***

