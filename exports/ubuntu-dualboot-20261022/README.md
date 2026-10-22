# Ubuntu dualboot — ReVeal/test snapshot, 2026-10-22

The repository data/, cache/, artifacts/ and runs/ directories were synchronized from the native Ubuntu run.
Their exact exported files are versioned in runtime.tar.gz.part-* (45 MiB maximum per part).
No credentials, .env, virtualenv, Java or Joern installation is included.

Restore from the repository root (this writes the archived runtime files):

```bash
(cd exports/ubuntu-dualboot-20261022 && sha256sum -c SHA256SUMS)
cat exports/ubuntu-dualboot-20261022/runtime.tar.gz.part-* | tar -xzf -
```

runtime_manifest.json records every restored file hash. Original executed notebooks are preserved in notebooks/.
The main notebooks/full-pipeline.ipynb entrypoint points at the Data project and defaults RUN_INFERENCE=False.
The raw snapshot notebook retains its original Ubuntu path and saved output for provenance.
Use the shared /media/quang-dung/Data/JUNIOR/LabThayTho/.env separately for FPT credentials.

This is a partial inference run with a 24000-byte context profile. See run_snapshot.json for the evaluated cohort.
43 integrated graphs carry explicit build-context assumptions. No extra LLM calls were made during export.

The snapshot has 1137 successful unique cached responses covering 1140 test records: identical prompt/request identities reuse one response across records.
