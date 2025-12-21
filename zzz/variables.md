The first big thing to wrap your head around is that in Rust, variables are "immutable" by default. This means once you give a variable a value, it’s locked in forever like it's carved in stone. If you want to change it later, you have to explicitly label it with `mut`. Think of it like a safety lock on a gun; Rust assumes everything is locked until you intentionally flip the switch to make it "mutable." You’ll see this when you create a new string to hold the user's guess.

```rust
use std::cmp::Ordering;
use std::io;

use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

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
