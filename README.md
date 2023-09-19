# Deletable Bloom Filter (DlBF)

The Deletable Bloom Filter (DlBF) is a data structure built on the idea of efficiently removing items from a Bloom filter when they are no longer needed. It leverages the fact that bits set by only one item can be safely deleted. This README provides an overview of how the DlBF works and its key concepts.

## How DlBF Works

### Basic Idea

- DlBF tracks bit collisions when inserting items.
- It encodes regions of deletable bits efficiently, using a fraction of the filter memory.
- An item can be removed if at least one of its bits is in a collision-free region.

### Key Components

- Bit Array: The DlBF uses a bit array of size `m`.
- Regions: The bit array is divided into `r` regions, each with `m'` bits.
- Collision Bitmap: A bitmap of size `r` marks collision-free regions with 0 and regions with collisions with 1.

### Operations

- `Insert(x)`: Maps an item `x` to `k` bit positions using independent hashes. If a bit is already set (collision), the corresponding region is marked as non-deletable.
- `Query(x)`: Checks if the `k` bit positions are set to 1.
- `Remove(x)`: Clears only those bit positions among `k` located in collision-free zones.

### Trade-Offs

- Avoids false negatives at the cost of some items not being deletable (counted as false positives).
- The choice of the parameter `r` is critical, affecting deletability and false positive behavior.

## Usage

To use the Deletable Bloom Filter in your applications, consider the following steps:

1. Create a DlBF instance using the desired size, hash seeds, and the number of regions.
2. Use `Put(x)` to insert elements, `Check(x)` to check existence, and `Delete(x)` to remove elements.

```rust
// Example Rust code (not provided in this README)

// Create a Deletable Bloom Filter
let mut dlbf = DeletableBloomFilter::new(size, hash_seeds, num_regions);

// Insert an element
dlbf.put(&element);

// Check if an element exists
let exists = dlbf.check(&element);

// Remove an element
let removed = dlbf.delete(&element);
```

### Limitations

In dynamic applications with frequent insertions and deletions, non-removable bits may accumulate, leading to increased false positives.

### Choosing Parameter r
The choice of the r parameter is crucial, as it impacts the filter's performance in terms of deletability and false positives. You may need to adjust this parameter based on your specific use case.

## Research Paper
Here is the original research paper - [The Deletable Bloom filter](https://arxiv.org/pdf/1005.0352.pdf)