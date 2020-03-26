# Chapter 2 - Programming a Guessing Game

**Objectives:**

 * Learn RUST through a hands-on project
 * Learn the use of `let`, `match`, methods, associated funtions, external crates, etc...
 * Practice the fundamentals

## Setting Up a New Project

As discussed in [Chapter_1](../Chapter_1/README.md#hello-cargo) we can use cargo to make out lives easier when setting up projects:
```
$ cargo new guessing_game --vcs git
```
**NOTE:** The `--vcs git` flag is to make sure that the extra files when compiled is not goint to be pushed to the repository for the sake of saving space and organization.

We have already gone over the files and project structure of a newly generated project so the first step is to verify if all the information in the [Cargo.toml](guessing_game/Cargo.toml) is right. If there is any information that is not correct then edit it to your liking and save it.

Now test compile and run the project using the `cargo run` command. Since we just generated the project the output should look just like the `Hello, world!` project we made in `Chapter_1`.

## Processing a Guess

First step is to ask for user input. We can do this by copying and pasting the following code:
```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```
Now we run through each line to understand each part of the program.

First is the line:
```rust
use std::io;
```
This line "imports" the `io` library into the scope of the program so we can use its functions. This library contains the fucntions we need: reading in user input and printing the result as an output.

The next few lines should be familiar as we went over them in [Chapter_1](../Chapter_1/README.md#hello-world).

## Storing Values with Variables

Next we need to store the user input. We can store this in a string as one would in a typical C/C++ program but we have a feature in Rust that we can use to our advantage: the `let` keyword.

The `let` keyword is what we can use to create a variable instead of declaring a variable of type `string` like in C/C++ it allows the developer to have a certain degree of flexibility in managing variables. However if one so desires the developer can use the syntax to declare a variable with a specific type (This topic will be discussed in [Chapter_3](../Chapter_3/README.md#variables-and-mutability))

The reason for we we can use the keyword `let` without necessarily declaring a specific variable type is because Rust is a type of inferred language meaning that the Rust compiler will automatically infer(know) the type of data we want stored in the variable based on the initial value we assign it. This is known as `type inference`.

Oh geez thats a lot of information for a single topic, but now that helps us understand the following line: 
```rust
let mut guess = String::new();
```
We've just discussed the `let` keyword so now we encounter a new keyword: `mut`

The `mut` keyword is to make the variable we declare mutable. Again this will be discussed in [Chapter_3](../Chapter_3/README.md#variables-and-mutability).

