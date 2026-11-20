# Results: 315 BabelStream HBM Bandwidth

**Main goal:** Measure sustained BabelStream HBM bandwidth  
**Tools:** Build via scripts/build.sh if needed; run bin/hip-stream

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_149 · recorded 2026-11-19). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-43-generic · driver 7.1.9.31600000
- **Run:** 2026-11-19T21:38:24Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:10 · benchmark 00:01 · whole run 00:17
- **Source:** run 315_20261119_213824
- **Command:** `bash run_benchmark.sh --smoke --device-id 0 --block-size 256 --array-size 67108864 --warmup-iters 1 --num-iterations 20`

| # | Metric | Result |
|---|---|---|
| 1 | Copy bandwidth (bandwidth_gb_s_copy) | 3771.909 |
| 2 | Mul bandwidth (bandwidth_gb_s_mul) | 3745.704 |
| 3 | Add bandwidth (bandwidth_gb_s_add) | 3853.836 |
| 4 | Triad bandwidth (bandwidth_gb_s_triad) | 3874.078 |
| 5 | Dot bandwidth (bandwidth_gb_s_dot) | 2229.905 |
<!-- /profile:smoke -->
