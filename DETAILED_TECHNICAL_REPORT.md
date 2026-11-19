# Detailed Three-Campaign Technical Report: Quality-Gated Packing, Counterbalanced `ABCCBA` Replication & Multi-Workload Validation

*Companion technical report to [`README.md`](README.md) in [`prometheus-quality-gated-packing`](https://github.com/PrometheusDynamicsCanada/prometheus-quality-gated-packing).*

## 1. Unified Accounting Summary Across All Three Live Campaigns (`29` Unique Provider Records)

| Campaign | Target Processors | Provider Records (`Bench + Aux`) | Physical PUBs (`Bench + Aux`) | Physical Shots (`Bench + Aux`) | Settled Provider Billing | Primary Verified Outcome |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **Campaign 1 (`Option C` Cross-Vendor Baseline)** | `ibm_fez` (`156Q`), `iqm_emerald` (`54Q`), `ibm_kingston` (`156Q` aux) | `9` (`8 + 1`) | `39` (`38 + 1`) | `60,416` (`58,368 + 2,048`) | `19.00 s` IBM (`11+5+3`) + `15.75 cr` IQM (`9.75+6.0`) | **`-27.27%` IBM QPU `s`/accepted** (`1.3750 s -> 1.0000 s` on `ibm_fez`); `IQM Emerald` `0/8` operating boundary (`-38.46%` credit spend) |
| **Campaign 2 (`Option 2` Counterbalanced `ABCCBA`)** | `ibm_fez` (`156Q`) | `12` (`12 + 0`) | `44` (`44 + 0`) | `67,584` (`67,584 + 0`) | `44.00 s` IBM (`22+12+10`) | **`Quality-Gated 4-Way` `1.5000 s/accepted` (`8/16`)** beat `Unconstrained 8-Way` `1.6667 s/accepted` (`6/16`, **`-10.00%` `s/acc`, `+33.33%` yield**); `1-Way` `0/16` |
| **Campaign 3 (`Multi-Workload` `4` Families, `8` Circuits)** | `ibm_fez` (`156Q`) | `8` (`8 + 0`) | `38` (`38 + 0`) | `58,368` (`58,368 + 0`) | `33.00 s` IBM (`11+11+6+5`) | **`Unconstrained 8-Way` `0.6250 s/accepted` (`8/8`, `-54.55%` vs. `1-Way`)**; placement-matched `4-Way` `0.8571 s/accepted` (`-37.66%`, `-0.0013` mean $\Delta F_B$) |
| **CUMULATIVE REPOSITORY TOTALS** | **`ibm_fez`, `ibm_kingston`, `iqm_emerald`** | **`29` (`28 + 1`)** | **`121` (`120 + 1`)** | **`186,368` (`184,320 + 2,048`)** | **`96.00 s` IBM + `15.75 cr` IQM** | **`0.0000` ledger discrepancy across all `29` records; `112/112` raw-histogram evaluations verified** |

---

## 2. Campaign 3 Per-Family & Per-Circuit Breakdown (`8` Distinct `8Q` Circuits Across `4` Algorithm Families)

Physical `8Q` subgraph coordinates are represented by deterministic `subgraph_sha256_id` commitments (`isl_<16-hex>`) per [`PUBLIC_DISCLOSURE.md`](PUBLIC_DISCLOSURE.md), preserving exact island-count and placement-matching verification (`Slot 2` == `Slot 3` across all `8` circuits) without exposing proprietary chip-coordinate rankings.

### 2.1 Algorithm-Family Summary (`N = 2` Distinct Circuits per Family per Arm)

| Algorithm Family (`Logical 2Q Gates / Depth`) | `Slot 1: Legacy 1-Way` $F_B$ / $F_H$ / `TVD` `*(mean)*` | `Slot 2: Rotated 1-Way` $F_B$ / $F_H$ / `TVD` `*(mean)*` | `Slot 3: Matched 4-Way` $F_B$ / $F_H$ / `TVD` `*(mean)*` | `Slot 4: Unconstrained 8-Way` $F_B$ / $F_H$ / `TVD` `*(mean)*` | `% Delta TVD` (`Slot 2` vs. `Slot 1`) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`1D_Ising_Trotter` (`98` `CX` / depth `51`)** | `0.9342` / `0.8728` / `0.2747` (`2/2` acc) | `0.9543` / `0.9107` / `0.2151` (`2/2` acc) | **`0.9557` / `0.9135` / `0.2120` (`2/2` acc)** | `0.9514` / `0.9053` / `0.2254` (`2/2` acc) | **`-21.70%` `TVD`** (`0` routing SWAPs) |
| **`3Regular_MaxCut_QAOA` (`72` `CX` / depth `50-86`)** | **`0.8729` / `0.7620` / `0.3821` (`2/2` acc)** | `0.8326` / `0.6946` / `0.4286` (`2/2` acc) | `0.8338` / `0.6965` / `0.4245` (`2/2` acc) | `0.8159` / `0.6665` / `0.4466` (`2/2` acc) | `+12.17%` `TVD` (sector variation on $Q_3$) |
| **`Coherent_Inverse_QFT8` (`56` `CX` / depth `57`)** | `0.8844` / `0.7821` / `0.3125` (`2/2` acc) | **`0.9124` / `0.8325` / `0.2498` (`2/2` acc)** | `0.9101` / `0.8283` / `0.2558` (`2/2` acc) | `0.8839` / `0.7814` / `0.3080` (`2/2` acc) | **`-20.06%` `TVD`** (`+3.17%` $F_B$) |
| **`Dense_AllToAll_QAOA` (`112` `CX` / depth `67`)** | `0.8288` / `0.6947` / `0.4564` (`2/2` acc) | `0.8167` / `0.6778` / `0.4650` (`2/2` acc) | `0.8111` / `0.6692` / `0.4714` (`1/2` acc) | **`0.8729` / `0.7647` / `0.3870` (`2/2` acc)** | `+1.88%` `TVD` (`Slot 4` `8/8` accepted) |

### 2.2 Per-Circuit Hardware Ledger (`32` Logical Evaluations Across `W1a..W4b` in `data/ALL_112_CIRCUIT_EVALUATIONS_LEDGER.csv`)

| Circuit & Family | `Slot 1` (`1-Way` Pinned): `CZ`/Dep, $F_B$ ($F_H$), `TVD`, Status | `Slot 2` (`1-Way` Rotated): `CZ`/Dep, $F_B$ ($F_H$), `TVD`, Status | `Slot 3` (`4-Way` Matched `S2`): `CZ`/Dep, $F_B$ ($F_H$), `TVD`, Status | `Slot 4` (`8-Way` Buffered): `CZ`/Dep, $F_B$ ($F_H$), `TVD`, Status |
| :--- | :---: | :---: | :---: | :---: |
| **`0: W1a_Ising1D_Ferro`** (`1D` Ising) | `137`/`120`, `0.9337` (`0.8718`), `0.2795`, `ACCEPTED` | **`98`/`81`**, `0.9647` (`0.9306`), `0.1819`, `ACCEPTED` | **`98`/`81`**, **`0.9675` (`0.9361`)**, **`0.1713`**, `ACCEPTED` | `137`/`120`, `0.9441` (`0.8913`), `0.2414`, `ACCEPTED` |
| **`1: W1b_Ising1D_Canted`** (`1D` Ising) | `137`/`120`, `0.9347` (`0.8737`), `0.2698`, `ACCEPTED` | **`98`/`81`**, `0.9439` (`0.8909`), `0.2482`, `ACCEPTED` | **`98`/`81`**, `0.9439` (`0.8909`), `0.2526`, `ACCEPTED` | `140`/`158`, **`0.9587` (`0.9192`)**, **`0.2094`**, `ACCEPTED` |
| **`2: W2a_Wagner3Reg_QAOA`** ($M_8$ QAOA) | **`168`/`228`**, **`0.8751` (`0.7659`)**, `0.3866`, `ACCEPTED` | **`168`**/`234`, `0.8700` (`0.7568`), `0.3893`, `ACCEPTED` | **`168`**/`234`, `0.8698` (`0.7565`), **`0.3852`**, `ACCEPTED` | `209`/`279`, `0.8456` (`0.7151`), `0.4163`, `ACCEPTED` |
| **`3: W2b_Cube3Reg_QAOA`** ($Q_3$ QAOA) | **`167`**/`172`, **`0.8707` (`0.7582`)**, **`0.3776`**, `ACCEPTED` | `179`/**`156`**, `0.7952` (`0.6323`), `0.4678`, `ACCEPTED` | `179`/**`156`**, `0.7978` (`0.6365`), `0.4637`, `ACCEPTED` | `179`/**`156`**, `0.7861` (`0.6180`), `0.4769`, `ACCEPTED` |
| **`4: W3a_CoherentIQFT8_A`** (`QFT-8` #1) | `99`/`123`, `0.8891` (`0.7904`), `0.3301`, `ACCEPTED` | **`83`/`112`**, **`0.9126` (`0.8328`)**, **`0.2724`**, `ACCEPTED` | **`83`/`112`**, `0.9095` (`0.8271`), `0.2805`, `ACCEPTED` | `99`/`123`, `0.8816` (`0.7771`), `0.3354`, `ACCEPTED` |
| **`5: W3b_CoherentIQFT8_B`** (`QFT-8` #2) | `99`/`122`, `0.8796` (`0.7738`), `0.2950`, `ACCEPTED` | **`83`**/`110`, **`0.9122` (`0.8322`)**, **`0.2271`**, `ACCEPTED` | **`83`**/`110`, `0.9107` (`0.8294`), `0.2312`, `ACCEPTED` | `85`/**`108`**, `0.8863` (`0.7856`), `0.2807`, `ACCEPTED` |
| **`6: W4a_CompleteK8_QAOA`** ($K_8$ QAOA) | `206`/`221`, `0.7409` (`0.5489`), `0.5746`, `ACCEPTED` | **`176`/`188`**, `0.7133` (`0.5088`), `0.6119`, `ACCEPTED` | **`176`/`188`**, `0.7045` (`0.4963`), `0.6207`, `INCONCLUSIVE` (`0.649946`) | `182`/`196`, **`0.8206` (`0.6734`)**, **`0.4581`**, `ACCEPTED` |
| **`7: W4b_SpinGlassSK8_QAOA`** (`SK` QAOA) | `206`/`223`, `0.9168` (`0.8404`), `0.3382`, `ACCEPTED` | `186`/**`178`**, `0.9202` (`0.8467`), `0.3180`, `ACCEPTED` | `186`/**`178`**, `0.9176` (`0.8420`), `0.3221`, `ACCEPTED` | **`182`**/`196`, **`0.9252` (`0.8559`)**, **`0.3158`**, `ACCEPTED` |

---

## 3. Campaign 2 Per-Slot Counterbalanced `ABCCBA` Breakdown (`6` Runs, `12` Jobs on `ibm_fez`)

| Slot & Block | Arm | Primary / Confirm Job IDs | PUBs (`W1+W2`) | Physical Shots | Billed `s` (`RU`) | Accepted `(↑)` | Overlap $F_B$ `*(mean / median)*` `(↑)` | `TVD` `*(mean / median)*` `(↓)` | Reported Calibration ID |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Slot 1 (`B1`)** | `Isolated (1-Way)` | `dbuuohmps3fc73bjjb5g`, `dbuuonff8rhs73bn100g` | 16 (`8+8`) | 24,576 | 11.00 s (`0.167892`) | 0 / 8 | 0.6279 / 0.6279 | 0.6652 / 0.6669 | `2026-11-18T14:54:52-03:00` |
| **Slot 2 (`B1`)** | **`Quality-Gated (4-Way)`** | `dbuuovlarooc73fkm1tg`, `dbuup6v82tmc73du4tng` | 4 (`2+2`) | 6,144 | 6.00 s (`0.087857`) | **4 / 8** | **0.6974 / 0.7031** | **0.6048 / 0.5986** | `2026-11-18T14:54:52-03:00` |
| **Slot 3 (`B1`)** | `Unconstrained (8-Way)` | `dbuupblarooc73fkm2d0`, `dbuupfn82tmc73du4u2g` | **2 (`1+1`)** | **3,072** | **5.00 s (`0.072816`)** | 3 / 8 | 0.6924 / 0.6976 | 0.6061 / 0.6015 | `2026-11-18T14:54:52-03:00` |
| **Slot 4 (`B2`)** | `Unconstrained (8-Way)` | `dbuupkmps3fc73bjjcd0`, `dbuupomps3fc73bjjcig` | **2 (`1+1`)** | **3,072** | **5.00 s (`0.072869`)** | 3 / 8 | 0.6879 / 0.6915 | 0.6098 / 0.6085 | `2026-11-18T14:54:52-03:00` |
| **Slot 5 (`B2`)** | **`Quality-Gated (4-Way)`** | `dbuuptf82tmc73du4ul0`, `dbuuq1n82tmc73du4up0` | 4 (`2+2`) | 6,144 | 6.00 s (`0.087484`) | **4 / 8** | **0.6942 / 0.7032** | **0.6077 / 0.5986** | `2026-11-18T14:54:52-03:00` |
| **Slot 6 (`B2`)** | `Isolated (1-Way)` | `dbuuqg782tmc73du4vb0`, `dbuuqkdarooc73fkm3sg` | 16 (`8+8`) | 24,576 | 11.00 s (`0.167928`) | 0 / 8 | 0.6340 / 0.6355 | 0.6589 / 0.6606 | `2026-11-18T14:54:52-03:00` |
