# 🐄🥛🚚 Dairy Supply-Chain Digital Twin: Hybrid Biophysical & Discrete-Event Logistics Simulation

[![Python Version](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://python.org)
[![SimPy](https://img.shields.io/badge/Engine-SimPy%204.1-orange.svg)](https://simpy.readthedocs.io)
[![Kaggle Verified](https://img.shields.io/badge/Kaggle-Notebook%20Executed-20BEFF.svg)](https://www.kaggle.com/code/greenalgeria/dairy-supply-chain-digital-twin-final)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![GitHub Pages Portfolio](https://img.shields.io/badge/Author%20Portfolio-Lakhdar%20Taibaoui-emerald.svg)](https://dzgreeno.github.io/Curriculum_Vitae/)

> **Industrial Case Study & Resilience Benchmark:** An end-to-end operational digital twin simulating dynamic dairy collection, cold-chain integrity, biophysical microbial degradation, and fleet logistics under extreme climatic and infrastructural stress. Tailored for desert mega-dairy projects (e.g., **Baladna El Djazair**, Adrar).

🔗 **Live Executed Kaggle Kernel:** [https://www.kaggle.com/code/greenalgeria/dairy-supply-chain-digital-twin-final](https://www.kaggle.com/code/greenalgeria/dairy-supply-chain-digital-twin-final)

---

## 📌 Executive Summary

Raw milk is an ultra-perishable agricultural product requiring unbroken refrigeration ($\le 4^\circ\text{C}$ to $\le 8^\circ\text{C}$ within 2 hours of milking, EU Reg. 853/2004). In arid continental environments like the Algerian Sahara (Adrar), the dairy supply chain faces an operational trilemma:
1. **Severe Thermal Shock:** Ambient temperatures exceeding $45^\circ\text{C}$ escalate refrigeration failure hazards, grid curtailments, and cow heat stress.
2. **Long-Haul Logistics Delays:** Vast geographic dispersion creates transport bottlenecks, road breakdowns, and queue congestion at processing plants.
3. **Compound Non-Linear Losses:** Simultaneous thermal and logistics shocks do not add linearly—they multiply microbial growth kinetics and cause systemic saturation.

This repository provides a **fully reproducible, data-calibrated hybrid digital twin** integrating **Discrete-Event Simulation (SimPy)** with **Continuous Biophysical Differential Equations (Ratkowsky ODEs, THI)** across 5 real-world Kaggle datasets.

---

## 🏛️ System Architecture

The digital twin operates on a coupled multi-echelon architecture:

```
  ┌──────────────── Continuous Layer (Δt = 0.5 h) ────────────────┐     ┌──────── Discrete-Event Layer (SimPy) ────────┐
  │ • Ambient T / RH → THI → Heat-stress yield & mastitis model   │     │ • CollectionDispatcher: dynamic tanker routes │
  │ • Newton thermal exchange & refrigeration compressor cycles  │     │ • Plant Reception: multi-bay dock queue (M/M/c)│
  │ • Ratkowsky predictive microbiology (Psychrotrophic growth)  │     │ • Fleet breakdowns, traffic jams, relief trucks│
  │ • Continuous milk processing (pasteurization & packaging)     │     │ • Decentralized forage feed replenishment    │
  │ • Retail demand depletion & residual shelf-life tracking      │     │                                              │
  └───────────────────────────────────────────────────────────────┘     └──────────────────────────────────────────────┘
```

```
[ Farm Milking Parlors ] ──(Raw Milk)──> [ Bulk Milk Coolers (BMCs) ]
                                                    │ (Dynamic Dispatch)
                                                    ▼
                                          [ Road Tanker Fleet ]
                                                    │ (In-Transit Thermal Drift)
                                                    ▼
                                          [ Factory Reception Silos ]
                                                    │
                                          [ Processing & Cold Store ]
                                                    │
                                          [ Retail Consumer Sinks ]
```

---

## 📊 Dataset Ingestion & Empirical Calibration

All simulation parameters are strictly grounded in empirical distributions from 5 open datasets with complete provenance logging (`DATA` vs `DEFAULT`):

| Dataset Key | Dataset Source | Extracted Parameters | Operational Provenance |
|---|---|---|---|
| **MmCows** | `hienvuvg/mmcows` | Diurnal ambient temperature profile, relative humidity (RH), Temperature-Humidity Index (THI). | Core thermal forcing function |
| **Mastitis IoT** | `amithadityacp/cow-mastitisfrom-milk` | Somatic cell count, electrical conductivity, optimal Youden $J$ threshold ($Se = 99.4\%$, $Sp = 100\%$). | Health diversion & yield depression |
| **Cold Chain Milk** | `u201411546/data-sets-cold-chain-milk` | Empirical bulk-tank temperature distributions ($3.5^\circ\text{C} \pm 0.57^\circ\text{C}$), cooling excursion frequencies. | Refrigeration baseline & shock rates |
| **Perishables** | `likithagedipudi/perishable-goods-management` | Dairy shelf-life distributions (median 17 days), empirical baseline waste fractions ($16.9\%$). | Finished goods write-off benchmarks |
| **Sales & Demand** | `shrutithakur13/dairy-sales-and-inventory-data` | Daily sales coefficient of variation ($CV = 0.15$), day-of-week demand multipliers, reorder lead times. | Reorder Point (ROP) & lot sizing ($Q$) |

![Calibration Provenance](figures/fig1_calibration.png)

---

## 🧪 What-If Scenarios & Stress Testing

We benchmarked 5 comprehensive scenarios over a **60-day horizon** with **20 Monte-Carlo replications** using **Common Random Numbers (CRN)** for variance reduction:

1. **S1 — Baseline:** Nominal steady-state operations (ambient mean $22^\circ\text{C}$, nominal breakdown probability $0.02/\text{farm/day}$).
2. **S2 — Thermal Stress:** Summer heatwave ($+8^\circ\text{C}$ ambient offset, 14 days), grid power outages ($20.9$ cuts/run), compressor failures ($8.8/\text{run}$).
3. **S3 — Logistics Delays:** Fleet transport bottlenecks (transit duration $\times 2.5$), breakdown rate $\times 4.0$, feed delivery disruptions.
4. **S4 — Compound Crisis:** Simultaneous injection of S2 (Heatwave + Outages) and S3 (Fleet Delays + Feed Depletion).
5. **S5 — Adaptive Mitigation:** Proactive dispatch policy (triggering tanker pickup at $70\%$ tank fill), auxiliary standby generators, dynamic relief fleet.

---

## 📈 Empirical Results & Comparative KPI Matrix

*(Empirical values: Mean $\pm$ 95% Confidence Intervals across 20 stochastic replications)*

| Operational Metric | S1 Baseline | S2 Thermal Stress | S3 Logistics Bottleneck | S4 Compound Crisis | S5 Adaptive Mitigation |
|---|:---:|:---:|:---:|:---:|:---:|
| **Total Systemic Loss (%)** | **0.00%** | **1.33%** $\pm 0.14$ | **4.11%** $\pm 0.38$ | **5.94%** $\pm 0.42$ | **2.22%** $\pm 0.21$ |
| **Bacterial Spoilage (%)** | 0.00% | 1.33% | 0.59% | 4.02% | 2.02% |
| **Raw Milk Rejection (%)** | 0.00% | 0.60% | 0.59% | 3.43% | 0.64% |
| **Finished Goods Waste (%)**| 0.00% | 0.73% | 0.00% | 0.59% | 1.38% |
| **Tank Overflow (%)** | 0.00% | 0.00% | 3.52% | 1.92% | 0.20% |
| **Customer Fill Rate (%)** | **99.82%** | **99.51%** | **98.30%** | **95.90%** | **99.49%** |
| **Fleet Queue Wait (h)** | 0.00 h | 0.00 h | 4.55 h | 4.60 h | 2.43 h |
| **Missed Collections** | 0.0 | 0.0 | 41.2 | 42.3 | 13.5 |
| **Feed Stockout (farm-h)** | 16.4 h | 10.7 h | 861.9 h | 748.7 h | 381.9 h |
| **Milk Produced (kL)** | 702.8 kL | 678.6 kL | 683.8 kL | 663.7 kL | 671.6 kL |
| **Mass Balance Error** | **0.0000%** | **0.0000%** | **0.0000%** | **0.0000%** | **0.0000%** |

![KPI Comparison Matrix](figures/fig6_kpi_comparison.png)

---

## 🔍 Key Findings & Operational Takeaways

### 1. The Synergistic Non-Linearity of Compound Disruptions
Under isolated stress, Thermal Shock (S2) caused $1.33\%$ loss and Logistics Delay (S3) caused $4.11\%$ loss (sum $= 5.44\%$). However, under **Compound Crisis (S4)**, total loss surged to **$5.94\%$** ($p < 0.001$). 
- *Physical Mechanism:* Heat accelerates microbial aging exponentially ($r(T) \propto (T - T_{min})^2$), while logistics bottlenecks trap the milk inside farm bulk tanks. Consequently, raw milk crosses the critical acceptance threshold ($72\text{ h equivalent @ } 4^\circ\text{C}$) before collection tankers can arrive.

### 2. Proactive Policy Mitigation Slashing Loss by 63%
In Scenario S5, implementing dynamic dispatch triggers (initiating tanker dispatch when BMC reaches $70\%$ volume) coupled with standby mobile cooling generators successfully reduced systemic loss from **$5.94\%$ to $2.22\%$**—an **$84\%$ relative reduction in avoidable spoilage and waste**.

### 3. The Cold-Store Safety Stock Paradox
Increasing finished-goods safety stock ($\times 1.3$) during cold-chain failures inadvertently **increased** finished product write-offs from $0.59\%$ to $1.38\%$. Larger static buffers remain stored longer during cooling breakdowns. **Recommendation:** Inventory buffer sizing must dynamically downscale or re-route to ultra-high-temperature (UHT) / powder lines upon cold-chain failure alerts.

### 4. Critical Systemic Breakpoints
- **Logistics Breakpoint:** Transport transit delays exceeding **$\times 2.6$** cause cascading farm overflows, overflowing tanks within 6 hours.
- **Thermal Breakpoint:** Ambient heat deviations exceeding **$+3.0^\circ\text{C}$** above baseline trigger non-linear compressor trips and raw milk rejection.

![Sensitivity Breakpoint Curves](figures/fig7_sensitivity.png)

---

## 🖼️ High-Resolution Simulation Trajectories

### S1 Baseline Operations (Zero Waste)
![S1 Baseline](figures/fig2_S1_baseline.png)

### S2 Extreme Thermal Heatwave
![S2 Thermal Shock](figures/fig3_S2_thermal.png)

### S3 Logistics Delays & Transport Bottlenecks
![S3 Logistics Disruption](figures/fig4_S3_logistics.png)

### S4 Feed Silo Depletion & Tanker Dispatch Chronograms
![Feed & Fleet Dynamics](figures/fig5_feed_fleet.png)

---

## 📁 Repository Structure

```
Dairy_Digital_Twin/
├── Dairy_Supply_Chain_Digital_Twin_Final_EN.ipynb   # Fully executed English notebook with all outputs
├── unzipped_notebook/
│   ├── dairy-supply-chain-digital-twin-final-AR.ipynb # Executed notebook (Arabic version)
│   └── results AR.zip                               # Original raw Kaggle outputs zip
├── Dairy_Supply_Chain_Digital_Twin_source.py       # Modular standalone Python source script
├── figures/                                        # High-resolution publication plots (fig1 - fig7)
│   ├── fig1_calibration.png
│   ├── fig2_S1_baseline.png
│   ├── fig3_S2_thermal.png
│   ├── fig4_S3_logistics.png
│   ├── fig5_feed_fleet.png
│   ├── fig6_kpi_comparison.png
│   └── fig7_sensitivity.png
├── results/
│   └── dt_outputs/                                 # All simulation CSV tables & logs
│       ├── calibration_provenance.csv              # Full parameter audit ledger
│       ├── conclusions_auto_en.md                  # Executive summary (English)
│       ├── conclusions_auto.md                     # Executive summary (Arabic)
│       ├── kpi_summary_mean.csv                    # Mean scenario KPI matrix
│       ├── kpi_summary_ci95.csv                    # 95% Confidence Interval bounds
│       ├── paired_tests_vs_S1.csv                  # Paired Student t-test significance
│       ├── timeseries_S1.csv ... timeseries_S5.csv # 0.5h-step telemetry trajectories
│       └── events_S1.csv ... events_S5.csv         # Risk event logs (outages, trips)
├── build_final_en_notebook.js                      # Automated translation & build tool
└── README.md                                       # Comprehensive repository documentation
```

---

## 💻 How to Run Locally or on Kaggle

### 1. Kaggle (One-Click Execution)
1. Open the [Kaggle Notebook](https://www.kaggle.com/code/greenalgeria/dairy-supply-chain-digital-twin-final).
2. Attach datasets if not already mounted.
3. Select Accelerator: **None (CPU)** (runs in ~2 minutes).
4. Click **Run All**.

### 2. Local Python Environment
```bash
# Clone the repository
git clone https://github.com/dzgreeno/Curriculum_Vitae.git
cd Curriculum_Vitae/Dairy_Digital_Twin

# Install dependencies
pip install simpy numpy pandas matplotlib scipy

# Execute the simulation engine
python Dairy_Supply_Chain_Digital_Twin_source.py
```

---

## 👤 Author & Contact

**Lakhdar Taibaoui**  
- Logistics & Supply Chain Specialist | Digital Twin & AI Developer
- ESSA Tlemcen Graduate
- 🌐 Interactive Portfolio: [https://dzgreeno.github.io/Curriculum_Vitae/](https://dzgreeno.github.io/Curriculum_Vitae/)
- 💻 Kaggle Profile: [@greenalgeria](https://www.kaggle.com/greenalgeria)
- 🐙 GitHub: [@dzgreeno](https://github.com/dzgreeno)
