# Rust Feature Development Process Proposal

This repository is developing a proposal for a staged development process for Rust that integrates incremental development plus specification work.

## About

The goal of this proposal is to establish a clear and structured approach to feature development in Rust. The proposal covers:

- **Staged Development**: How features progress through different stages from experimental to stable
- **Incremental Development**: Allowing features to be developed and tested iteratively
- **Specification Work**: Ensuring features are properly specified alongside implementation

## Reading the Proposal

The proposal is written as an [mdBook](https://rust-lang.github.io/mdBook/) and can be read online at:

**https://nikomatsakis.github.io/rust-feature-dev-process/**

## Building Locally

To build and view the book locally:

```bash
# Install mdbook if you haven't already
cargo install mdbook

# Build and serve the book
mdbook serve

# Or just build it
mdbook build
```

The built book will be available in the `book` directory.

## Contributing

Contributions and feedback are welcome! Please feel free to open issues or pull requests.

## License

This work is dual-licensed under:

- MIT License ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)
- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)

at your option.

This follows the same licensing approach as the Rust project itself.
