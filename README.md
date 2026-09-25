# Morton Octree

## Abstract
This repository explores the implementation of Octrees using the Morton code technique written in Luau. Performance benchmarks show up to ~36 times faster when the `native` directive is enabled compared to standard VM interpretation, though non-native execution incurs notable interpreter overhead due to heavy bitwise manipulation.

## Overview

In Luau, multidimensional spatial structures such as `Table[x][y][z]` incur high runtime processing costs due to nested table indexing and multiple hash lookups per point. This library aims to eliminate nested indexing by interleaving the bits of 3D integer coordinates into a single 1D Morton code. By reducing 3D lookups to a single key, it significantly lowers search complexity. Because bit-interleaving relies heavily on bitwise operations, this implementation is specifically designed to leverage Luau's native code generation (`--!native`), where bitwise math is compiled directly into native hardware instructions.

> [!WARNING]
> This library was strictly designed to operate based on the native code generation enabled by the `--!native` directive.
> The NCG compiler requires modern CPU instructions to generate machine code reliably. When the hardware lacks support for the necessary instructions, the only fallback is to Luau VM bytecode. The resulting interpreter overhead can negate the benefits of single-table lookups, making it potentially slower than conventional spatial hashing approaches.
