# rust notes  

## manage rust and projects

install:  
- `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`  

uninstall:  
- `rustup self uninstall`

update:
- `rustup update`

version:
- `rustc --version`

create new package/library
- `cargo new the_name`

jump into project root folder and show the structure in terminal
- `cd the_name && tree .`

build in debug + run:
- `cargo run`

build in debug:
- `cargo build`

build in release:
- `cargo build --release`

update one package:
- `cargo update -p regex   # updates just “regex”`

run test:
- `cargo test`  to run tests in each of your `src` files and any tests in `tests/`
- `cargo test foo` to run any test with `foo` in its name.
--------------------------------
cargo.toml

```
[dependencies]
regex = "0.1.41"
```  

- add dependencies, before use `use regex::Regex;` inside code

- https://doc.rust-lang.org/cargo/guide/project-layout.html not a static, depends on project structure

- if building an executable/command-line tool/application/a system library with crate-type of staticlib or cdylib, keep Cargo.lock into git

- if building a non-end product, such as a rust library that other rust packages will depend on, put Cargo.lock in your `.gitignore`

---
- CI https://doc.rust-lang.org/cargo/guide/continuous-integration.html

- command lists https://doc.rust-lang.org/cargo/commands/index.html

https://doc.rust-lang.org/rust-by-example/

## rust syntax

`println!` - ends with `!` because it is not common function but macro. The advanced functionality of the rust  

### incode comments
```
// single line comment  
/* Multi
line
comment
*/
```  

### doc comments which are parsed into HTML library documentation:
```
/// library docs for the following item.
//! library docs for the enclosing item.
```

### formatted print


- `format!` - "print" to String variable.  
- `print!` - print to the console (io::stdout).  
- `println!` - `print!`+ newline is appended.  
- `eprint!` - print to the standard error (io::stderr).  
- `eprintln!`- `eprint!` + newline is appended.

Examples:

- `println!("{0}, this is {1}. {1}, this is {0}", "Alice", "Bob");` - positional
- `println!("Base 10:               {}",   69420);` - 69420
- `println!("Base 2 (binary):       {:b}", 69420);` - 10000111100101100
- `println!("Base 8 (octal):        {:o}", 69420);` - 207454
- `println!("Base 16 (hexadecimal): {:x}", 69420);` - 10f2c
- `println!("Base 16 (hexadecimal): {:X}", 69420);` - 10F2C

- `println!("{:#?}", peter);` - `#` pretty print

Also justifiyng available, using extra spaces or zeros before print
https://doc.rust-lang.org/rust-by-example/hello/print.html

### debug

// This structure cannot be printed either with `fmt::Display` or with `fmt::Debug`.  
```
struct UnPrintable(i32);
```

// The `derive` attribute automatically creates the implementation required to make this `struct` printable with `fmt::Debug`.  
```
#[derive(Debug)]
struct DebugPrintable(i32);
```

customize the output appearance by manually implementing `fmt::Display`, which uses the `{}` print marker.  
https://doc.rust-lang.org/rust-by-example/hello/print/print_display.html  