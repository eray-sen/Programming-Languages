# Assigment 1
## Part 1 — Fix the borrow checker



*Each program below is rejected by the compiler. Read the full error message (rustc --explain E0382 etc.), then fix the program without simply adding .clone() everywhere. Explain the fix in a comment.*

---

### (a)
```rust
fn print_len(s: String) { println!("{}", s.len()); }
fn main() {
    let s = String::from("hello");
    print_len(s);
    println!("{s}");
}
```
* The reason for the **E0382: borrow of moved value** error was that print_len() function moved the ownership of variable `s`. Since the value was moved, the original variable became invalid.<br>
* If I pass a reference `(&s)` to the function instead of the value itself, it allows the function to read the data without taking ownership(borrowing).
So **s** remains valid in the main function.

#### Fixed Code:
```rust
fn print_len(s: String) { println!("{}", s.len()); } 
fn main() { 
    let s = String::from("hello"); 
    print_len(&s);
    println!("{s}");
}
```

### (b)
```rust
fn main() {
    let mut v = vec![1, 2, 3];
    let first = &v[0];
    v.push(4);
    println!("first = {first}");
}
```
* The reason for the **E0502: cannot borrow as mutable because it is also borrowed as immutable** error was that the program tried to give the variable `v` both a mutable and an immutable reference. Rust strictly forbids this because `v.push(4)` might force the vector to resize and move to a new location in memory (reallocation). <br>
* If you must use a reference, use it before modifying the vector. Once first is printed, its lifespan ends, and the vector becomes safe to modify.
#### Fixed Code:
```rust
fn main() {
    let mut v = vec![1, 2, 3];
    let first = &v[0]; //immutable borrow starts
    println!("first = {first}"); // immutable borrow ends here after usage
    v.push(4); //it is safe to modify the vector now
}
```

### (f) 
```rust
fn shout(s: String) -> String { s.to_uppercase() }
fn main() {
    let name = String::from("ada");
    let r = &name;
    let loud = shout(&name);
    println!("{r} {loud}");
}
```
* The reason for the **E0505: cannot move out of `name` because it is borrowed** error was that the `shout` function took the ownership of the `name` variable while it was immutably borrowed by `r`. We cannot move a variable before its borrowing ends.

#### Fixed Code:
```rust
fn shout(s: String) -> String { s.to_uppercase() }
fn main() {
    let name = String::from("ada");
    let r = &name;
    let loud = shout(name);
    println!("{r} {loud}");
}
```

### (g) 
```rust
fn main() {
    let mut scores = vec![10, 20, 30];
    for s in &scores {
        if *s > 15 {
            scores.push(*s);
        }
    }
    println!("{scores:?}");
}
```
* The reason for the **E0502: cannot borrow `scores` as mutable because it is also borrowed as immutable** error was that for loop creates an immutable borrow `(&scores)` to iterate over the vector. While this borrow is still active inside the loop, the `scores.push(*s)` line tries to mutably borrow `(&mut scores)` the exact same vector.
* The way not to get this error is using a temproary array to collect the  values that satisfy the statement and then add it to the main vector:
#### Fixed Code:
```rust
fn main() {
    let mut scores = vec![10, 20, 30];
    let mut temp_vec = vec![]; // temp vector

    for s in &scores {
        if *s > 15 {
            temp_vec.push(*s); 
        }
    }
    scores.extend(temp_vec); //updating the scores after borrowing ends.
    println!("{scores:?}");
}
```

### (h) 
```rust
struct Counter { value: i32 }
impl Counter {
    fn bump(&mut self) -> i32 {
        self.value += 1;
        self.value
    }
}
fn main() {
    let mut c = Counter { value: 0 };
    let a = &mut c;
    let b = &mut c;
    println!("{} {}", a.bump(), b.bump());
}
```
 
* The reason for the **E0499: cannot borrow `c` as mutable more than once at a time** is that we are trying to create a second mutable borrow `b` on the variable `c` while the first mutable borrow `a` is still active. But we can only have one active mutable borrow (&mut) at a time for any piece of data to prevent data races.
* The solution is to eliminate the references and use the `c` variable directly.
#### Fixed Code: 
```rust
struct Counter { value: i32 }
impl Counter {
    fn bump(&mut self) -> i32 {
        self.value += 1;
        self.value
    }
}
fn main() {
    let mut c = Counter { value: 0 };
    println!("{} {}", c.bump(), c.bump());
}
```


## Part 2 — Strings and slices

*String is an owned, growable, heap-allocated UTF-8 buffer. &str is a borrowed view into UTF-8 data (part of a String, or a literal). Functions that only read text should take &str. Then they accept both &String and literals.*

### 1)
*fn first_word(s: &str) -> &str — the text up to the first space, or the whole string. first_word("hello world") == "hello", first_word("single") == "single"*
```rust
fn first_word(s: &str) -> &str {
    s.split(' ').next().unwrap_or(s)
}

fn main() {
    println!("{}", first_word("hello world")); 
    println!("{}", first_word("single"));      
}
```
* 1.  s.split(' '): Splits the string eat the first space into pieces as array elements.
* 2. .next(): Takes the first piece of the elements.
* 3. .unwrap_or(s): In case the string is without a space, it returns the original string.

### 2)
*fn count_vowels(s: &str) -> usize — count aeiou, ignoring case. Use s.chars().*
```rust
fn count_vowels(s: &str) -> usize {
    let mut count = 0; //counter

    // s.chars() -> Break the text down into its individual letters.
    for c in s.chars() {
        // .to_ascii_lowercase() -> (A -> a, E->e etc)
        let lowercase_char = c.to_ascii_lowercase();

        if lowercase_char == 'a' || lowercase_char == 'e' || lowercase_char == 'i' || lowercase_char == 'o' || lowercase_char == 'u' {
            count += 1;
        }
    }
    count // returning "count" as the output.
}
```

### 3)
*fn reverse_words(s: &str) -> String — "the quick brown fox" → "fox brown quick the". Use split_whitespace.*
```rust
fn reverse_words(s: &str) -> String {
    let mut words: Vec<&str> = s.split_whitespace().collect();
    words.reverse(); //reversing

    words.join(" ") //combining words into a single string of characters with spaces between the words
}

```
### 4)
*fn capitalize(s: &str) -> String — upper-case the first character: "rust" → "Rust", "" → "".*
```rust
fn capitalize(s: &str) -> String {
    // if the string is empty, return an empty String
    if s.is_empty() {
        return String::new();
    }

    // getting the first character and convert it to uppercase
    let first = s.chars().next().unwrap().to_uppercase();
    
    // taking a slice of the remaining characters from index 1 to the end
    let remainder = &s[1..]; 

    format!("{}{}", first, remainder) //combining them
}

fn main() {
    println!("{}", capitalize("rust")); 
    println!("{}", capitalize(""));      
}

```
### 5)
*fn is_palindrome(s: &str) -> bool — ignore case and anything that is not alphanumeric: is_palindrome("A man, a plan, a canal: Panama") is true*
```rust
fn is_palindrome(s: &str) -> bool {
    let cleaned: String = s.chars()
        .filter(|c| c.is_alphanumeric())   // only letters or digits 
        .map(|c| c.to_ascii_lowercase())   // converting all to lowercase
        .collect();                          

    cleaned == cleaned.chars().rev().collect::<String>()
}

fn main() {
    println!("{}", is_palindrome("A man, a plan, a canal: Panama")); // true
    println!("{}", is_palindrome("race a car"));  // false
}


```

