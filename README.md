# BabelStream HBM Bandwidth Benchmark

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![CI](https://img.shields.io/badge/CI-host--safe-green.svg)](.github/workflows/ci.yml)

Target: Ubuntu 26.04 · AMD · see Hardware Requirements. This is a host benchmark, not a laptop `pip install` project.

## Quick Start

```bash
git clone https://github.com/garys-gpu-benchmarks/315-gpu-bench-amd-babelstream-hbm-bandwidth-ubu2604.git
cd 315-gpu-bench-amd-babelstream-hbm-bandwidth-ubu2604
sudo bash setup.sh --assume-yes
bash run_benchmark.sh --profile smoke --validate
```
Results are written to `results/benchmark.db` and `results/summary.json`.

This workload is executed on the validation host after the repository is copied there. `setup.sh` and `run_benchmark.sh` do not open an outbound SSH session.

Prerequisites: Ubuntu 26.04; AMD; Python 3.14.4; root or sudo for `setup.sh`. Framework: Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC. This is a host benchmark, not a laptop `pip install` project.

```mermaid
flowchart LR
  setup.sh --> run_benchmark.sh --> parse_results.py --> results/benchmark.db
```

## 1. Overview

Builds and runs custom bin/hip-stream (BabelStream-style HIP kernels). array_size, num_iterations, and device_id are forwarded. peak_percent is bandwidth/53. runtime_ms is hardcoded 1.0. Sweep dimensions: device_id, dtype, block_size, array_size, kernels, warmup_iters, num_iterations, output_format.

## 2. What It Validates

- Validates Copy, Mul, Add, Triad, and Dot bandwidth from one BabelStream run
- #1: Copy bandwidth (bandwidth_gb_s_copy); is present and physically sensible.
- #2: Mul bandwidth (bandwidth_gb_s_mul); is present and physically sensible.
- #3: Add bandwidth (bandwidth_gb_s_add); is present and physically sensible.
- #4: Triad bandwidth (bandwidth_gb_s_triad); is present and physically sensible.
- #5: Dot bandwidth (bandwidth_gb_s_dot) is present and physically sensible.

## 3. Metrics Captured

- **#1: Copy bandwidth** — stored as `bandwidth_gb_s_copy`.
- **#2: Mul bandwidth** — stored as `bandwidth_gb_s_mul`.
- **#3: Add bandwidth** — stored as `bandwidth_gb_s_add`.
- **#4: Triad bandwidth** — stored as `bandwidth_gb_s_triad`.
- **#5: Dot bandwidth** — stored as `bandwidth_gb_s_dot`.

## 4. Hardware Requirements

### Supported environment

- OS: Ubuntu 26.04
- GPU vendor: AMD
- Framework family: Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC
- Python: Python 3.14.4

### Reference validation environment

The tables below describe the machine used to generate the reference results. They are not a requirement that every user buy that exact cloud instance.

### System

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC

### GPU

Ubuntu 26.04 / AMD / Bash, SQLite, Python, PyYAML, ROCm Runtime, C/C++, HIP/ROCm, HIPCC

## 5. Software Requirements

| Component | Version |
|---|---|
| OS | Ubuntu 26.04 |
| Kernel | kernel 7.0.0 |
| Python | Python 3.14.4 |
| ROCm | ROCm 7.14 |
| rocBLAS | N/A - rocBLAS not used |

Builds and runs custom bin/hip-stream (BabelStream-style HIP kernels). array_size, num_iterations, and device_id are forwarded. peak_percent is bandwidth/53.

## 6. Installation

```bash
Build via scripts/build.sh if needed; run bin/hip-stream
```

## 7. Running the Benchmark

```bash
Build via scripts/build.sh if needed; run bin/hip-stream
```

**Validating results separately:**

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional
python3 -m venv .venv
source ".venv/bin/activate"
".venv/bin/python" scripts/validate_results.py
```

## 8. Output

### `results/benchmark.db` (SQLite)

hip-stream stdout plus one CSV row per kernel

sample_index,status,kernel,bandwidth_gb_s,peak_percent,runtime_ms,error_message
0,ok,Copy,4200,79.2,1.0,

```bash
Build via scripts/build.sh if needed; run bin/hip-stream
```

### `results/summary.json`

Consolidated metrics from the most recent run — suitable for CI artifact upload or dashboard ingestion.

### `results/raw/<timestamp>.txt`

hip-stream stdout plus one CSV row per kernel

sample_index,status,kernel,bandwidth_gb_s,peak_percent,runtime_ms,error_message
0,ok,Copy,4200,79.2,1.0,

## 9. Baselines / Thresholds

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

## 10. Troubleshooting

**`setup.sh` missing collector**
Create cannot finish without `scripts/collect_workload.py`.

**`self_check` overlay rewritten**
Do not overwrite files listed in `results/overlay_lock.json`.

**Remote SSH drop during setup**
Reconnect and resume `bash setup.sh --assume-yes`. Do not wipe `.venv` or `.cache`.

## 11. NVIDIA H100 Coding Differences

Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Repository layout

```text
.
├── setup.sh
├── run_benchmark.sh
├── benchmark_specification.json
├── config/
├── scripts/
├── src/
├── tests/
├── docs/
├── results/
└── LICENSE
```
