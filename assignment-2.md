# Assigment 3
---

## Part 1 — Operators for the calculator

```rust
#[derive(Debug, Clone, Copy, PartialEq)]
enum Op { Add, Sub, Mul, Div }
```
---

### (1) - fn from_char(c: char) -> Option<Op>
```rust
impl Op {
    fn from_char(c: char) -> Option<Op> {
        match c {
            //If the 'c' is "+", "-", "*" or "/", it wraps the Add branch of the Op enum around the Some and returns it
            '+' => Some(Op::Add), 
            '-' => Some(Op::Sub),
            '*' => Some(Op::Mul),
            '/' => Some(Op::Div),
            _   => None, // '_' is the wildcard
        }
    }
}
```
---

### (2) - fn symbol(self) -> char
```rust
impl Op {
    fn symbol(self) -> char {
        match self {
            Op::Add => '+',
            Op::Sub => '-',
            Op::Mul => '*',
            Op::Div => '/',
        }
    }
}
```
---

### (3) - fn precedence(self) -> u8 (unsigned 8-bit integer 0-255)
```rust
impl Op {
    fn precedence(self) -> u8 {
        match self {
          // Pratt Parser - Binding Power
            Op::Add | Op::Sub => 1,
            Op::Mul | Op::Div => 2,
        }
    }
}
}
```
---

### (4) - fn apply(self, a: f64, b: f64) -> Option<f64> 
```rust
impl Op {
    // "self" indicates either this is add, sub, mul or div
    fn apply(self, a: f64, b: f64) -> Option<f64> {
        match self {
            Op::Add => Some(a + b),
            Op::Sub => Some(a - b),
            Op::Mul => Some(a * b),
            Op::Div => {
                if b == 0.0 {
                    None // avoiding division-by-zero error
                } else {
                    Some(a / b)
                }
            }
        }
    }
}

```
---

### (5) - fn evaluate_simple(a: f64, op: char, b: f64) -> Option<f64>
```rust
fn evaluate_simple(a: f64, op: char, b: f64) -> Option<f64> {
    Op::from_char(op)?.apply(a, b)
}
```
 
* `Op::from_char(op)`: First, the function we defined in item 1 is called. This returns an `Option<Op>` (Some(Op) or None). <br>

* ?: If an invalid character is provided (such as op = ‘?’), the from_char, it terminates the function and immediately returns None. <br>
If the character is valid (e.g., ‘/’), it opens the Some container and passes the Op::Div value inside to the next step. <br>

* `.apply(a, b)`: The method is called on that opened Op object, and the final `Option<f64>` result is returned.

  
---
### Part 1 — Full Code:

```rust
#[derive(Debug, Clone, Copy, PartialEq)]
enum Op {
    Add,
    Sub,
    Mul,
    Div,
}

impl Op {
    fn from_char(c: char) -> Option<Op> {
        match c {
            '+' => Some(Op::Add),
            '-' => Some(Op::Sub),
            '*' => Some(Op::Mul),
            '/' => Some(Op::Div),
            _ => None,
        }
    }

    fn symbol(self) -> char {
        match self {
            Op::Add => '+',
            Op::Sub => '-',
            Op::Mul => '*',
            Op::Div => '/',
        }
    }

    fn precedence(self) -> u8 {
        match self {
            Op::Add | Op::Sub => 1,
            Op::Mul | Op::Div => 2,
        }
    }

    fn apply(self, a: f64, b: f64) -> Option<f64> {
        match self {
            Op::Add => Some(a + b),
            Op::Sub => Some(a - b),
            Op::Mul => Some(a * b),
            Op::Div => {
                if b == 0.0 {
                    None
                } else {
                    Some(a / b)
                }
            }
        }
    }
}

fn evaluate_simple(a: f64, op: char, b: f64) -> Option<f64> {
    Op::from_char(op)?.apply(a, b)
}
```
---
---

## Part 2 - Operators for the calculator
```rust
#[derive(Debug)]
struct Stack {
    items: Vec<f64>,
}
```
* This structure is a dynamic stack that holds our numbers (f64) sequentially. 
* The stack architecture operates according to the `LIFO` (Last In, First Out) rule.
---

### (6) - new, push and pop
```rust
impl Stack {
    fn new() -> Self {
        Stack {
            items: Vec::new(),
        }
    }

    fn push(&mut self, v: f64) {
        self.items.push(v);
    }

    fn pop(&mut self) -> Option<f64> {
        self.items.pop()
    }
}
```
* fn new() -> Self: Self -  Since it does not take a `Self` parameter, it is an associated function called directly as `Stack::new()`, rather than through an object.

* &mut self: Since both the `push` and `pop` operations modify/update the elements within the stack, it must use a mutable reference (`&mut self`).

* self.items.pop(): The Vec::pop() method in the Rust standard library already returns an Option<T>: It removes an element and returns Some(value) if there is one, or None if the vector is empty.
---

### (7) - peek and len
* peek: Reads the top element of the stack without removing it (Some(&2.0) or None if empty).<br>

* len: Returns the total number of elements in the stack <br>

```rust
impl Stack {
    fn peek(&self) -> Option<&f64> {
        self.items.last()
    }

    fn len(&self) -> usize {
        self.items.len()
    }
}
```
* `&self`: Here, we’re not modifying the stack; we’re just observing its elements. That’s why a read-only reference (&self) is sufficient instead of &mut self. <br>

* `Option<&f64`>: We’re not moving the number off the stack; we’re borrowing a reference to its location in memory. Rust’s `Vec::last()` function does exactly this: it returns a reference to the last element as an `Option<&T>`.
---

### (8-9) - Applying the Operation to the Stack and Rolling Back in Case of an Error
```rust
impl Stack {
    fn apply(&mut self, op: Op) -> Option<()> {
        // 9th Article Rule: Return none if the stack has less than 2 items 
        if self.items.len() < 2 {
            return None;
        }

        // First b, then a is popped
        let b = self.items.pop().unwrap();
        let a = self.items.pop().unwrap();

        match op.apply(a, b) {
            Some(result) => {
                self.items.push(result);
                Some(()) // Success
            }
            None => {
                // Unsuccessfull (like division-zero)
                // Pushing the items back to bring the stack to its original version
                self.items.push(a);
                self.items.push(b);
                None 
            }
        }
    }
}
```
* `Option<()>`: In Rust, empty parentheses are equivalent to `void` in C/Java. The function will not return any meaningful data, but it will indicate whether the operation was successful (Some(())) or failed (None).

* `self.items.push(a); self.items.push(b);`: To satisfy the condition [1.0, 0.0] with Op::Div remains [1.0, 0.0]: We push the numbers we removed due to division by zero back onto the stack in the same order.
---

### (10) - Calculating the Expression (2+3)×4−1 and Popping the Stack
```rust
fn compute_expression() {
    let mut stack = Stack::new();

    // 1. (2 + 3) 
    stack.push(2.0);
    stack.push(3.0);
    stack.apply(Op::Add);

    // 2. * 4 
    stack.push(4.0);
    stack.apply(Op::Mul);

    // 3. - 1 
    stack.push(1.0);
    stack.apply(Op::Sub);

    // Result Check 
    println!("Result: {:?}", stack.peek()); // Some(19.0)

    // Loop to pop the stack 
    while let Some(v) = stack.pop() {
        println!("The value derived from the stack: {v}");
    }
}

```
* `while let` is Rust's pattern-matching loop. As long as `stack.pop()` returns a `Some(v)`, it assigns the number to the variable `v` and enters the loop; as soon as the stack is empty and the `pop()` function returns `None`, it quietly and safely terminates the loop.

---
### Part 2 — Full Code:

```rust
#[derive(Debug)]
struct Stack {
    items: Vec<f64>,
}

impl Stack {
    // 6. Creating a new Stack 
    fn new() -> Self {
        Stack { items: Vec::new() }
    }

    // 6. Adding an item to the Stack 
    fn push(&mut self, v: f64) {
        self.items.push(v);
    }

    // 6. Deriving an item from the Stack 
    fn pop(&mut self) -> Option<f64> {
        self.items.pop()
    }

    // 7. Checking the item on the top (last) 
    fn peek(&self) -> Option<&f64> {
        self.items.last()
    }

    // 7. The number of items 
    fn len(&self) -> usize {
        self.items.len()
    }

    // 8 & 9. Applying (error-proof)
    fn apply(&mut self, op: Op) -> Option<()> {
        if self.items.len() < 2 {
            return None;
        }

        let b = self.items.pop().unwrap();
        let a = self.items.pop().unwrap();

        match op.apply(a, b) {
            Some(result) => {
                self.items.push(result);
                Some(())
            }
            None => {
                self.items.push(a);
                self.items.push(b);
                None
            }
        }
    }
}
```
