# Assigment 4
---

### (1) - Count Word
```rust
use std::collections::HashMap;

fn count_words(text: &str) -> HashMap<String, usize> {
    let mut counts = HashMap::new();

    // splitting the text based on space
    for word in text.split_whitespace() {
        let clean_word = word.to_lowercase(); // non-case-sensitive

        // Entry API: If the words exists, increase its counter 1; if not, start it with 0 and increase its counter to 1. 
        let count = counts.entry(clean_word).or_insert(0);
        *count += 1;
    }

    counts
}
```
---

### (2) - Count Of
```rust
use std::collections::HashMap;

fn count_of(counts: &HashMap<String, usize>, word: &str) -> usize {
    let clean_word = word.to_lowercase();
    counts.get(&clean_word).copied().unwrap_or(0)
}
```
---

### (3) - Total Words
```rust
use std::collections::HashMap;

fn total_words(counts: &HashMap<String, usize>) -> usize {
    counts.values().sum() // sum up all the vales in the map to count the total
}
```
---

### (4) - Sorted Words
```rust
use std::collections::HashMap;

fn sorted_words(counts: &HashMap<String, usize>) -> Vec<(String, usize)> {
    // the pairs in the map (key,value) -> vector
    let mut pairs: Vec<(String, usize)> = counts.iter().map(|(w, &c)| (w.clone(), c)).collect();

    // logic of the sorting
    pairs.sort_by(|a, b| {
        // first look at the numbersk: compare b with a 
        let count_order = b.1.cmp(&a.1);

        if count_order != std::cmp::Ordering::Equal {
            count_order
        } else {
            // if there is tie, compare them alphabetically 
            a.0.cmp(&b.0)
        }
    });

    pairs
}

```
---
---

### (5) - Full Code
```rust
use std::collections::HashMap;

fn count_words(text: &str) -> HashMap<String, usize> {
    let mut counts = HashMap::new();
    for word in text.split_whitespace() {
        let clean_word = word.to_lowercase();
        *counts.entry(clean_word).or_insert(0) += 1;
    }
    counts
}

fn count_of(counts: &HashMap<String, usize>, word: &str) -> usize {
    let clean_word = word.to_lowercase();
    counts.get(&clean_word).copied().unwrap_or(0)
}

fn total_words(counts: &HashMap<String, usize>) -> usize {
    counts.values().sum()
}

fn sorted_words(counts: &HashMap<String, usize>) -> Vec<(String, usize)> {
    let mut pairs: Vec<(String, usize)> = counts
        .iter()
        .map(|(w, &c)| (w.clone(), c))
        .collect();

    pairs.sort_by(|a, b| {
        let count_order = b.1.cmp(&a.1);
        if count_order != std::cmp::Ordering::Equal {
            count_order
        } else {
            a.0.cmp(&b.0)
        }
    });

    pairs
}

fn main() {
    let text = "the cat The dog THE end";

    // TEST 
    // 1. count_words 
    let counts = count_words(text);
    println!("Words table: {:?}", counts);
    
    // 2. count_of 
    println!("'the' counter: {}", count_of(&counts, "the"));     // expecting 3 
    println!("'horse' counter: {}", count_of(&counts, "horse")); // expecting 0 
    
    // 3. total_words 
    println!("Total words: {}", total_words(&counts));        // expecting 6 
    
    // 4. sorted_words 
    println!("Sorted list: {:?}", sorted_words(&counts));
    // expecting: [("the", 3), ("cat", 1), ("dog", 1), ("end", 1)]
}
```
 

  
