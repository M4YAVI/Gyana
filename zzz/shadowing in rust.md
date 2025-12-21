

Shadowing is when you declare a new variable with the same name as a previous variable, effectively "hiding" the original. Let's break this down completely.

---

## The Core Mechanism

```rust
fn main() {
    let x = 5;          // First binding: x is i32 with value 5
    let x = x + 1;      // Second binding: NEW x shadows the first, value is 6
    let x = x * 2;      // Third binding: NEW x shadows the second, value is 12
    
    println!("{x}");    // Prints: 12
}
```

**What's actually happening:** Each `let x` creates an entirely new variable. The old variable still exists in memory until the scope ends, but it's inaccessible because the name now refers to the new variable.

---

## Shadowing vs Mutability: The Critical Distinction

### Mutability (`mut`)

```rust
fn main() {
    let mut x = 5;
    x = 6;              // Same variable, new value
    x = "hello";        // ❌ COMPILE ERROR: can't change type
}
```

### Shadowing

```rust
fn main() {
    let x = 5;
    let x = "hello";    // ✅ Works! It's a completely new variable
    let x = x.len();    // ✅ Works! Now x is usize
}
```

**Key insight:** Shadowing creates a new variable; mutation modifies an existing one. This is why shadowing allows type changes.

---

## The Five Powers of Shadowing

### 1. Type Transformation

```rust
fn main() {
    let input = "   42   ";           // &str
    let input = input.trim();          // &str (trimmed)
    let input: i32 = input.parse().unwrap();  // i32
    
    // Same semantic meaning "input", but transformed through types
}
```

### 2. Immutability After Mutation

```rust 
fn main() {
    let mut data = Vec::new();
    data.push(1);
    data.push(2);
    data.push(3);
    
    let data = data;    // Shadow with immutable binding
    // data.push(4);    // ❌ Now this would fail - data is immutable
    
    // Rest of your code has guarantee: data won't change
}
```

### 3. Unwrapping Optionals/Results with Same Name
```rust
fn main() {
    let config: Option<String> = Some("production".to_string());
    
    // Instead of config_unwrapped or similar
    let config = config.unwrap_or("development".to_string());
    
    // Clean! config is now just a String
}
```

### 4. Narrowing Scope of Original Value

```rust 
fn main() {
    let connection = expensive_database_connect();
    let result = connection.query("SELECT * FROM users");
    
    let connection = ();  // Shadow with unit type - can't use connection anymore
    // This signals "we're done with the connection concept here"
    
    process(result);
}
```

### 5. Progressive Refinement in Parsing/Validation

```rust
fn process_user_input(raw: &str) {
    let input = raw.trim();
    let input = input.to_lowercase();
    let input = input.replace(" ", "_");
    
    // Each step refines, same semantic name throughout
}
```

---

## Shadowing in Different Scopes

### Inner Scope Shadowing (Temporary)

```rust 
fn main() {
    let x = 5;
    
    {
        let x = 99;         // Shadows only within this block
        println!("{x}");    // Prints: 99
    }
    
    println!("{x}");        // Prints: 5 (original x is back)
}
```

### Same Scope Shadowing (Permanent for that scope)

```rust
fn main() {
    let x = 5;
    let x = 99;             // Shadows for rest of this scope
    
    println!("{x}");        // Prints: 99
    // Original 5 is permanently inaccessible
}
```

---

## Shadowing in Pattern Matching

```rust
fn main() {
    let pair = (Some(5), Some("hello"));
    
    match pair {
        (Some(x), Some(x)) => {  // ❌ ERROR: can't bind same name twice
            println!("{x}");
        }
        _ => {}
    }
}
```


```rust
fn main() {
    let value = Some(Some(42));
    
    // Shadowing works in sequential patterns though:
    if let Some(value) = value {        // value: Option<i32>
        if let Some(value) = value {    // value: i32
            println!("{value}");         // Prints: 42
        }
    }
}
```

---

## Shadowing with References
```rust
fn main() {
    let s = String::from("hello");
    let s = &s;             // s is now &String
    let s = s.as_str();     // s is now &str
    let s = s.to_uppercase(); // s is now String again
}
```



```rust
fn main() {
    let s = String::from("hello");
    let s = &s;  // s: &String, but original String is still owned... by whom?
    
    // Original String exists, borrowed by new s
    // When scope ends, borrow checker still tracks correctly
}
```

---

## When to Use What: The 99% Rules

### ✅ USE SHADOWING When:

**1. You're transforming data through stages (most common use case)**

```rust
// ✅ Perfect shadowing use case
let input = get_raw_input();
let input = input.trim();
let input = validate(input)?;
let input: Config = parse(input)?;
```

**2. You want to change types but keep semantic meaning**

```rust
// ✅ The string "age" becomes the number age
let age = "25";
let age: u32 = age.parse().unwrap();
```

**3. You're unwrapping Option/Result and don't need the wrapped version**

```rust
// ✅ Clean unwrapping
let user = fetch_user(id);      // Option<User>
let user = user?;               // User
```

**4. You want to "freeze" a mutable variable**

```rust
// ✅ Builder pattern completion
let mut builder = StringBuilder::new();
builder.append("hello");
builder.append(" world");
let builder = builder;  // Now immutable, prevent further changes
```

**5. You're in a different scope and need a local override**

```rust
// ✅ Temporary local value
let precision = 2;
{
    let precision = 10;  // Higher precision for this calculation
    complex_calculation(precision);
}
// Back to precision = 2
```

---

### ✅ USE MUTABILITY (`mut`) When:

**1. You're modifying the same data repeatedly (collections, counters)**
```rust
// ✅ Accumulating into same collection
let mut results = Vec::new();
for item in items {
    results.push(process(item));
}
```

**2. You need the variable in a loop that modifies it**

```rust
// ✅ Counter
let mut count = 0;
for _ in items {
    count += 1;
}
```

**3. You're updating state that needs to persist across operations**

```rust
// ✅ Stateful processing
let mut parser = Parser::new(input);
parser.advance();
parser.consume_whitespace();
let token = parser.next_token();
```

**4. Performance-critical code where you can't afford new allocations**
```rust
// ✅ Reusing buffer
let mut buffer = String::with_capacity(1000);
for line in lines {
    buffer.clear();
    buffer.push_str(line);
    process(&buffer);
}
```

---

### ❌ DON'T Shadow When:

**1. You need access to both values**

```rust
// ❌ Bad: You lose access to original
let original = compute_something();
let original = transform(original);
// Can't compare or use original anymore!

// ✅ Better: Use different names
let original = compute_something();
let transformed = transform(&original);
// Can use both
```

**2. It hurts readability (same name, unrelated purposes)**

```rust
// ❌ Confusing: x means completely different things
let x = get_user_id();
// ... 50 lines later ...
let x = calculate_price();  // What?? This is confusing

// ✅ Better: Use descriptive names
let user_id = get_user_id();
let price = calculate_price();
```

**3. In structs/enums (shadowing is for local variables only)**

```rust
struct Foo {
    x: i32,
    x: String,  // ❌ Not allowed - this isn't shadowing
}
```

---

## The Mental Model

Think of variable names as **labels on boxes**:

- **Mutability**: Same box, you can change what's inside
- **Shadowing**: New box, you move the label to the new box; old box still exists but is unlabeled (inaccessible)

> MUTABILITY:
> ┌─────────┐
> │    5    │  ← "x" label
> └─────────┘
>      ↓ x = 6
> ┌─────────┐
> │    6    │  ← same "x" label, same box, new contents
> └─────────┘
> 
> SHADOWING:
> ┌─────────┐
> │    5    │  ← "x" label
> └─────────┘
>      ↓ let x = 6
> ┌─────────┐     ┌─────────┐
> │    5    │     │    6    │  ← "x" label moved here
> └─────────┘     └─────────┘
>   (still exists,    (new box)
>    but unlabeled)

---

## Quick Reference Decision Tree

> Need to modify a value repeatedly in place?
>     → Use mut
> 
> Transforming through stages (type may change)?
>     → Use shadowing
> 
> Building up a collection?
>     → Use mut
> 
> Unwrapping Option/Result, don't need original?
>     → Use shadowing
> 
> Need both old and new values?
>     → Use different names (neither shadowing nor mut helps)
> 
> Want to make a mutable binding immutable?
>     → Shadow it: let x = x;
> 
> Loop counter or accumulator?
>     → Use mut

---

## Edge Cases You Should Know

### Shadowing doesn't affect closures that captured the original

```rust
fn main() {
    let x = 5;
    let closure = || println!("{x}");  // Captures x = 5
    
    let x = 10;  // Shadows x
    
    closure();   // Still prints: 5 (captured the original)
    println!("{x}");  // Prints: 10
}
```

### Shadowing in function parameters

```rust
fn process(data: Option<String>) {
    let data = data.unwrap_or_default();  // ✅ Shadows parameter
    // Now data is String, not Option<String>
}
```

### You cannot shadow in the same `let` statement

```rust
let (x, x) = (1, 2);  // ❌ Error: identifier bound more than once
```

---

You now have complete mastery of shadowing in Rust. The 99% rule: **use shadowing for type transformations and progressive refinement; use `mut` for in-place modifications and accumulation.**