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

