# Public Disclosure & Verification Scope

## 1. Verification Tier (`Tier 1: Raw-Measurement Histogram Reconstruction`)

This repository (`prometheus-quality-gated-packing`) archives full per-circuit primary (`2,048` shots) and confirmation (`1,024` shots) bitstring measurement histograms (`data/raw_counts/*.json`), unrouted logical OpenQASM 2.0 circuits and exact `256`-state reference distributions (`data/logical_workloads/logical_workloads_and_references.json`), provider Job IDs, execution timestamps, reported calibration IDs, and settled billing records (`data/ALL_29_PROVIDER_JOBS_LEDGER.csv`). Running `python scripts/verify_quality_gated_packing.py` (`numpy` only) independently reconstructs all `112` logical circuit evaluations and provider billing totals offline.

## 2. Withheld Proprietary Implementation

Per Prometheus Dynamics portfolio policy, internal compiler heuristics, multi-objective topology scoring weights, quality-gating survival formulas, execution-control source code, and raw physical qubit coordinate arrays are withheld. Physical `8`-qubit placements are represented by deterministic cryptographic hashes (`subgraph_sha256_id`) and `original_private_bundle_sha256` digests for continuity during confidential technical diligence.

## 3. Clean Single-Commit Provenance

This repository is published as a freshly initialized single-commit sanitized release (`0` pre-sanitization historical blobs in Git object storage).
