This repository is a multi-model evaluation framework for Stanford's BoxingGym benchmark, with Streamlit/TUI dashboards, W&B sweep orchestration, and PyMC-based model discovery scoring.

## Stack
- Python 3.11+, **uv** for dependency management; run tests with `uv run pytest tests/ -x`
- PyMC 5.x for probabilistic model evaluation, LiteLLM for multi-provider LLM calls
- W&B for sweep orchestration, Hydra (conf/) for config, Streamlit for dashboards

## Critical rules

**No backward compatibility in runtime code.** No alias fields, dual-read adapters, or legacy shims. Stale configs must fail fast with an explicit error at init.

**LiteLLM provider additions must be symmetric.** A new provider added to experiment runs must also appear in cost tracking, dashboard aggregation, and sweep configs — never partially.

**Canonical data source is the parquet snapshot.** README tables, HF Space summaries, and dashboard data all derive from the same parquet file. Never update them from separate ad-hoc sources.

**EIG and model-discovery metrics are distinct.** EIG (expected information gain) measures experimental design quality; prediction error / ELPD measures model discovery quality. Do not mix them in the same metric or aggregate them without explicit labeling.

**W&B sweep configs must specify all required fields.** Missing required sweep parameters must be caught at config validation, not at runtime.

## Code style
- No trivial comments — code should be self-documenting
- No single-use abstractions
- Prefer deleting stale provider shims over keeping them for compatibility
- Validate inputs at system boundaries (LLM API calls, file I/O); trust internal invariants
