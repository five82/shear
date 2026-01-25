# shear

Scene change detection for chunked video encoding.

A thin wrapper around [av-scenechange](https://github.com/rust-av/av-scenechange) that detects scene boundaries and automatically splits long scenes for parallel encoding workflows.

## Features

- Scene detection using av-scenechange (rav1e's algorithm)
- Automatic splitting of long scenes at regular intervals
- Simple output format (one frame number per line)
- Configurable maximum scene length

## Installation

Requires Rust toolchain and FFmpeg development libraries.

### From GitHub

```bash
cargo install --git https://github.com/five82/shear
```

### From source

```bash
git clone https://github.com/five82/shear
cd shear
cargo install --path .
```

The binary will be installed to `~/.cargo/bin/shear`.

## Usage

```bash
shear -i input.mkv -o scenes.txt \
  --fps-num 24000 --fps-den 1001 \
  --total-frames 11520 \
  --progress
```

### Options

| Flag | Description |
|------|-------------|
| `-i, --input` | Input video file |
| `-o, --output` | Output scene file |
| `--fps-num` | FPS numerator |
| `--fps-den` | FPS denominator |
| `--total-frames` | Total frame count |
| `--max-scene-secs` | Max scene length in seconds (default: 10) |
| `--max-scene-frames` | Max scene length in frames (default: 300) |
| `--progress` | Show progress output |

### Output format

One frame number per line, representing scene start frames:

```
0
720
1440
2160
```

## How it works

1. Runs av-scenechange scene detection (Standard mode with flash detection)
2. Splits any scene longer than the maximum into evenly-sized chunks
3. Outputs the final list of scene boundaries

This ensures no chunk is too long while respecting natural scene boundaries where possible.

## Related projects

- [reel](https://github.com/five82/reel) - AV1 encoding tool using shear for scene-based chunking
- [spindle](https://github.com/five82/spindle) - Encoding orchestrator
- [av-scenechange](https://github.com/rust-av/av-scenechange) - The underlying scene detection library

## License

GPL-3.0
