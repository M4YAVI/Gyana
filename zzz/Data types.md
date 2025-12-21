## Rust Data Types Quick Reference

### Scalar Types (Single Values)

|Type|Description|When to Use (99% of the time)|Example|
|---|---|---|---|
|**i32**|32-bit signed integer|Default integer choice - use unless you need specific size/range|`let count: i32 = -42;`|
|**u32**|32-bit unsigned integer|When you know the value is never negative (array indices, counts)|`let age: u32 = 25;`|
|**usize**|Pointer-sized unsigned|Array/vector indexing, collection sizes, memory operations|`let len: usize = vec.len();`|
|**f64**|64-bit float|Default for decimal numbers, scientific calculations|`let pi: f64 = 3.14159;`|
|**bool**|Boolean|Conditions, flags, true/false states|`let is_ready = true;`|
|**char**|Unicode character|Single characters (NOT for strings!)|`let letter = 'A';`|

### String Types

|Type|Description|When to Use (99% of the time)|Example|
|---|---|---|---|
|**String**|Owned, growable string|When you need to modify or own the string data|`let mut name = String::from("Alice");`|
|**&str**|String slice (borrowed)|Function parameters, string literals, read-only access|`let greeting: &str = "Hello";`|

### Collection Types

|Type|Description|When to Use (99% of the time)|Example|
|---|---|---|---|
|**Vec**|Dynamic array|When size changes or unknown at compile time|`let mut nums = vec![1, 2, 3];`|
|**[T; N]**|Fixed-size array|When size is known and never changes|`let rgb: [u8; 3] = [255, 0, 128];`|
|**HashMap<K,V>**|Key-value store|Quick lookups by key, caching, counting|`HashMap::new()`|
|**HashSet**|Unique values only|Removing duplicates, membership testing|`HashSet::new()`|

### Smart Pointers & Wrappers

|Type|Description|When to Use (99% of the time)|Example|
|---|---|---|---|
|**Option**|Value or None|Nullable values, optional parameters, find operations|`Some(42)` or `None`|
|**Result<T,E>**|Success or Error|Any operation that can fail (file I/O, parsing, network)|`Ok(value)` or `Err(error)`|
|**Box**|Heap allocation|Large data, recursive types, trait objects|`Box::new(BigStruct)`|
|**Rc**|Reference counted|Multiple owners, shared read-only data in single thread|`Rc::new(data)`|
|**Arc**|Atomic ref count|Shared data across threads (with Mutex/RwLock)|`Arc::new(Mutex::new(0))`|

### Quick Decision Guide

**"What type should I use for..."**

- **Whole numbers?** → `i32` (negative) or `u32` (positive only)
- **Text?** → `String` (if you own it) or `&str` (if borrowing)
- **List of things?** → `Vec<T>`
- **Maybe has a value?** → `Option<T>`
- **Might fail?** → `Result<T, E>`
- **Counting/indexing?** → `usize`
- **Decimal numbers?** → `f64`
- **Shared between threads?** → `Arc<Mutex<T>>`
- **Fixed size collection?** → Array `[T; N]`
- **Key-value pairs?** → `HashMap<K, V>`


### Common Gotchas to Avoid

1. **Don't use `i8`, `i16`, `u8`, `u16`** unless you're doing systems programming or need exact sizes
2. **Don't use `f32`** unless you have memory constraints - `f64` is almost always better
3. **Don't use `String` for function parameters** - use `&str` instead (more flexible)
4. **Don't use `Box<T>`** just to get around the borrow checker - understand the real issue first
5. **Don't reach for `Rc/Arc`** immediately - often restructuring your code is better
6. **Don't use `&String`** - use `&str` instead (it's more general)
7. **Don't forget `usize` for indexing** - using `i32` for array indices will require casting
8. **Don't use `char` for strings** - `char` is for single characters only
9. **Don't use arrays when you need dynamic sizing** - use `Vec<T>` instead
10. **Don't ignore `Option` and `Result`** - embrace them instead of using sentinel values like -1 or empty strings

### The 80/20 Rule for Rust Types

**80% of your code will use these:**

- `i32` or `u32` for numbers
- `String` and `&str` for text
- `Vec<T>` for collections
- `Option<T>` for nullable values
- `Result<T, E>` for fallible operations

**The other 20% might need:**

- `HashMap` for lookups
- `usize` for indexing
- `f64` for decimals
- `Arc<Mutex<T>>` for concurrent sharing
- Other specialized types

### Pro Tips

- **Start simple**: Use the common types first, optimize later if needed
- **Let type inference help**: Often you can write `let x = 42;` and Rust figures out `i32`
- **Clone is okay initially**: Don't obsess over perfect performance while learning
- **Match your domain**: Use `u32` for age, `i32` for temperature, `f64` for money calculations
- **Read the compiler errors**: Rust's error messages often suggest the right type to use