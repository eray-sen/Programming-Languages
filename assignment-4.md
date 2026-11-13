# Assigment 4
---

### (1) - Either Methods
* The methods of Either , as for Maybe in Lab 5. new puts a value in Right . map applies a
function to the Right value, and flat_map does the same for a function that can fail itself.
Both pass a Left on unchanged.

```rust
#[derive(Debug, PartialEq)]
enum Either<L, R> {
    Left(L),
    Right(R),
}

impl<L, R> Either<L, R> {
    // the value is accepted as successfull 
    fn new(x: R) -> Either<L, R> {
        Either::Right(x)
    }

    // if the value is Right(val), it applies f(val)fuction, if it's Left(err) it only returns error 
    fn map<U>(self, f: fn(R) -> U) -> Either<L, U> {
        match self {
            Either::Right(val) => Either::Right(f(val)),
            Either::Left(err) => Either::Left(err),
        }
    }

    //  if the value is Right(val), it runs the f(val)fuction, if it's Left(err) it only returns error 
    fn flat_map<U>(self, f: fn(R) -> Either<L, U>) -> Either<L, U> {
        match self {
            Either::Right(val) => f(val),
            Either::Left(err) => Either::Left(err),
        }
    }
}
```
---
### (2) - divide and reciprocal 
* fn divide(a: f64, b: f64) -> Either<String, f64>
* fn reciprocal(x: f64) -> Either<String, f64>
divide(8.0, 4.0) gives Right(2.0) , divide(8.0, 0.0) gives Left("cannot divide 8 by
0") . reciprocal computes 1 / x using divide .

```rust
fn divide(a: f64, b: f64) -> Either<String, f64> {
    if b == 0.0 {
        Either::Left(format!("cannot divide {} by 0", a))
    } else {
        Either::Right(a / b)
    }
}

fn reciprocal(x: f64) -> Either<String, f64> {
    divide(1.0, x)
}
```
---
### (3) - sqrt
* fn sqrt(x: f64) -> Either<String, f64>

* sqrt(16.0) gives Right(4.0) , sqrt(-4.0) gives Left("cannot take the square root of
-4") .

```rust
fn sqrt(x: f64) -> Either<String, f64> {
    if x < 0.0 {
        Either::Left(format!("cannot take the square root of {}", x))
    } else {
        Either::Right(x.sqrt())
    }
}
```

### (4) - Full Code with Tests

```rust
#[derive(Debug, PartialEq)]
enum Either<L, R> {
    Left(L),
    Right(R),
}

impl<L, R> Either<L, R> {
    fn new(x: R) -> Either<L, R> {
        Either::Right(x)
    }

    fn map<U>(self, f: fn(R) -> U) -> Either<L, U> {
        match self {
            Either::Right(val) => Either::Right(f(val)),
            Either::Left(err) => Either::Left(err),
        }
    }

    fn flat_map<U>(self, f: fn(R) -> Either<L, U>) -> Either<L, U> {
        match self {
            Either::Right(val) => f(val),
            Either::Left(err) => Either::Left(err),
        }
    }
}

fn divide(a: f64, b: f64) -> Either<String, f64> {
    if b == 0.0 {
        Either::Left(format!("cannot divide {} by 0", a))
    } else {
        Either::Right(a / b)
    }
}

fn reciprocal(x: f64) -> Either<String, f64> {
    divide(1.0, x)
}

fn sqrt(x: f64) -> Either<String, f64> {
    if x < 0.0 {
        Either::Left(format!("cannot take the square root of {}", x))
    } else {
        Either::Right(x.sqrt())
    }
}

fn main() {
    fn main() {
    // Tests
    println!("{:?}", divide(8.0, 4.0));
    println!("{:?}", divide(8.0, 0.0));
    println!("{:?}", sqrt(16.0));
    println!("{:?}", sqrt(-4.0));

    println!("{:?}", divide(1.0, 16.0).flat_map(reciprocal).flat_map(sqrt));
    println!("{:?}", divide(8.0, 0.0).flat_map(reciprocal).flat_map(sqrt));
    println!("{:?}", divide(0.0, 5.0).flat_map(reciprocal).flat_map(sqrt));
    println!("{:?}", divide(-1.0, 4.0).flat_map(reciprocal).flat_map(sqrt));
}

```

