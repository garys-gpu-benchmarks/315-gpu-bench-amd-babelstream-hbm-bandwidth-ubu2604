# Results: 315 BabelStream HBM Bandwidth

**Main goal:** Measure sustained BabelStream HBM bandwidth  
**Tools:** Build via scripts/build.sh if needed; run bin/hip-stream

Reference results from the validation run on the system shown in each section (recorded 2026-11-22). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× AMD Instinct MI300X VF (196288 MB) · INTEL(R) XEON(R) PLATINUM 8568Y+ · Ubuntu 26.04.1 LTS · kernel 7.0.0-43-generic · driver 7.1.9.31600000
- **Run:** 2026-11-22T14:30:30Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:34 · benchmark 00:01 · whole run 00:41
- **Source:** run 315_20261122_143030
- **Command:** `bash run_benchmark.sh --smoke --device-id 0 --block-size 256 --array-size 67108864 --warmup-iters 1 --num-iterations 20`

| # | Metric | Result |
|---|---|---|
| 1 | Copy bandwidth (bandwidth_gb_s_copy) | 3765.328 |
| 2 | Mul bandwidth (bandwidth_gb_s_mul) | 3749.199 |
| 3 | Add bandwidth (bandwidth_gb_s_add) | 3851.275 |
| 4 | Triad bandwidth (bandwidth_gb_s_triad) | 3835.202 |
| 5 | Dot bandwidth (bandwidth_gb_s_dot) | 2181.334 |
<!-- /profile:smoke -->
