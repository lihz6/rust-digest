# 1. Getting Started

## 1.1 Installation

### Installing

```bash
$ curl https://sh.rustup.rs -sSf | sh
$ source $HOME/.cargo/env
$ rustc --version
$ cargo --version
```

### Updating

```bash
$ rustup update
```

### Installing

```bash
$ rustup self uninstall
```

### Local documentation

```bash
$ rustup doc
```

## 1.2 Hello, world!

### Creating

```bash
$ mkdir hello_world
$ cd hello_world
$ touch main.ts
```

Filename: main.ts

```rust
fn main() {
    println!("Hello, world!");
}
```

### Compiling

```bash
$ rustc main.ts
```

### Running

```bash
$ ./main
```

## 1.3 Hello, Cargo!

### Creating

```bash
$ cargo new hello_cargo
$ cd hello_cargo
```

Filename: src/main.rs

```rust
fn main() {
    println!("Hello, world!");
}
```

### Building

```bash
$ cargo build
   Compiling hello_cargo v0.1.0 (/Users/lihzhang/Documents/rust-learn/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 1.74s
```

### Running

```bash
$ ./target/debug/hello_cargo
Hello, Cargo!
```

### Build and run

```bash
$ cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.05s
     Running `target/debug/hello_cargo`
Hello, Cargo!
```

### Check without build

```bash
$ cargo check
    Checking hello_cargo v0.1.0 (/Users/lihzhang/Documents/rust-learn/hello_cargo)
    Finished dev [unoptimized + debuginfo] target(s) in 0.18s
```

### Build for release

```bash
$ cargo build --release
   Compiling hello_cargo v0.1.0 (/Users/lihzhang/Documents/rust-learn/hello_cargo)
    Finished release [optimized] target(s) in 0.36s
```

# 2. Programming a Guessing Game

## Use external crates

### Step 1: modify `the Cargo.toml`

```toml
[dependencies]

rand = "0.3.14"
```

> Note: `0.3.14` is shorthand for `^0.3.14`.

### Step 2: build the project

```bash
$ cargo build
    Updating crates.io index
   Compiling libc v0.2.50
   Compiling rand v0.4.6
   Compiling rand v0.3.23
   Compiling guessing_game v0.1.0 (/Users/lihzhang/Documents/rust-learn/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 14.13s
```

> Note: `Cargo.lock` ensures reproducible builds in the future.

### Step 3: update a crate

```bash
$ cargo update
    Updating crates.io index
```

## Complete guessing game code

Filename: src/main.rs

```rust
use std::cmp::Ordering;
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

# 3. Common Programming Concepts

## 3.0 Identifiers

Variables, functions, structs, lots of things, all of these things need identifiers, which can be:

either:

- The first character is a letter.
- The remaining characters are alphanumeric or `_`.

or:

- The first character is `_`.
- The identifier is more than one character. `_` alone is not an identifier.
- The remaining characters are alphanumeric or `_`.

Identifiers cannot be keywords, unless use a “raw identifier”. Raw identifiers start with `r#`:

```rust
let r#fn = "this variable is named 'fn' even though that's a keyword";

// call a function named 'match'
r#match();
```

## 3.1 Variables and Mutability

### Use _snake case_ naming convention

```rust
let max_score = 0;
let last_name = "Lee";
```

### By default variables are immutable

```rust
let x = 5;
// error[E0384]: cannot assign twice to immutable variable `x`
x = 6;
```

### Make variables mutable

```rust
let mut x = 5;
x = 6;
```

### Make a constant

- Constants are always immutable.
- Constants use `const` instead of `let`.
- Constants can be declared in any scope.
- Constants cannot be set at runtime.

```rust
const MAX_POINTS: u32 = 100_000;
```

### Variable shadowing

```rust
let x = "     ";
let x = x.len();
```

## 3.2 Data Types

- Scalar Types
  - Integer Types
  - Floating-Point Types
    - `f64`
      - `let x = 3.0;`
      - `let x = 1.0f64`
      - `let x: f64 = 1.0`
    - `f32`
      - `let x = 1.0f32`
      - `let x: f32 = 1.0`
  - The Boolean Type `bool`
    - `false`
    - `true`
  - The Character Type `char`
    - `'汉'`
- Compound Types
  - Two Primitive Compound Types
    - The Tuple Type
    - The Array Type

### Integer types

| Length | Signed | Unsigned |
| :----: | :----: | :------: |
| 8-bit  |   i8   |    u8    |
| 16-bit |  i16   |   u16    |
| 32-bit |  i32   |   u32    |
| 64-bit |  i64   |   u64    |
| 64-bit |  i128  |   u128   |
|  arch  | isize  |  usize   |

### Integer literals

| Number literals |    Example    |
| :-------------: | :-----------: |
|     Decimal     |   `98_222`    |
|       Hex       |    `0xff`     |
|      Octal      |    `0o77`     |
|     Binary      | `0b1111_0000` |
| Byte(`u8` only) |    `b'A'`     |

### Integer variable declaration

```rust
let x = 32; // i32
let a: u32 = 16;
let b = 32i8;
let c = b'A'; // `u8` only
```

### The tuple type

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
```

```rust
let tup: (i32,) = (500,);
```

```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
```

```rust
let tup = (500, 6.4, 1);
let (x, _, _) = tup;
```

```rust
let x: (i32, f64, u8) = (500, 6.4, 1);
let five_hundred = x.0;
let six_point_four = x.1;
let one = x.2;
```

### The array type

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

```rust
let a = [1, 2, 3, 4, 5];
```

```rust
let a = [1, 2, 3, 4, 5];
let first = a[0];
let second = a[1];
```

## 3.3 How Functions work

### How blocks work

- Cannot declare variables in `mod` level block.
- Blocks always evaluate to the last expression:
  - statement blocks evaluate to `()`, the empty tuple.
  - expression blocks evaluate to non-`()`.
  - loop blocks are supposed to evaluate to `()` due to uncertainty of how the loop terminated, but `loop` blocks can break with a value.
  - `for` loop blocks must evaluate to `()`.
- Blocks can terminate the nearst containing `fn` with the `return` statement.
- Blocks can break out of the nearst containing `loop` with the `break` statement.

### How to use `;` in a block

- All expressions except the last or _block statements_ in a block should end with `;`.
- If the last expression evaluates to `()`, it should end with `;` too.
- Block statements are those evaluating to `()`.
- If an exprssion evaluates to `()`, it should not be assigned to a variable.
- Expressions ending with `;` evaluate to `()`.

### How to use functions

- Use _snake case_ naming convention.
- Must declare parameters type if have any.
- Need to explicitly declare return type unless `()`.
- Always have a return value.

```rust
fn main() {
    println!("{}", abs({ 1 + 3 } * 4)); // 16
    println!("{}", abs(-4)); // 4
}

fn abs(x: i32) -> i32 {
    opp(if x >= 0 {
        return x;
    } else {
        x
    })
}

fn opp(x: i32) -> i32 {
    {
        {
            {
                -x
            }
        }
    }
}

```

## 3.4 Comments

There is only one style comment.

```rust
// So we’re doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what’s going on.
```

## 3.5 Control Flow

### `if` expression

- `if` blocks are expressions.
- `if` condition must evaluate to `bool` type.
- parentheses around `if` condition are omitted.

```rust
let number = 6;

if number % 3 == 0 {
    println!("number is divisible by 3");
} else if number % 2 == 0 {
    println!("number is divisible by 2");
} else {
    println!("number is not divisible by 4, 3, or 2");
}
```

```rust
let condition = true;

let number = if condition {
    5
} else {
    6
};
```

### Repeating Code with `loop`

```rust
loop {
    println!("again!");
}
```

Unlike `while` and `for` mentioned below can be terminated both by condition turning into `false` and by `break` statement, `loop` can only be terminated by `break` statement, so a value can be returned from loops:

```rust
let mut counter = 0;

let result = loop {
    counter += 1;

    if counter == 10 {
        break counter * 2;
    }
};
```

### Conditional Loops with `while`

```rust
let mut number = 3;

while number != 0 {
    println!("{}!", number);

    number = number - 1;
}
```

### Looping Through a Collection with `for`

```rust
let a = [10, 20, 30, 40, 50];

for element in &a {
    println!("the value is: {}", element);
}
```
