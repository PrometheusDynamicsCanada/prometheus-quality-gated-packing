# Data Dictionary

## 1. Master CSV Ledgers (`data/`)

- **`data/ALL_29_PROVIDER_JOBS_LEDGER.csv`** (`29` rows = `28` completed benchmark jobs + `1` superseded `ibm_kingston` queue-failover record):
  - `campaign_id`: `campaign1_option_c`, `campaign2_option2_abccba`, or `campaign3_multi_workload`.
  - `record_kind`: `benchmark` (`28` rows) or `auxiliary_superseded` (`1` row: `dbuo2p782tmc73dtohkg`, billed `3.0 s` on `ibm_kingston` prior to cancellation).
  - `hardware_job_id`: Verifiable IBM Quantum Job ID or IQM Resonance UUID.
  - `pack_factor`: Simultaneous `8`-qubit circuits co-scheduled per physical PUB (`1`, `4`, or `8`).
  - `physical_pubs`: Number of physical PUBs executed in that job (`121` total = `120` benchmark + `1` auxiliary).
  - `physical_shots`: Physical hardware shots (`physical_pubs * shots_per_pub`, `186,368` total = `184,320` benchmark + `2,048` auxiliary).
  - `billed_amount` & `billing_unit`: Native provider charge (`96.00 seconds` IBM Quantum; `15.75 credits` IQM Resonance).
  - `reported_calibration_id`: Provider calibration identifier recorded at execution completion.

- **`data/ALL_112_CIRCUIT_EVALUATIONS_LEDGER.csv`** (`112` rows = `14` completed runs x `8` logical circuits/run):
  - `subgraph_sha256_id`: Deterministic truncated SHA-256 commitment (`isl_<16-hex>`) of the sorted `8`-qubit physical island, enabling offline verification of distinct-island counts and matched-island equality (`Slot 2` == `Slot 3` in Campaign 3) while withholding raw physical qubit coordinates per [`PUBLIC_DISCLOSURE.md`](PUBLIC_DISCLOSURE.md).
  - `compiled_2q_gates` & `compiled_depth`: Post-routing native two-qubit gate count (`CZ`) and subcircuit depth.
  - `prim_fb`, `conf_fb`, `primary_lcb`, `confirm_lcb`: Primary (`2,048` shots) and confirmation (`1,024` shots) Classical Bhattacharyya Overlap scores and conservative lower confidence bounds (`score - 1.96 / (2 * sqrt(N))`).
  - `comb_fb`, `comb_fh`, `comb_tvd`, `comb_hop`, `comb_xeb`, `comb_entropy_bits`, `wave_agreement`: Pooled `3,072`-shot observables (`comb_entropy_bits` is descriptive).

## 2. Raw Bitstring Histograms & Reference Distributions

- **`data/logical_workloads/logical_workloads_and_references.json`**: Unrouted OpenQASM 2.0 source and exact normalized `256`-state `ideal_probs` for all `9` unique logical circuits (`C12_K8_MaxCut_QAOA_p2` + `W1a..W4b`).
- **`data/raw_counts/campaign1_option_c_raw_counts.json`**, **`campaign2_option2_abccba_raw_counts.json`**, **`campaign3_multi_workload_raw_counts.json`**: Complete per-circuit `2,048`-shot primary and `1,024`-shot confirmation bitstring histograms (`224` histograms across `112` evaluations).
