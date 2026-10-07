# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Builds and runs custom bin/hip-stream (BabelStream-style HIP kernels). array_size, num_iterations, and device_id are forwarded. peak_percent is bandwidth/53. runtime_ms is hardcoded 1.0. Sweep dimensions: device_id, dtype, block_size, array_size, kernels, warmup_iters, num_iterations, output_format.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| device_id | `--device-id` | smoke=0, baseline=0, extended=0 | 0 | From Parameter list; see Execution Description With Parameters. |
| dtype | `--dtype` | smoke=FP64, baseline=FP64, extended=FP64 | FP64 | From Parameter list; see Execution Description With Parameters. |
| block_size | `--block-size` | smoke=256, baseline=256, extended=256 | 256 | From Parameter list; see Execution Description With Parameters. |
| array_size | `--array-size` | smoke=1048576, baseline=268435456, extended=536870912 | 268435456 | From Parameter list; see Execution Description With Parameters. |
| kernels | `--kernels` | smoke=Copy,Mul,Add,Triad,Dot, baseline=Copy,Mul,Add,Triad,Dot, extended=Copy,Mul,Add,Triad,Dot | Copy,Mul,Add,Triad,Dot | From Parameter list; see Execution Description With Parameters. |
| warmup_iters | `--warmup-iters` | smoke=1, baseline=2, extended=5 | 2 | From Parameter list; see Execution Description With Parameters. |
| num_iterations | `--num-iterations` | smoke=2, baseline=30000, extended=40800 | 30000 | From Parameter list; see Execution Description With Parameters. |
| output_format | `--output-format` | smoke=csv, baseline=csv, extended=csv | csv | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Build via scripts/build.sh if needed; run bin/hip-stream
```

## Raw Output Format

hip-stream stdout plus one CSV row per kernel

sample_index,status,kernel,bandwidth_gb_s,peak_percent,runtime_ms,error_message
0,ok,Copy,4200,79.2,1.0,

## Metrics

- **#1: Copy bandwidth** — stored as `bandwidth_gb_s_copy`.
- **#2: Mul bandwidth** — stored as `bandwidth_gb_s_mul`.
- **#3: Add bandwidth** — stored as `bandwidth_gb_s_add`.
- **#4: Triad bandwidth** — stored as `bandwidth_gb_s_triad`.
- **#5: Dot bandwidth** — stored as `bandwidth_gb_s_dot`.

## Framework

Builds and runs custom bin/hip-stream (BabelStream-style HIP kernels). array_size, num_iterations, and device_id are forwarded. peak_percent is bandwidth/53.

## Installation and Execution Summary

Compile and run bin/hip-stream once over yaml array_size and num_iterations, parse Copy/Mul/Add/Triad/Dot bandwidth, and attach peak_percent = bandwidth/53, to measure HBM bandwidth. runtime_ms is hardcoded 1.0

## Platform Portability

- **AMD (primary):** ```bash
Build via scripts/build.sh if needed; run bin/hip-stream
```
- **NVIDIA:** Primary target is AMD ROCm. NVIDIA notes in this section are reference only and are not the execution path.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

hip-stream stdout plus one CSV row per kernel

sample_index,status,kernel,bandwidth_gb_s,peak_percent,runtime_ms,error_message
0,ok,Copy,4200,79.2,1.0,

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Builds and runs custom bin/hip-stream (BabelStream-style HIP kernels). array_size, num_iterations, and device_id are forwarded. peak_percent is bandwidth/53.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Builds and runs custom bin/hip-stream (BabelStream-style HIP kernels). array_size, num_iterations, and device_id are forwarded. peak_percent is bandwidth/53.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.
