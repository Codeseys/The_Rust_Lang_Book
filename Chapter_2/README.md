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

### Storing Values with Variables

Next we need to store the user input. We can store this in a string as one would in a typical C/C++ program but we have a feature in Rust that we can use to our advantage: the `let` keyword.

The `let` keyword is what we can use to create a variable instead of declaring a variable with a designated type like in C/C++ it allows the developer to have a certain degree of flexibility in managing variables. However if one so desires the developer can use the syntax to declare a variable with a specific type (This topic will be discussed in [Chapter_3](../Chapter_3/README.md#variables-and-mutability))

The reason for we we can use the keyword `let` without necessarily declaring a specific variable type is because Rust is a type of inferred language meaning that the Rust compiler will automatically infer(know) the type of data we want stored in the variable based on the initial value we assign it. This is known as `type inference`.

Oh geez thats a lot of information for a single topic, but now that helps us understand the following line: 
```rust
let mut guess = String::new();
```
We've just discussed the `let` keyword so now we encounter a new keyword: `mut`

The `mut` keyword is to make the variable we declare mutable. Again this will be discussed in [Chapter_3](../Chapter_3/README.md#variables-and-mutability).

This then leads us to the statement: `let mut guess`. This introduces a mutable variable called `guess` to the scope and binds it to a type string (the result of `String::new`).

The `::` syntax just indicates that the keyword that follows is an _associated function_ of the `String` type. The concept is fairly similar to the use of Abstract Data Types (ADT) in languages like C/C++. The `String` type is provided by the standard library and is UTF-8 encoded.

The `new` **function** is common amongst many data types since its a function to initialize the default state of a data type. Which in this case means that the variable that we declared (`guess`) will be a `String` type initialized with an empty string.

Now onto the next line: 
```rust
io::stdin().read_line(&mut guess)
    .expect("Failed to read line");
```
If you notice that there are **two** different lines here even though I said **line** as a singular, this is because the code block above is technically one line. The code has been formatted to be visually compact reducing the amount of time needed to read the code by just scannign up and down with minimal left to right movement.

Aside from that little caveat; you can see that we use the `io` package that we set to `use` in the line right above the main function. If we didnt write that line above the main function then instead we would have to write 
```rust
std::io::stdin().read_line(&mut guess)
    .expect("Failed to read line");
```
which would have been a couple extra characters you didn't need to write if you ever need to use the `io::stdin` function ever again.

The `.read_line(&mut guess)` part of the line calls the `read_line` function on `stdin` to get input from the user. We are passing a parameter we would like to modify: `&mut guess` which mutable refrence of the variable guess (essentially sends the memory address at which guess is stored at) to the `read_line` function which populates the variable with data from the user input.

The unary operator `&` is part of the pointers system which will be discussed in [Chapter_4](../Chapter_4/README.md#references-and-borrowing). Long story short, by sending the **address** of the variable you can modify what is stored inside and have that modification be present in all scopes in which the address is present in. This is helpful because the Rust language is a pass-by-copy language which means that without using pointers the data sent to a function will only be a copy and any modificaiton will not be reflected elsewhere in any other scope. 

Sending the address of the memory block in which the data is stored would still pass by value but its like the tracking number of a shipment. Every waypoint of the shipment's path will get a copy of the tracking number (address) and the shipment status will change (data). No matter what happens the way to access the shipment status you will always use the same tracking number. I don't know if this concept will translate properly to a concept that can be understood but it helps to play around with the syntax to get a good feel for the concept.

Man this is feeling more and more like a lecture... Onto the next part then!!!

### Handling Potential Failure with the `Result` Type

Another lovely thing about the Rust language compared to the C/C++ language is the error checking. When working with function that you haven't made yourself or haven't thoroughly examined the function that you are going to use, then there may be errors that you won't know the reason for. To solve this dillemma the `expect()` function is used which is part of the `io::Result` library.

The `Result` type(s) is(are) enumeration(s). Enumerations (enums) are types that can have fixed values which are called variants. A simple example would be the boolean type in C (C-lang did not have a boolean type). The boolean can only hold 2 values: true or false. Technically you could use an integer as a boolean but thats just another thing you gotta look out for instead of just looking at a simple thing like true or false. [Chapter_6](../Chapter_6/README.md#defining-an-Enum) will have more information and better example(s).

For `Result` the variants are `Ok` and `Err`. The `Ok` variant tells us that the operation was successful and the result of the operation that we look for is stored in `Ok`. Likewise the `Err` variant tells us that the operation failed and the variant will have the information about how the operation failed.

The purpose of the `Result` types is to encode error-handling info. the `io::Result` library like any other library will have methods defined on them; hence `.expect("Failed to read line");` 

The `expect()` method will handle errors and in the case of `io::Result` returning a `Err` value the `expect()` method will kill the program and print the message that is in the paramters of the method call. In the case of `io::Result` returning a `Err` value the `expect()` method will return the value in the `Ok` value and continue with the rest of the program.

If the `expect()` method isn't used the program will proceed to compile and run but the compiler will spit out a warning that program may not have handled a possible error. 

The correct way to deal with the warning is to do proper error-handling be since this is a simple program an all that is beign checked is the input from the user the `expect()` method is enough.

### Printing Values with `println!` Placeholders

There are very few lines left in the program to talkj about.

Here we talk about formatting our print function:
```rust
println!("You guessed: {}", guess);
```
If you programmed in C/C++ or Python then you would understand the hassle of formatting our output. Luck for us the `println!()` macro makes it easy for us. we just need a pair of curly braces within the predetermined output and the variable right after to print the variable. 

The macro is similar to the `printf()` function in C/C++ and the `format()` function in Python.

There can be multiple variables printed at the same time in the same macro line. All that needs to be done is just adding more curly braces and comma-variable sections after (in this case) `guess`. The variables need to correspond with their curly braces in order.

### Testing the first part

Now let's test the code we have now running the `cargo run` command which should generate the following or similar:
```bash
$ cargo run
   Compiling guessing_game v0.1.0 (~/The_Rust_Lang_Book/Chapter_2/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.59s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
10
You guessed: 10
```
The first part is done now on to the next part...

## Generating a Secret Number

Now we need to generate a random number that the user will try to guess hence the `guessing` part of this game.

### Using a Crate to Get More Functionality

A crate is a package/library/collection of rust source code files. What we have made so far is a _binary crate_. We will use a crate called `rand` to help us in this program. In this case the `rand` crate is a library crate which has code intended for use in other programs as it cannot compile to a runnable binary on its own.

To use the `rand` crate we first need to edit our [Cargo.toml](guessing_game/Cargo.toml) and add a line under the `[dependencies]` section:
```toml
[dependencies]
rand = "*"
```
[The Book](https://doc.rust-lang.org/book) that this repository follows uses the 0.5.5 version of the `rand` crate. We can instead just use and asterisk like above to designate that we want to use the most current version of the crate. The carot character (`^`) can be used to indicate the use of any public API compatible with version `x.x.x`

Now we run the same `cargo run` command as before:
```bash
$ cargo run
    Updating crates.io index
  Downloaded rand v0.7.3
  Downloaded getrandom v0.1.14
  Downloaded rand_chacha v0.2.2
  Downloaded rand_core v0.5.1
  Downloaded libc v0.2.68
  Downloaded cfg-if v0.1.10
  Downloaded ppv-lite86 v0.2.6
   Compiling libc v0.2.68     
   Compiling getrandom v0.1.14
   Compiling cfg-if v0.1.10
   Compiling ppv-lite86 v0.2.6
   Compiling rand_core v0.5.1
   Compiling rand_chacha v0.2.2
   Compiling rand v0.7.3
   Compiling guessing_game v0.1.0 (/mnt/c/Users/bbala/Documents/Mirror/CS/Rust/The_Rust_Lang_Book/Chapter_2/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 37.81s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
10
You guessed: 10
```
you can see that there is a line 
```
Updating crates.io index
```
and all this is saying is that Cargo is fetching the latest version of everything from the registry which is a copy of [Crates.io](https://crates.io/). Crates.io is a open pool of opensource Rust project that others can use.

After updating the registery, Cargo then proceeds to check the `[dependencies]` section and downloads any crates that ahvent been downloaded. Although the only dependency listed is `rand` it picked up `rand-core` and `libc` cause those are the packages that rand uses. After retrieving these packages and compiles them with the dependencies available.

If you were to make a small change in the [main.rs](src/main.rs) file then run the command again, Cargo won't re-download the dependencies because its already downloaded.

### Updating a Crate to Get a New Version

To update the crates to a newer version you can run the `cargo update` command. Keep in mind that the command only updates the version within the `x.x.y` series. Only the `y` changes but if you want to change either of the `x` version denotation then you will have to manually do that.

## Generating a Random Number

Now that we added the `rand` crate to the project let's use it to generate numbers to compare to our guess.

First is to update the [main.rs](src/main.rs) to look like this:
```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```
First, we add a use line: `use rand::Rng`. The `Rng` trait defines methods that random number generators implement, and this trait must be in scope for us to use those methods.

Next we added this line:
```rust
let secret_number = rand::thread_rng().gen_range(1, 101);
```
and all this does is that 