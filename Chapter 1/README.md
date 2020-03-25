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

The inside of the file [Cargo.toml](hello_cargo/Cargo.toml) should look like this:
```toml
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["codeseys"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

If you haven't noticed already, the file format is in TOML which is _Tom’s Obvious, Minimal Language_. IDK who Tom is but thanks for the easy to use and understand formatting.

The first line, `[package]`, is a section heading that indicates that the following statements are configuring a package. As we add more information to this file, we’ll add other sections.

The next 4 lines set the configuration information Cargo needs to compile our program such as the name of the package, the version, the author, and the edition of rust to use. 

The last part of the file should be the `[dependencies]` section. Here is the section where the dependencies of the project can be listed for the compiler to download, compile and package together.

### Building and Running a Cargo Project

Now we look at the difference in building and running between our "Hello, world!" programs. 

In the [hello_cargo](hello_cargo/) directory run this command:
```
$ cargo build
```
And the resulting output should look like this:
```
    Compiling hello_cargo v0.1.0 (~/The_Rust_Lang_Book/Chapter 1/hello_cargo)
     Finished dev [unoptimized + debuginfo] target(s) in 2.55s
```

Now that we have finished building our project we can go into the `/target/debug/` and run the finalized binary generated by cargo.

There is another shortcut that helps with compiling AND running. Instead of compiling first then navigating to the folder to find the binary file and running to test it we can run this command to both compile and run subsequently:
```
$ cargo run
```
The result should look like this:
```
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/hello_cargo`
Hello, world!
```
Another helpful command is `cargo check`. This command just quickly checks the code to see that is compiles right without any errors. This is useful when writing code to check for compile-time errors without continually building and waiting for that to finish to any errors.

## Building for Release

when the project is finally ready to be released for non-dev use use the `cargo build --release` command to make an executable in the filepath `~target/release/` intead of `~target/debug/`. The command compiles the release with optimizations which results in a lengthier compile time. This is the reason for the existence of the `--release` flag. It helps speed up development by keeping compile times fast.

**TIP #1:** To make a cargo project with a .gitignore file use the `--vcs git` flag like so (So that you wont commit the `/target/` directory with the compiled binaries) :
```
$ cargo new <PROJECT_NAME> --vcs git
```
**TIP #2:** If benchmarking code use the release binary not the development binary.

# RECAP

* Use `cargo new <PROJECT NAME>` to make a new project
* Use `cargo run` to compile and run project
* Use `cargo check` or `cargo build` to build the project
* Use `cargo build --release` to build a release binary with optimizations


