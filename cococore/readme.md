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

`#![allow(dead_code)]` // no warnings for unused code. Inject at the beginning of the file

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

### Primitives/types  
https://doc.rust-lang.org/rust-by-example/primitives.html

### Literals/operators  
https://doc.rust-lang.org/rust-by-example/primitives/literals.html

`+ - && || !true & | ^ << >> -2.5e-3`
readability `10_000` is `10000`

### variables

```rust
let cant_change = 3;  

let mut change_me = 4;  
change_me = 5;
```

### tuples
```rust
let tu = ("a", 3, true); //access by index tu.0; tu.1; tu.2;
let (one, two, three) = tu // distructing to create bindings
```

### arrays

```rust
let xs: [i32; 5] = [1, 2, 3, 4, 5];
let ys: [i32; 500] = [0; 123]; // fill x500 elements by 123
xs[i] // access
xs.len() // length
f(&xs[1..4]) // send into function as slice [2,3,4,5]
```

### structures

```rust
struct Unit; //field-less (for generics).
struct Pair(i32, f32); // A tuple struct
struct Point {
    x: f32,
    y: f32,
} // A struct with two fields
```

### enums

```rust
enum WebEvent {
Move, // unit-like
Jump,
// like tuple structs,
KeyPress(char), //tuple structs
Paste(String),
Click { x: i64, y: i64 }, //c-like structures
}

type WE = WebEvent; // to use short name
...
let action = WE::Jump;
```

`use`

```rust
enum Status {
    Rich,
    Poor,
}

enum Work {
    Civilian,
    Soldier,
}

use crate::Status::{Poor, Rich}; // manual scoping.
use crate::Work::*; // auto scoping for each name inside.
// now can use in code as Rich, Poor, Civilian, Soldier
```

### constants

```rust
const cant_change_value: i32 = 10;
static possibly_mutable_value: &str = "rust"
```

### variables

```rust
let mut my_number = 11u8 // can be changed, because of mut
let mut my_number:u8 = 11 // same as above, but another syntax
let _avoid_warning_if_not_used = "because name started from underscore"
let warning_if_not_used = "common behavior"
```

### types

use `as` keyword to cast into type
```rust
let decimal = 65.4321_f32;
let integer = decimal as u8
```

### convertion

https://doc.rust-lang.org/rust-by-example/conversion/from_into.html  

### flow of control

`loop`
```rust
if n < 0 && (magic1 || magic2) {} else if n > 0 {} else {}
'outer:loop{
  'inner:loop{ // infinite loop can be without label 'inner
    break 'outer; // stop both loops
  }
}

```

Returning from loop  
https://doc.rust-lang.org/rust-by-example/flow_control/loop/return.html  

`while`

```rust
while n < 101 {n+=1}
```

`for`

```rust
for n in 1..101 {/* 1..100 */}
for n in 1..=100 {/* 1..100 */}
let names = vec!["one", "there", "fool"];
for name in names.iter() {} // after ends, "names" can be printed
for name in names.into_iter() {}// names can not be printed after end
for name in names.iter_mut() { // can modify on place, can be printed after end
  *name = "wonderful"
}
```

`match`

```rust
match number {
  1 => println!("single case"),
  2 | 11 => println!("multicase"),
  13..=19 => println!("[13..19] range"),
  _ => println!("if nothing fires before"),
    }
```

destructing using `match`  
https://doc.rust-lang.org/rust-by-example/flow_control/match/destructuring.html

`if let`  
https://doc.rust-lang.org/rust-by-example/flow_control/if_let.html  

`let else`  
https://doc.rust-lang.org/rust-by-example/flow_control/let_else.html  

### functions

```rust
// common
fn my_f(i:u8, s:str) -> (str, bool){}
// closure
let closure_annotated = |i: i32| -> i32 { i + outer_var };
let closure_inferred  = |i     |          i + outer_var  ;
```

`closure input functions`  
https://doc.rust-lang.org/rust-by-example/fn/closures/input_functions.html

`closure as output parameters`  
https://doc.rust-lang.org/rust-by-example/fn/closures/output_parameters.html

debug print macros `dbg!`  
```rust
let x = Some(1);
dbg!(x); // [src/main.rs:3] x = Some(1)
let y = dbg!(x.unwrap()) + 6; // x.unwrap() = 1
dbg!(y); // y = 7
```