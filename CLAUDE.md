# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Shear is a Rust CLI tool for scene change detection in video files, designed for chunked video encoding workflows. It wraps the `av-scenechange` library (rav1e's scene detection algorithm) with FFmpeg backend support.

## Build and Development Commands

```bash
# Build optimized release binary
cargo build --release

# Run directly with arguments
cargo run --release -- --input video.mkv --output scenes.txt --fps-num 24000 --fps-den 1001 --total-frames 11520 --progress

# Run all tests
cargo test

# Run a specific test
cargo test test_split_long_scenes_single_split

# Format code
cargo fmt

# Lint code
cargo clippy

# Install locally
cargo install --path .
```

**Note**: Requires Rust nightly toolchain and FFmpeg development libraries.

## Architecture

Single-binary CLI tool with all code in `src/main.rs`:

1. **CLI Layer** (lines 19-55): Clap-based argument parsing with `Args` struct
2. **Main Processing** (lines 57-156): FFmpeg decoder setup via av-scenechange, scene detection execution, result processing
3. **Core Algorithm** - `split_long_scenes()` (lines 163-197): Takes scene boundaries and splits any scenes exceeding max length into equal chunks

**Data Flow**: Input video → FFmpeg decoder → av-scenechange detection → scene splitting → output file (one frame number per line)

## Dependencies

- `av-scenechange` (0.12): Core scene detection with FFmpeg feature
- `clap` (4): CLI argument parsing with derive macros
- `anyhow` (1): Error handling

## Conventions

- Follows conventional commit format (feat:, fix:)
- Release builds use LTO and binary stripping
