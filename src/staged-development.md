# Staged Development

This chapter describes the stages that a feature progresses through during development.

## Stage Overview

Features in Rust should progress through well-defined stages, each with specific goals and requirements.

### Proposed Stages

1. **Experimental**: Initial exploration and prototyping
2. **Unstable**: Implementation available on nightly for testing
3. **Stabilizing**: Feature is being prepared for stabilization
4. **Stable**: Feature is available in stable Rust

## Stage Requirements

Each stage has specific entry and exit criteria that must be met before a feature can advance.

### Experimental Stage

- RFC accepted or design discussion ongoing
- Prototype implementation may exist
- No guarantees about future direction

### Unstable Stage

- Implementation on nightly
- Basic documentation available
- Feature flag required
- Breaking changes allowed

### Stabilizing Stage

- Specification work complete or in progress
- Comprehensive documentation
- Substantial real-world testing
- No anticipated breaking changes

### Stable Stage

- Full specification available
- Complete documentation
- Stabilization approved
- Backward compatibility guaranteed
