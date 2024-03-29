# IndexedLinkedHashMap &emsp; [![Latest Version]][crates.io] [![Downloads]][crates.io] [![Documentation]][docs.rs]

[Latest Version]: https://img.shields.io/crates/v/indexedlinkedhashmap
[Downloads]: https://img.shields.io/crates/d/indexedlinkedhashmap
[Documentation]: https://img.shields.io/docsrs/indexedlinkedhashmap/latest
[crates.io]: https://crates.io/crates/indexedlinkedhashmap
[docs.rs]: https://docs.rs/indexedlinkedhashmap

An indexable LinkedHashMap. Written in Rust.

## About

Bring your own ordering data structure. Uses the standard library's `HashMap`.

- If you use a data structure like `Vec` for keys, you can index easily.
- If you use a data structure like `BinaryHeap` for keys, it doesn't make much sense to index on certain operations.
  - For example, this is how you'd call the set method: `ins.set(None, value)`.

## Devlopers

- If you want to use your own data structure, implement the `Ordered` trait at `indexedlinkedhashmap::traits::Ordered`.

## Features

- collections_ordering_vec
  - Support for `Vec` usage
- collections_ordering_binary_heap
  - Support for `BinaryHeap` usage

## Usage

```toml
[dependencies]
indexedlinkedhashmap = "3.0.0"
```

```toml
[dependencies]
indexedlinkedhashmap = { version = "3.0.0", features = [ "collections_ordering_vec", "collections_ordering_binary_heap" ] }
```

### Examples

```rust
fn main() {
    let mut ins = IndexedLinkedHashMap::<Vec<&str>, &str, usize>::new();

    assert!(ins.remove("k") == None);
    assert!(ins.len() == 0);
    assert!(ins.keys().len() == 0);
    assert!(ins.values().len() == 0);

    ins.set("k", 1);

    assert!(
        ins.remove("k")
            == Some(IndexedLinkedHashMapValue {
                index: Some(0),
                value: 1
            })
    );
    assert!(ins.len() == 0);
    assert!(ins.keys().len() == 0);
    assert!(ins.values().len() == 0);
}
```

```rust
fn main() {
    let mut ins = IndexedLinkedHashMap::<Vec<&str>, &str, usize>::new();
    
    ins.set("k", 1);

    assert!(ins.len() == 1);
    assert!(ins.keys().len() == 1);
    assert!(ins.values().len() == 1);
    assert!(ins.get("k") == Some(&1));
}
```

```rust
#[derive(Clone, Debug)]
struct Line2D {
    id: String,
    p1: usize,
    p2: usize,
}

fn main() {
    let mut ins = IndexedLinkedHashMap::<Vec<String>, String, Line2D>::new();
    let line = Line2D {
        id: String::from("1"),
        p1: 0,
        p2: 10,
    };

    ins.set(line.to_owned().id, line);
}
```

```rust
use std::collections::BinaryHeap;

fn main() {
    let mut ins = IndexedLinkedHashMap::<BinaryHeap<usize>, usize, bool>::new();
    ins.set(2, false);
    ins.set(1, true);

    assert!(ins.at(Some(1)) == Some(&true));
}
```

## Testing

Run `cargo test --features collections_ordering_vec,collections_ordering_binary_heap`
