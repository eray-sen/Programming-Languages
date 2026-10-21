# Assigment 2 - Vectors, structs, owning data and the VM value stack




## Task 4 — Vectors and slices


### 1 - double_in_place Function
```rust
fn double_in_place(values: &mut [i32]) {
    for x in values.iter_mut() // .iter_mut() to check each element in the array
{
        *x *= 2; // '*' allows us to reach to the referred value 
}
```

### 2 - Largest Function
```rust
fn largest(values: &[i32]) -> Option<i32> {
    if values.is_empty() {
        return None; // return None if the array is empty
    }
    
    let mut max_val = values[0];
    for &x in values.iter() {
        if x > max_val {
            max_val = x;
        }
    }
    Some(max_val) // return the max value in Some
}
```

### 3 - task_4 Vectors
```rust
fn double_in_place(values: &mut [i32]) {
    for x in values.iter_mut() {
        *x *= 2;
    }
}

fn largest(values: &[i32]) -> Option<i32> {
    if values.is_empty() {
        return None;
    }
    
    let mut max_val = values[0];
    for &x in values.iter() {
        if x > max_val {
            max_val = x;
        }
    }
    Some(max_val)
}

fn remove_duplicates(v: &mut Vec<i32>) {
    let mut unique = Vec::new();
    for &x in v.iter() {
        if !unique.contains(&x) {
            unique.push(x);
        }
    }
    *v = unique;
}

fn running_sum(v: &[i32]) -> Vec<i32> {
    let mut sum = 0;
    let mut result = Vec::new();
    for &x in v.iter() {
        sum += x;
        result.push(sum);
    }
    result
}

fn task4_vectors() {
    let mut v = vec![3, 1, 3, 2, 1]; 
    
    remove_duplicates(&mut v); 
    
    double_in_place(&mut v);   
    
    println!("v: {:?}", v); 
    println!("running_sum: {:?}", running_sum(&v)); 
    println!("largest: {:?}", largest(&v));         
}

fn main() {
    task4_vectors();
}     
```
---

## Task 5 — Structs and methods
### All Signatures Together

```rust
#[derive(Debug)]
struct Point {
    x: f64,
    y: f64,
}

#[derive(Debug)]
struct Rectangle {
    top_left: Point,
    width: f64,
    height: f64,
}

impl Rectangle {
    // 1. Area
    fn area(&self) -> f64 {
        self.width * self.height
    }

    // 2. Contains Point?
    fn contains(&self, p: Point) -> bool {
        p.x >= self.top_left.x && p.x <= self.top_left.x + self.width &&
        p.y >= self.top_left.y && p.y <= self.top_left.y + self.height
    }

    // 3. does other rectangle fit? 
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width >= other.width && self.height >= other.height //comparing the sizes
    }

    // 4. Scaling - &mut self as we will change the value!
    fn scale(&mut self, factor: f64) {
        self.width *= factor;
        self.height *= factor;
    }
}

// Test
fn main() {
    let mut rect1 = Rectangle {
        top_left: Point { x: 0.0, y: 0.0 },
        width: 10.0,
        height: 10.0,
    };

    let rect2 = Rectangle {
        top_left: Point { x: 5.0, y: 5.0 },
        width: 5.0,
        height: 5.0,
    };

    let point = Point { x: 5.0, y: 5.0 };
    let other_point = Point { x: 20.0, y: 20.0 };

    println!("rect1 area: {}", rect1.area()); // expecting 100 
    println!("rect1 containts the point?? {}", rect1.contains(point)); //expecting  true 
    println!("rect1 containts the other point? {}", rect1.contains(other_point)); //expecting false 
    
    println!(" can rect1 hold rect2? {}", rect1.can_hold(&rect2)); // expecting true 

    rect1.scale(2.0); 
    println!("area of rect1 after doubling it: {}", rect1.area()); // expecting 400 
}
```
--- 

## Task 6 — Structs and methods
### 1 - Defining the Structs
```rust
#[derive(Debug, Clone)]
struct Book {
    title: String,
    author: String,
    year: u16,
}

#[derive(Debug)]
struct Library {
    books: Vec<Book>,
}
```
### 2 - Book::new - Creating a book 

```rust
impl Book {
    fn new(title: &str, author: &str, year: u16) -> Self {
        Book {
            title: title.to_string(),   // taking the ownership by converting from &str to String
            author: author.to_string(), 
            year,                       // same name
        }
    }
}
```

### 3 - Library::new ve add (Library Management)
```rust
impl Library {
    // Creating an empty library
    fn new() -> Self {
        Library {
            books: Vec::new(), // starting with an empty vector
        }
    }

    // adding a new book to the library
    fn add(&mut self, book: Book) {
        // move semantic -> the ownership of "book" is on the function now.
        self.books.push(book); // the ownership goes to the vector 
    }
}
```
 

### 4 - task6_library
```rust
fn main() {
    let mut my_library = Library::new(); // mut as we will modify it 

    let book1 = Book::new("1984", "George Orwell", 1949);
    let book2 = Book::new("Dune", "Frank Herbert", 1965);

    my_library.add(book1);
    my_library.add(book2); // moving the ownerships while adding the books to the library

    println!("The books in my library: {:#?}", my_library); 
}
```

