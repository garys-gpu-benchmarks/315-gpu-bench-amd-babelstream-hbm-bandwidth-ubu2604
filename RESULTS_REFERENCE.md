# Results: 315 BabelStream HBM Bandwidth

**Main goal:** Measure sustained BabelStream HBM bandwidth  
**Tools:** Build via scripts/build.sh if needed; run bin/hip-stream

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_140 · recorded 2026-11-13). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-38-generic · driver 7.1.9.31600000
- **Run:** 2026-11-13T20:34:21Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:09 · benchmark 00:01 · whole run 00:18
- **Source:** run 315_20261113_203421
- **Command:** `bash run_benchmark.sh --smoke --device-id 0 --block-size 256 --array-size 67108864 --warmup-iters 1 --num-iterations 20`

| # | Metric | Result |
|---|---|---|
| 1 | Copy bandwidth (bandwidth_gb_s_copy) | 3723.08 |
| 2 | Mul bandwidth (bandwidth_gb_s_mul) | 3716.625 |
| 3 | Add bandwidth (bandwidth_gb_s_add) | 3860.601 |
| 4 | Triad bandwidth (bandwidth_gb_s_triad) | 3845.251 |
| 5 | Dot bandwidth (bandwidth_gb_s_dot) | 2163.478 |
<!-- /profile:smoke -->
