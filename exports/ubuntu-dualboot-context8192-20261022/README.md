# Ubuntu dualboot — ReVeal/test, 8192-byte context, 2026-10-22

Dataset prefix: 50% = 1137/2274 samples. Selected prefix completed; full-dataset status remains partial.
446 oversized prompts were re-fit with the historical fitter and called through FPT; 691 exact responses were reused.
Model: DeepSeek-V4-Flash. No request errors or invalid predictions. Accuracy 90.8531%; F1=0; MCC=0.
The model predicted all 1137 selected samples as non-vulnerable (1033 negatives, 104 positives).
Comparison against 24000-byte prompts uses the exact same 1137 record IDs in context_benchmark.json.
This is descriptive prefix evaluation, not a complete official test benchmark.

This archive is incremental on base commit `8a4ecf763b092926b06072feccce73dbeafb1dbb`. Restore the previous snapshot first on a clean clone:

```bash
(cd exports/ubuntu-dualboot-context8192-20261022 && sha256sum -c SHA256SUMS)
cat exports/ubuntu-dualboot-context8192-20261022/runtime.tar.gz.part-* | tar -xzf -
```

runtime_manifest.json records the exact hashes of profile prompts, inference outputs and new cache responses.
The Data-root runtime files have already been synchronized. No credentials or .env are included.
The exact saved notebook is preserved in notebooks/; execution results come from the saved run reports.
The main notebook entrypoint defaults RUN_INFERENCE=False to avoid accidental new API calls.
