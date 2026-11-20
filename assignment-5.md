# Assigment 5
---

### (1) - Memoizetion
* A memoized function remembers its results: the second call with the same argument returns the
stored result instead of computing it again.

* (1.1) memoize — takes any function and returns a memoized version of it, keeping the results in a
HashMap captured by the closure. In a comment, explain why it returns FnMut and not Fn

```rust
use std::collections::HashMap;
use std::hash::Hash;

fn memoize<A, R, F>(mut f: F) -> impl FnMut(A) -> R
where
    A: Eq + Hash + Clone,
    R: Clone,
    F: FnMut(A) -> R, //FnMut because Clousure changes the cach in HasMap
{
    let mut cache: HashMap<A, R> = HashMap::new();
    move |arg: A| -> R {
        if let Some(result) = cache.get(&arg) {
            return result.clone();
        }
        let result = f(arg.clone());
        cache.insert(arg, result.clone());
        result
    }
}
```
---
*  (1.2) - Slow Function

```rust
use std::thread;
use std::time::{Duration, Instant};

fn slow_square(x: u64) -> u64 {
    thread::sleep(Duration::from_secs(2)); // simulating the 2 seconds task
    x * x
}

fn measure_memoize() {
    let mut fast_square = memoize(slow_square);

    let start1 = Instant::now();
    let res1 = fast_square(5);
    let dur1 = start1.elapsed();
    println!("1. çağrı: {} ({:.2} sn)", res1, dur1.as_secs_f64());

    let start2 = Instant::now();
    let res2 = fast_square(5);
    let dur2 = start2.elapsed();
    println!("2. çağrı: {} ({:.6} sn)", res2, dur2.as_secs_f64());

    // The first call takes approximately 2.00 seconds because the argument (5) is not yet in the cache
    // The second call takes 0.00 seconds (microseconds) because the result is stored in the HashMap
    // and is returned directly from memory without being recalculated.
}
```
---
### (2) - Fibonacci - fib_memo
* the same recursion, but it first looks n up in cache , and stores every result it
computes there. In a comment, explain why memoize(fib) from subtask 1 would not make
fib(50) fast.


```rust
fn fib_memo(n: u64, cache: &mut HashMap<u64, u64>) -> u64 {
    // if n was calculated before, return from the cache 
    if let Some(&val) = cache.get(&n) {
        return val;
    }

    let result = if n < 2 {
        n
    } else {
        fib_memo(n - 1, cache) + fib_memo(n - 2, cache)
    };

    cache.insert(n, result);
    result
}

// The recursive calls in the body of the `fib` function (`fib(n - 1) + fib(n - 2)`)
// continue to call the original, uncached `fib` function directly, rather than the wrapped closure,
// so the subproblems are calculated over and over again.

```

### (3) - Full Code with Tests

```rust
use std::collections::HashMap;
use std::hash::Hash;
use std::thread;
use std::time::{Duration, Instant};

// 1. Memoize
fn memoize<A, R, F>(mut f: F) -> impl FnMut(A) -> R
where
    A: Eq + Hash + Clone,
    R: Clone,
    F: FnMut(A) -> R,
{
    let mut cache: HashMap<A, R> = HashMap::new();

    move |arg: A| -> R {
        if let Some(val) = cache.get(&arg) {
            return val.clone();
        }
        let val = f(arg.clone());
        cache.insert(arg, val.clone());
        val
    }
}

// 2. Slow Funciton Example 
fn slow_calc(x: u64) -> u64 {
    thread::sleep(Duration::from_secs(2));
    x * 10
}

// 3. fib_memo
fn fib_memo(n: u64, cache: &mut HashMap<u64, u64>) -> u64 {
    if let Some(&val) = cache.get(&n) {
        return val;
    }

    let result = if n < 2 {
        n
    } else {
        fib_memo(n - 1, cache) + fib_memo(n - 2, cache)
    };

    cache.insert(n, result);
    result
}

fn main() {
    // 1.Test (calculation of time)
    let mut memo_calc = memoize(slow_calc);

    let t1 = Instant::now();
    let r1 = memo_calc(7);
    println!("1. call result: {}, time: {:.2} seconds", r1, t1.elapsed().as_secs_f64());

    let t2 = Instant::now();
    let r2 = memo_calc(7);
    println!("2. call result: {}, süre: {:.6} saniye", r2, t2.elapsed().as_secs_f64());
    // 1. call  takes 2.00 seconds because the value is calculated for the first time
    // 2. call  takes almost 0.00 seconds because it reads immediately from the HashMap

    // 2. Test (fib(50))
    let mut fib_cache = HashMap::new();
    let t_fib = Instant::now();
    let f50 = fib_memo(50, &mut fib_cache);
    println!("fib_memo(50) = {} (time: {:.6} seconds)", f50, t_fib.elapsed().as_secs_f64());
}

```
