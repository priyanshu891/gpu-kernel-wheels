# gpu-kernel-wheels

Prebuilt CUDA kernel wheels for [granite.build](https://github.com/priyanshu891/granite.build)'s AutoTuneX image, so the image does not have to compile them from source.

Each **release** is one build combination. Wheels are attached as release assets, not committed to git.

## Releases

| Release tag | CUDA | torch | Python | Platform | Packages |
|---|---|---|---|---|---|
| `cu130-torch2.11-cp312` | 13.0 | 2.11.0 | 3.12 | linux x86_64 | flash-attn 2.8.3, mamba-ssm 2.3.1, causal-conv1d 1.7.0 |

Each release ships:

- the wheels, named as pip expects, with no local version tag;
- `SHA256SUMS`, to verify downloads;
- `BUILD_INFO.txt`, the toolchain the wheels were built with.

## Verify a download

```bash
sha256sum -c SHA256SUMS
```

## Toolchain (from `BUILD_INFO.txt`)

- CUDA compilation tools 13.0 (V13.0.48)
- glibc 2.39 build host; the binaries need at most glibc 2.32
- Python 3.12.3
- torch 2.11.0+cu130

The wheels were built against torch 2.11.0+cu130. Install them into an environment with that torch and no other CUDA torch.

## Install

Replace `<wheel>` with the wheel's file name from the release:

```bash
pip install --no-deps \
  https://github.com/priyanshu891/gpu-kernel-wheels/releases/download/cu130-torch2.11-cp312/<wheel>
```

## Build target

The binaries contain code for several GPU architectures, including `sm_80` (A100) and `sm_90` (H100). This list comes from scanning the binaries for strings, not from `cuobjdump`, and some entries may be stray strings. Verify the architectures you need before relying on them.

## Licenses

Each wheel includes its upstream LICENSE and AUTHORS files under `*.dist-info/licenses/`. Those files govern redistribution of the wheel.

## Rebuilding

The build recipe is not in this repo yet. Until it is added, `BUILD_INFO.txt` records the toolchain, but rebuilding these wheels exactly is not yet reproducible from here.
