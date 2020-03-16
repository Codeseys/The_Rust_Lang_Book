# Chapter 1

## Installation
I use the Windows Subsystem for Linux for the installation of the rustup and cargo tools.

To install the rustup tool this command was used:
```bash
curl https://sh.rustup.rs -sSf | sh
```
This will downoad the rustup tool and install it. After installing, Rust will be automatically added to the system PATH. However if Rust is not added to the system PATH automatically (which you can test by running the rustup command), then the following command can be used to update the system PATH manually:
```bash
PATH="$HOME/.cargo/bin:$PATH"
```
## Hello, World!
Here we attempt to write the typical programmer's first program: the simple "Hello, World!". 

First we start by making a project directory and navigating into it. 
```bash
mkdir hello_world
cd hello_world
```

Now that we are in the project folder, we start programming in Rust. The basic rule for making a project folder and running Rust programs/projects is that a [main.rs](hello_world/main.rs) file is required. The rustc compiler will then use the main file to make a runnable executable.

In the [main.rs](hello_world/main.rs) file we write this piece of code:
```rust
fn main() {
    println!("Hello, world!");
}
```
The `fn main()` designates the main function entry point for the program (it is always the first code that runs in every executable Rust program). Any parameters to pass to a function are placed in the parentheses: `()`.

The `println!("Hello, world!");` prints the string "Hello, World!" to the console. Rust uses macros and `println!` is a macro which is designated by an exclamation mark: `!`.

After the code is written and saved to the [main.rs](hello_world/main.rs) file the following commands are used to compile and run:
```bash
$ rustc main.rs
$ ./main
Hello, World!
```
**SUCCESS!!!** The typical first program has been written successfully.

**Side Notes:**  Rust is a language that follows the flow of developing compiling and running similar to C and C++. Due to this Rust is a Compile-type language not a scripting language. The reason for why this is explicitly stated will be evident int he next section...

## Hello, Cargo!
Cargo is Rust’s build system and package manager. Which make is an almost essential tool to rapid development and testing. If you have a C/C++ background you know that when writing seperate classes/ADTs (with *.h files) the amount of compile commands increases. But with Cargo the amount of compile commands stays at just 1 (most of the time).

Cargo is somewhat akin to npm from NodeJs which is very helpful in that the tool manages packages for the project and compiles as well (JavaScript does not compile).

First things first, check to see that cargo is installed (it should have been isntalled when rustup was installed if not reinstall rustup):
```bash
$ cargo --version
cargo 1.42.0 (86334295e 2020-01-31)
```
At the moment of writing this I am currently on version 1.42.0 as seen above

### Creating a Project with Cargo
As discussed before, the cargo tool can be used to create projects. The process is much simpler than the way we used to create the [hello_world](hello_world) project.

To create the project, these two commands need to be run:
```bash
$ cargo new hello_cargo
$ cd hello_cargo
```
Cargo has generated two files and one directory for us: a [Cargo.toml](hello_cargo/Cargo.toml) file and a [src](hello_cargo/src) directory with a [main.rs](hello_cargo/src/main.rs) file inside.

The inside of the file [Cargo.toml](hello_cargo/Cargo.toml)