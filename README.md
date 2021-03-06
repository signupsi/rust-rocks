# RustRocks

[![Build Status](https://travis-ci.org/bh1xuw/rust-rocks.svg?branch=master)](https://travis-ci.org/bh1xuw/rust-rocks)
[![crates.io badge](https://img.shields.io/crates/v/rocks.svg)](https://crates.io/crates/rocks)

Another RocksDB binding for Rust. [Documentation](https://docs.rs/rocks/)

Make RocksDB really rocks!

## How to compile

Static Link against: RocksDB 6.7.3.

Dynamic Link Tested: RocksDB 6.5.3 (ArchLinux).

tests pass under:

- macOS 10.15
- Linux amd64

For macOS(with RocksDB installed via brew):

    LIBRARY_PATH=/usr/local/lib CXXFLAGS=-I/usr/local/include \
    cargo test -- --nocapture

For Linux(with RocksDB installed into /usr/local):

    LD_LIBRARY_PATH=/usr/local/lib \
    LIBRARY_PATH=/usr/local/lib CXXFLAGS=-I/usr/local/include \
    cargo test -- --nocapture

For static build:

    git submodule update --init --recursive
    cargo test --features static-link -- --test-threads 1

    cargo test --features full -- --test-threads 1

List current supported compression types:

```console
$ cargo run --example it-works
RocksDB: 6.7.3
Compression Supported:
  - NoCompression
  - SnappyCompression
  - ZlibCompression
  - BZip2Compression
  - LZ4Compression
  - LZ4HCCompression
  - ZSTD
  - ZSTDNotFinalCompression
```

## Installation

Dynamicly link RocksDB:

```toml
[dependencies]
rocks = "0.1"
```

Static link against RocksDB(with snappy enabled by default):

```toml
[dependencies.rocks]
version = "0.1"
default-features = false
features = ["static-link"]
```

With all static features:

```toml
[dependencies.rocks]
version = "0.1"
default-features = false
features = ["full"]
```

## FAQ

- [Which features are supported/missing comparing to C++ RocksDB?](https://github.com/bh1xuw/rust-rocks/issues/1)
- [Why another RocksDB binding for Rust?](https://github.com/bh1xuw/rust-rocks/issues/2)

Feel free to Open a [New Issue](https://github.com/bh1xuw/rust-rocks/issues/new).

## TODOs

Big picture:

- [x] git submodule, static-link, compression as feature gate
- [x] information hiding (DO NOT EXPORT raw pointers)
- [x] Rust style
  - [x] wraps Status into a Rust style ``Result<T>``
  - [x] ``*Options`` via builder pattern
  - [x] handle CFHandle lifetime, Ref safety
- [ ] Lifetime safely guarantee
  - [x] `ReadOptions` + `snapshot`
  - [x] `ReadOptions` + `iterate_upper_bound`
  - [x] `DB` + `ColumnFamilyHandle`
  - [ ] `ColumnFamilyOptions` + `compaction_filter`
  - [ ] `ColumnFamilyOptions` + customized `comparator`
- [ ] Proof of usablility
- [ ] bench across C++/Java/other-rust binding
- [x] CI
  - [x] travis-ci integration
  - [ ] appveyor integration for windows
- [x] Zero-Copy between C++ part
  - [x] pinnable slice support
  - [x] exports `String/Vec<u8>` to C++ via `assign`-style API
- [ ] Full documentation with code examples
  - [x] good enough by copying C++ comments
  - [ ] rename C++ function names to rust name in doc comments
  - [ ] more examples in doc comment
