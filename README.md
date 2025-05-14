# GVarPy - Group Varint Encoding Library

Google's Group Varint encoding library implemented in Rust with Python bindings.

## What is Group Varint?

Group Varint is a variable-length integer encoding scheme that packs groups of four integers together with a single control byte. The control byte specifies the number of bytes used for each of the four integers (using two bits per integer).

This encoding is particularly efficient for sequences of integers that have mixed byte lengths but exhibit some locality in their size distribution.

## Features

- Fast, memory-safe implementation in Rust
- Python bindings via PyO3 (v0.24.2)
- Support for encoding/decoding slices of `u32` integers
- Robust error handling for malformed inputs
- Extensive test coverage
- Performance optimizations:
  - SmallVec (v1.15.0) for stack-allocated arrays (avoids heap allocations for small inputs)
  - Buffer reuse for decoding
  - Efficient control byte handling
- Comprehensive benchmarks

## Installation

### Python

```bash
# Install from PyPI
pip install gvarpy

# Or build from source
pip install maturin
maturin develop --release
```

### Rust

```toml
# Cargo.toml
[dependencies]
gvarpy = "0.1.0"
# Or with specific features:
# gvarpy = { version = "0.1.0", features = ["simd"] }
```

## Usage

### Python

```python
import gvarpy

# Encode a list of integers
data = [1, 42, 300, 65000]
encoded = gvarpy.encode(data)

# Decode back to a list
decoded = gvarpy.decode(encoded)
assert data == decoded

# Estimate encoded size
estimated_size = gvarpy.estimate_encoded_size(data)
```

### Rust

```rust
use gvarpy::GroupVarint;

// Encode a slice of integers
let data = [1u32, 42, 300, 65000];
let mut encoded = Vec::new();
GroupVarint::encode(&data, &mut encoded).unwrap();

// Decode back to a Vec<u32>
let decoded = GroupVarint::decode(&encoded).unwrap();
assert_eq!(data.to_vec(), decoded);

// Estimate encoded size
let estimated_size = GroupVarint::estimate_encoded_size(&data);
```

## Development

### Building from Source

```bash
# Clone the repository
git clone https://github.com/gideonphilip/gvarpy.git
cd gvarpy

# Build and test the Rust library
cargo test --all-features

# Build and test the Python bindings
# Install maturin (>= 1.4) first
pip install maturin>=1.4
maturin develop
python -m pytest tests/test_python.py
```

### Dependencies

- Rust 2021 edition
- PyO3 v0.24.2 or later
- smallvec v1.15.0 or later
- Python 3.8 or later (including 3.13)
- maturin 1.8.3 or later for building Python wheels

### Running Benchmarks

```bash
cargo bench
```

## Documentation

For more detailed documentation, see the [API Reference](https://docs.rs/gvarpy).

## References

- [Group Varint Encoding - Jeff Dean, WSDM 2009 Keynote](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/WSDM09-keynote.pdf)

## License

This project is licensed under either of

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or https://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or https://opensource.org/licenses/MIT)

at your option.

